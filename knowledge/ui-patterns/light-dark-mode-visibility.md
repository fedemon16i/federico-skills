# Light/Dark Mode — Visibility & Contrast Best Practices

**Status:** Done · **Priority:** High  
**Use when:** Designing or auditing any interface that supports both light and dark themes; setting up token architecture for a new design system.

---

## 1. Most Common Visibility Failures in Dark Mode

### Elements that disappear
- **White-background SVG icons** embedded as `<img>`: the icon is visible in light mode, invisible in dark because the fill matches the background.
- **Transparent PNG screenshots / illustrations** with implicit white halos — they weren't cut out properly, the halo is white, and it fuses with the dark background.
- **Disabled state text** styled as `opacity: 0.38` on a light surface looks readable; the same opacity on `#1a1a1a` drops below 1.5:1 — effectively invisible.
- **Placeholder text** (`::placeholder`) inheriting `color: gray` that resolves to #808080 — fine on white, 3.9:1; on dark surfaces with `#1e1e1e` background it's only 5.1:1 — technically passing AA but often perceived as dimmer than intended because the surrounding text is near-white.

### Contrast collapse
- **Borders coded as a fixed hex** (e.g. `border: 1px solid #e2e8f0`): barely visible in light mode, totally invisible in dark where the surface is `#1a1a1a`.
- **Card elevation using `box-shadow: 0 2px 8px rgba(0,0,0,0.12)`**: shadows punch *down* in light mode but are invisible in dark. Dark mode needs *light* glow, not dark shadow, to suggest elevation.
- **Gray-on-gray text blocks**: `color: #6b7280` on `background: #374151` — both are "grays" but the contrast ratio is 3.0:1 (AA fail for body text).
- **Status/badge backgrounds** that were tinted in light mode (`background: #dcfce7` for success) produce pure darkness when the tint is reversed naively to `#14532d` — the white label text over the dark green badge is fine, but the badge bleeds into the dark page surface.

### Shadows that invert wrong
Dark mode shadows should be replaced with subtle *ambient glow* or *surface elevation via lightness* — not inverted to `rgba(255,255,255,0.12)` which reads as bleached. The right approach: raise `background-color` lightness by ~8–12% per elevation step (`--bg-base → --bg-surface → --bg-raised`).

---

## 2. WCAG Contrast Ratios

| Level | Normal text (<18pt / <14pt bold) | Large text (≥18pt / ≥14pt bold) | UI components & graphics |
|-------|----------------------------------|----------------------------------|--------------------------|
| **AA** | **4.5:1** | **3:1** | **3:1** |
| **AAA** | **7:1** | **4.5:1** | — (not defined) |

**Practical minimums Federico uses:**
- Body text: 4.5:1 minimum, 7:1 target.
- Labels, captions, secondary text: 4.5:1 — no exceptions even if "de-emphasized".
- Icon-only controls: 3:1 against adjacent background.
- Placeholder text: 4.5:1 — WCAG 2.1 explicitly excludes placeholder from the exemption.
- Disabled elements: technically exempt from WCAG, but aim for 2.5:1+ to remain usable.

### Tools that check it
| Tool | Use |
|------|-----|
| **Stark** (Figma plugin) | In-canvas audit; checks AA/AAA per selection; great for early design phase |
| **Polychrom** (Figma plugin) | APCA-mode contrast (perceptual) alongside WCAG; shows font-weight thresholds |
| **Chrome DevTools** | Elements panel → Accessibility → Color contrast tooltip; Rendering → Emulate forced-colors |
| **WhitecoatUI** | Browser extension; overlays contrast ratios on live sites |
| **axe DevTools** | Full a11y audit including contrast; CLI/CI-compatible |
| **colour.fyi** | Tokenized contrast matrix — paste a palette, get a grid of all pair ratios |

---

## 3. Token Architecture for Dual-Mode

**Rule:** Never write a color decision in a component. Every color a component uses is a *semantic token* that resolves to a raw value at the theme level.

### Wrong (non-portable)
```css
.card { background: #ffffff; color: #111827; border: 1px solid #e5e7eb; }
```
Dark mode requires a full override — every component becomes a maintenance surface.

### Right (semantic tokens)
```css
/* ─── Raw palette (private — components never reference these) ─── */
:root {
  --_gray-50:  #f9fafb;
  --_gray-100: #f3f4f6;
  --_gray-200: #e5e7eb;
  --_gray-700: #374151;
  --_gray-800: #1f2937;
  --_gray-900: #111827;
  --_gray-950: #0d0f12;

  --_violet-400: #a78bfa;
  --_violet-500: #8b5cf6;
  --_violet-600: #7c3aed;

  --_green-400: #4ade80;
  --_red-400:   #f87171;
}

/* ─── Semantic tokens — light (default) ─── */
:root {
  --bg-base:       var(--_gray-50);
  --bg-surface:    #ffffff;
  --bg-raised:     var(--_gray-100);
  --bg-overlay:    rgba(0, 0, 0, 0.48);

  --text-primary:  var(--_gray-900);
  --text-secondary:var(--_gray-700);
  --text-disabled: var(--_gray-400, #9ca3af);
  --text-on-accent:#ffffff;

  --border-subtle: var(--_gray-200);
  --border-strong: var(--_gray-700);

  --accent:        var(--_violet-600);
  --accent-hover:  var(--_violet-500);
  --accent-muted:  #ede9fe;   /* tinted bg for badges, highlights */

  --status-success:var(--_green-400);
  --status-error:  var(--_red-400);

  --shadow-sm: 0 1px 3px rgba(0,0,0,0.10), 0 1px 2px rgba(0,0,0,0.06);
  --shadow-md: 0 4px 12px rgba(0,0,0,0.10);
}

/* ─── Dark override ─── */
@media (prefers-color-scheme: dark) {
  :root:not([data-theme="light"]) {
    --bg-base:       var(--_gray-950);
    --bg-surface:    var(--_gray-900);
    --bg-raised:     var(--_gray-800);
    --bg-overlay:    rgba(0, 0, 0, 0.72);

    --text-primary:  #f1f5f9;
    --text-secondary:#94a3b8;
    --text-disabled: #4b5563;
    --text-on-accent:#ffffff;

    --border-subtle: rgba(255,255,255,0.08);
    --border-strong: rgba(255,255,255,0.20);

    --accent:        var(--_violet-400);
    --accent-hover:  #c4b5fd;
    --accent-muted:  rgba(139,92,246,0.15);

    /* Elevation via glow, not shadow */
    --shadow-sm: 0 0 0 1px rgba(255,255,255,0.06);
    --shadow-md: 0 4px 24px rgba(0,0,0,0.40);
  }
}

/* ─── Explicit toggle wins over system preference ─── */
:root[data-theme="dark"] {
  /* same values as the @media block above */
}
:root[data-theme="light"] {
  /* same values as the default :root block */
}
```

Components then use only semantic tokens — zero raw hex. Switching themes is a single token swap at the root.

---

## 4. Patterns That Frequently Break

### Borders on dark
`border: 1px solid var(--_gray-200)` is invisible on dark. Use `--border-subtle: rgba(255,255,255,0.08)` — a white-alpha border works across any dark surface without needing to match background color.

### Box-shadows
Dark mode makes drop-shadows vanish (can't cast darkness onto darkness). Replace with:
1. Surface elevation: slightly lighter `background-color` per layer.
2. A 1px inset ring: `box-shadow: 0 0 0 1px rgba(255,255,255,0.08)`.
3. A diffuse dark glow: `box-shadow: 0 8px 32px rgba(0,0,0,0.48)` works because it's dark-on-dark *ambient*, not a directional drop.

### Images with white backgrounds
- PNGs/JPGs that weren't cut to transparent: add `mix-blend-mode: multiply` in light mode (removes white), `mix-blend-mode: screen` or `lighten` in dark mode.
- Better: use SVG with `currentColor` fills, or vector assets exported with transparent backgrounds.
- For screenshots: add a subtle border ring or treat them as "device frame" screenshots with explicit backgrounds.

### SVG fills
SVGs with `fill="#000000"` hardcoded: invisible on dark. Fix:
```css
.icon { fill: currentColor; }   /* SVG inherits text color */
```
Or use `fill: var(--text-primary)`. Never hardcode black or white fills.

### Placeholder text
```css
::placeholder { color: var(--text-disabled); opacity: 1; }
```
Always set `opacity: 1` — Firefox applies `opacity: 0.54` by default, which compounds with a low-contrast color to fail WCAG.

---

## 5. The "System Default" State Problem

Many designers test:
- Explicit light: `data-theme="light"` ✓
- Explicit dark: `data-theme="dark"` ✓

They miss:
- **No `data-theme` attribute at all** — the user's first visit before any toggle fires, or when JS is disabled.

In this state, `prefers-color-scheme` is the only signal. If your CSS only uses `[data-theme="dark"]` selectors and never `@media (prefers-color-scheme: dark)`, users whose OS is in dark mode see your light theme. They won't report a bug — they'll just leave.

**Correct structure (order matters):**
```
:root { /* light defaults */ }
@media (prefers-color-scheme: dark) { :root:not([data-theme="light"]) { /* dark */ } }
:root[data-theme="dark"] { /* explicit dark — overrides media query */ }
:root[data-theme="light"] { /* explicit light — declared for specificity */ }
```

Test the unstyled state by opening the page in a fresh private window with OS dark mode on and DevTools closed — no toggle fired, no `data-theme` set.

---

## 6. How Production Teams Handle Dual-Mode

### Linear
Generates its entire theme from a single hue + algorithmic lightness steps. Dark mode isn't a second palette — it's the same palette, traversed in reverse lightness order. This means contrast ratios are structurally guaranteed by the algorithm, not manually verified per component.

### Vercel
Uses a strict 3-layer token system: **primitives → semantic → component**. Components are allowed to reference only semantic tokens. Their dark mode is a semantic-layer override — the component code never changes. They publish the full token set as CSS custom properties and use them in Tailwind via `@layer base`.

### Supabase
Strong use of `--background-default`, `--foreground-default` naming (influenced by Radix UI / shadcn). Key insight: they distinguish between *interactive* surface colors (buttons, inputs) and *decorative* surface colors (cards, panels) — interactive elements get stricter minimum contrast; decorative elements can be more expressive.

### Stripe
Distinguishes "chrome" (structural UI) from "content" (user-generated / data). Chrome tokens have forced contrast rules. Content areas use a lighter hand to allow brand colors from customer-uploaded assets to breathe without clashing. Their dark mode only applies to chrome — content areas stay intentionally neutral.

---

## 7. Tools Reference

| Tool | What it does |
|------|-------------|
| **Stark** (Figma) | Real-time AA/AAA contrast checking in-canvas |
| **Polychrom** (Figma) | APCA-mode contrast; font-size-aware thresholds |
| **Chrome DevTools → Rendering** | Emulate `prefers-color-scheme` + forced-colors without changing OS settings |
| **Chrome DevTools → Accessibility** | Hover-over contrast tooltip in Elements panel |
| **WhitecoatUI** | Browser overlay; checks live sites for contrast failures |
| **system-colors in CSS** | `ButtonText`, `Canvas`, `CanvasText`, `LinkText` etc. — OS-level color tokens for forced-colors / high-contrast modes; use as fallbacks inside `@media (forced-colors: active)` |
| **axe DevTools** | Full programmatic audit; CI-ready; catches WCAG 2.1 AA failures including contrast |
| **colour.fyi** | Palette → full contrast matrix table |

**`forced-colors` mode pattern:**
```css
@media (forced-colors: active) {
  .badge {
    border: 1px solid ButtonText;  /* system color — always visible */
    background: Canvas;
    color: CanvasText;
  }
}
```
This ensures Windows High Contrast and similar OS accessibility modes don't break your UI.

---

## Quick Checklist Before Shipping a Dark Mode

- [ ] Token architecture: zero raw hex in component CSS
- [ ] `prefers-color-scheme` dark block exists AND is tested without `data-theme` attribute
- [ ] All borders use alpha-white tokens, not light-theme gray hex
- [ ] Box-shadows replaced with glow + surface elevation
- [ ] SVGs use `currentColor` or semantic token, never hardcoded `#000`
- [ ] `::placeholder` has `opacity: 1` and uses `--text-disabled` token
- [ ] Images with implicit white backgrounds handled (transparent, blend mode, or framed)
- [ ] Disabled state text ≥ 2.5:1 contrast (not just "visually dim")
- [ ] Tested with Stark or Polychrom at AA minimum
- [ ] Tested with DevTools → Rendering → `prefers-color-scheme: dark` (unstyled state)
- [ ] Tested with `forced-colors: active` emulation (high-contrast OS mode)

---

*Sources: WCAG 2.1 spec, Radix UI token docs, Linear design engineering blog, Vercel design system, Stripe Elements dark mode implementation notes, MDN forced-colors, Chrome DevTools accessibility docs.*
