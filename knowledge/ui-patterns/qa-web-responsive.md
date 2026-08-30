# QA for Web — Responsive, A11y, Visual Regression

> Context: pure HTML+CSS+JS portfolio, no build step, no framework.
> Playwright + axe-core already wired. This doc expands from that base.

---

## Tool Stack — Prioritized

| Tool | Purpose | When to run |
|---|---|---|
| **Playwright** (already installed) | Visual regression screenshots + multi-viewport | Every commit that touches CSS or layout |
| **@axe-core/playwright** (already installed) | WCAG 2.1 A/AA automated scan | Every commit that touches HTML structure or color |
| **Lighthouse CI** (`@lhci/cli`) | CWV scores, performance budget, SEO, a11y second pass | Weekly or before sharing the portfolio |
| **pa11y** | Standalone a11y CLI — faster to one-off a single URL | Spot checks during dev, before axe full suite |
| **Chromatic** | Visual diff with a hosted baseline + review UI | Only if you want a shareable diff link for a reviewer |
| **BackstopJS** | Visual regression with pixel-level diff reports | Alternative to Playwright screenshots if you want HTML reports |

**Verdict for this stack:** Playwright + axe covers 90% of what matters solo. Add `@lhci/cli` for CWV. Skip BackstopJS and Chromatic unless you have a reviewer who needs the diff UI.

---

## Pre-Ship Checklist

### Layout & Responsive

- [ ] No horizontal scroll at any of the 6 breakpoints (320 / 375 / 768 / 1024 / 1440 / 1920) — `document.body.scrollWidth === window.innerWidth`
- [ ] Text never touches a border or edge — minimum 16px margin on 320px viewport
- [ ] Images use `object-fit: cover` and have consistent heights within any grid row
- [ ] Grid collapses to 1 column below 768px; padding reduces at 480px
- [ ] Nav hamburger menu opens and closes correctly on touch (not inside the pointer:fine block)
- [ ] No fixed-height elements causing truncated text on small viewports

### Accessibility

- [ ] `<a class="skip-link" href="#main">` is first child of `<body>` on every page
- [ ] All `<img>` tags have descriptive `alt` text (not empty, not "image of")
- [ ] Nav dropdown has `aria-expanded`, `aria-haspopup`, `role="menu"`, `role="menuitem"`
- [ ] Color contrast passes WCAG AA: 4.5:1 for body text, 3:1 for large text and UI components
- [ ] `axe` returns zero violations on `wcag2a` + `wcag2aa` tags

### Animation & Interaction

- [ ] `prefers-reduced-motion` suppresses all transitions (the `*,*::before,*::after` rule is in place)
- [ ] Cards that grow on hover use `cubic-bezier(.34,1.56,.64,1)` — confirm in DevTools, not by feel
- [ ] Hover cards with size change have `pointerleave` debounce (~70ms)
- [ ] No gradient/glow/neon in card resting state — only on `:hover`

### Performance

- [ ] Lighthouse Performance score ≥ 90 on desktop, ≥ 75 on mobile
- [ ] LCP < 2.5 s, CLS < 0.1, INP < 200 ms (Core Web Vitals thresholds)
- [ ] Images are WebP or AVIF where possible; no uncompressed JPEG > 300 KB
- [ ] No render-blocking resources (fonts load with `display=swap`, scripts use `defer`)

### Cross-Browser Smoke Check

- [ ] Chrome/Chromium (primary — Playwright covers this)
- [ ] Safari/WebKit — one manual pass on iOS Safari for touch events and viewport meta
- [ ] Firefox — visual pass on layout only (market share ~3-4% but catches flexbox edge cases)

---

## Playwright — 6-Viewport Pattern

This is what `playwright.config.ts` already implements. Reference snippet for new test files:

```typescript
import { test, expect } from '@playwright/test';
import AxeBuilder from '@axe-core/playwright';

// Viewports are declared in playwright.config.ts projects[]:
// 320x568 | 375x667 | 768x1024 | 1024x768 | 1440x900 | 1920x1080

const routes = [
  { name: 'home',   path: '/' },
  { name: 'about',  path: '/about.html' },
  { name: 'resume', path: '/resume.html' },
];

for (const { name, path } of routes) {
  test(`${name} — visual regression`, async ({ page }) => {
    await page.goto(path);
    // networkidle may never fire on animated pages — catch and continue
    await page.waitForLoadState('networkidle', { timeout: 8000 }).catch(() => {});
    await page.emulateMedia({ reducedMotion: 'reduce' });
    await expect(page).toHaveScreenshot(`${name}.png`, { fullPage: true });
  });

  test(`${name} — accessibility`, async ({ page }) => {
    test.setTimeout(60000);
    await page.goto(path);
    await page.waitForLoadState('domcontentloaded');
    // colorScheme: dark ensures @media prefers-color-scheme: dark applies
    await page.emulateMedia({ reducedMotion: 'reduce', colorScheme: 'dark' });
    await page.waitForTimeout(500); // let theme JS apply data-theme
    const results = await new AxeBuilder({ page })
      .withTags(['wcag2a', 'wcag2aa'])
      .analyze();
    expect(results.violations).toEqual([]);
  });
}

// Pages with continuous animation — use domcontentloaded + delay, not networkidle
const animated = [
  { name: 'ey-fabric', path: '/projects/ey-fabric.html' },
];

for (const { name, path } of animated) {
  test(`${name} — visual regression`, async ({ page }) => {
    await page.goto(path);
    await page.waitForLoadState('domcontentloaded');
    await page.waitForTimeout(800); // let first animation frame settle
    await page.emulateMedia({ reducedMotion: 'reduce' });
    await expect(page).toHaveScreenshot(`${name}.png`, { fullPage: true });
  });
}
```

**Commands:**

```bash
# Serve + run all tests
npm run serve &
npm test

# Run only one viewport
npx playwright test --project=375

# Update baselines after an intentional design change
npm run test:update

# Run with UI (shows video, trace, diff)
npm run test:ui
```

---

## Lighthouse CI — Quick Setup

```bash
npm install -D @lhci/cli

# lhci autorun (one-off against the running server)
npx serve . --listen 8080 &
npx lhci autorun \
  --collect.url=http://localhost:8080 \
  --collect.url=http://localhost:8080/projects/ey-fabric.html \
  --assert.assertions.categories:performance=["warn",{"minScore":0.9}] \
  --assert.assertions.categories:accessibility=["error",{"minScore":0.95}] \
  --assert.assertions.first-contentful-paint=["warn",{"maxNumericValue":2500}] \
  --assert.assertions.cumulative-layout-shift=["error",{"maxNumericValue":0.1}]
```

Target scores: Performance ≥ 90 (desktop) / ≥ 75 (mobile), Accessibility ≥ 95, Best Practices 100, SEO 100.

---

## Cross-Browser Priority in 2026

| Browser | Priority | Why |
|---|---|---|
| Chrome/Chromium | Critical | ~65% global share; Playwright default |
| Safari (iOS) | Critical | ~19% share; only engine on iPhone; test touch + viewport meta |
| Firefox | Low | ~3-4% share; test once before launch, skip in CI |
| Samsung Internet | Skip | Chromium-based; covered by Chrome tests |
| Edge | Skip | Chromium-based; cosmetically identical to Chrome |
| Opera | Skip | Chromium-based |

**Safari-specific things to check manually:**
- `position: sticky` inside `overflow: hidden` containers — Safari breaks this
- `100vh` sizing — use `svh`/`dvh` or a JS-based fix for mobile Safari toolbar
- `env(safe-area-inset-*)` for notch/home indicator — add to body padding if using full-bleed layouts

---

## Mobile Interaction Testing

**Viewport meta (required on every page):**
```html
<meta name="viewport" content="width=device-width, initial-scale=1">
```

**Safe area insets (if using fixed bottom bars):**
```css
padding-bottom: max(1rem, env(safe-area-inset-bottom));
```

**Touch event checklist:**
- Touch targets ≥ 44×44px (iOS HIG) / ≥ 48×48px (Material)
- No `:hover`-only interactions — anything hover must also work on tap
- No `pointer:fine`-gated logic for functionality that must work on touch (hamburger menu, tap-to-expand)
- Scroll areas use `-webkit-overflow-scrolling: touch` only if you need momentum; otherwise omit

**Quick mobile smoke test (no device needed):**
```bash
# DevTools → Toggle device toolbar → iPhone SE (375px) + iPhone 14 Pro Max (430px)
# Then rotate both — check landscape layout doesn't break
```

---

## CWV — What to Aim For

| Metric | Good | Needs Work | Poor |
|---|---|---|---|
| LCP (Largest Contentful Paint) | < 2.5 s | 2.5–4 s | > 4 s |
| CLS (Cumulative Layout Shift) | < 0.1 | 0.1–0.25 | > 0.25 |
| INP (Interaction to Next Paint) | < 200 ms | 200–500 ms | > 500 ms |
| FCP (First Contentful Paint) | < 1.8 s | 1.8–3 s | > 3 s |
| TTFB | < 800 ms | 800 ms–1.8 s | > 1.8 s |

**Biggest CLS risk in this stack:** images without explicit `width`/`height` attributes. Always set both on `<img>` or use `aspect-ratio` in CSS to reserve space before load.

**Biggest LCP risk:** hero images served as JPEG > 300 KB. Convert to WebP, add `loading="eager"` + `fetchpriority="high"` on the hero image only.
