---
name: accessibility-standards
description: Apply global accessibility standards to UI components and copy. Use when building any interface, reviewing contrast, writing alt text, handling focus management, or documenting accessibility in case studies. Based on WCAG 2.2 AA (international), ADA (USA), EN 301 549 (EU), AODA (Canada). Activates for any HTML/CSS work, design review, or accessibility audit mention.
---

# Accessibility Standards Skill

## Legal Framework Reference

| Law | Region | Standard | Applies To |
|---|---|---|---|
| **WCAG 2.2 AA** | International | W3C | Baseline for everything |
| **ADA Title III** | USA | WCAG 2.1 AA minimum | Public-facing digital products |
| **EN 301 549** | EU | WCAG 2.1 AA + extras | Public sector, procurement |
| **AODA WCAG** | Canada | WCAG 2.0 AA | Ontario businesses |
| **Section 508** | USA (federal) | WCAG 2.1 AA | Government contractors |

**Safe default**: Build to **WCAG 2.2 AA**. This satisfies all of the above simultaneously.

WCAG 2.2 AA failure is not just a legal risk — it excludes: 2.2 billion people with visual impairment globally, elderly users (presbyopia affects ~50% of people 50+), users in bright sunlight, users on low-quality screens.

---

## The POUR Principles

Every interface must be:
- **Perceivable** — users can perceive all information (vision, hearing alternatives)
- **Operable** — all functionality works without a mouse
- **Understandable** — content and UI behavior are predictable
- **Robust** — works with assistive technology (screen readers, switches)

---

## Color & Contrast (Perceivable)

### Contrast Ratios — WCAG 2.2 AA

| Text Type | Minimum Ratio | Notes |
|---|---|---|
| Body text (< 18px or < 14px bold) | **4.5:1** | Most text falls here |
| Large text (≥ 18px or ≥ 14px bold) | **3:1** | Headlines, large labels |
| UI components & focus indicators | **3:1** | Borders, icons, input outlines |
| Decorative content | No requirement | Pure decoration only |

### AAA (best effort for elderly/low vision)

| Text Type | AAA Ratio |
|---|---|
| Body text | 7:1 |
| Large text | 4.5:1 |

### Font Weight Rules

- **Minimum body weight: 400** (regular). Never use 300 (light) or 100–200 for any readable text.
- At small sizes (< 14px): use 500–600 minimum
- Thin typography (`font-weight: 100–300`) is a WCAG failure for most text sizes

```css
/* ✅ Safe minimum */
body {
  font-weight: 400;
  font-size: 1rem; /* 16px base */
  line-height: 1.5; /* WCAG requires 1.5 for body */
}

/* ❌ Never for readable text */
.subtitle { font-weight: 300; font-size: 12px; } /* fails 4.5:1 + readability */
```

### Tools to Check Contrast

- **Browser**: DevTools → Accessibility → Color contrast
- **Online**: webaim.org/resources/contrastchecker
- **Figma plugin**: Contrast (by Stark) or Able

---

## Typography for Low Vision & Elderly Users

```css
/* Minimum viable readable typography */
body {
  font-size: 1rem;        /* 16px — never smaller for body */
  line-height: 1.5;       /* WCAG SC 1.4.12 */
  letter-spacing: 0.12em; /* Optional but helps readability */
  font-weight: 400;
}

/* Allow user zoom — NEVER disable */
/* ❌ NEVER: <meta name="viewport" content="user-scalable=no"> */
/* ✅ Always: <meta name="viewport" content="width=device-width, initial-scale=1"> */
```

### Text Spacing (WCAG 1.4.12)

Content must not break when users override:
- Line height to 1.5× font size
- Paragraph spacing to 2× font size
- Letter spacing to 0.12× font size
- Word spacing to 0.16× font size

**Test**: Apply this CSS and check nothing overlaps or clips:
```css
* {
  line-height: 1.5 !important;
  letter-spacing: 0.12em !important;
  word-spacing: 0.16em !important;
}
p { margin-bottom: 2em !important; }
```

---

## Focus Management (Operable)

```css
/* ✅ Always visible focus — never suppress */
:focus-visible {
  outline: 3px solid var(--color-focus, #005FCC);
  outline-offset: 2px;
  border-radius: 2px;
}

/* Remove only for mouse users via :focus (not :focus-visible) */
:focus:not(:focus-visible) {
  outline: none;
}

/* ❌ NEVER */
* { outline: none; } /* keyboard users become completely lost */
button:focus { outline: 0; }
```

### Focus Order
- Must follow logical reading order (top-left → bottom-right, or DOM order)
- Modals and drawers: trap focus inside when open
- Close button: return focus to the trigger element on close
- Tab order never skips interactive elements

---

## Interactive Elements (Operable)

### Touch Target Size (WCAG 2.5.5 — AA in 2.2)
- Minimum: **44×44px**
- Recommended: **48×48px** (Material Design / Apple HIG)

```css
.btn, a, [role="button"] {
  min-height: 44px;
  min-width: 44px;
  display: inline-flex;
  align-items: center;
  justify-content: center;
}
```

### Keyboard Navigation
Every interactive element must be reachable and operable via:
- `Tab` / `Shift+Tab` — navigate
- `Enter` / `Space` — activate buttons, checkboxes
- `Arrow keys` — navigate within components (menus, tabs, radio groups)
- `Escape` — close overlays, cancel actions

---

## Semantic HTML (Robust)

Use the right element for the job. Screen readers use HTML semantics.

```html
<!-- ✅ Semantic -->
<button type="button">Submit</button>
<nav aria-label="Main navigation">...</nav>
<main>...</main>
<h1>Page title</h1> <!-- one per page -->
<table>
  <caption>Transaction history</caption>
  <thead><tr><th scope="col">Date</th></tr></thead>
</table>

<!-- ❌ Div-soup -->
<div class="button" onclick="...">Submit</div>
<div class="nav">...</div>
```

### Landmark Roles
Every page must have:
- `<header>` or `role="banner"` — one per page
- `<main>` or `role="main"` — one per page
- `<nav>` — with `aria-label` if multiple navs
- `<footer>` or `role="contentinfo"`

---

## Images & Media (Perceivable)

```html
<!-- Informative image -->
<img src="chart.png" alt="Bar chart showing 40% increase in onboarding completion after API integration, Q3 2023">

<!-- Decorative image — empty alt, not missing alt -->
<img src="divider.svg" alt="">

<!-- Complex image (charts, diagrams) -->
<figure>
  <img src="funnel.png" alt="Pendo funnel showing 3-step onboarding">
  <figcaption>Step 3 had 68% drop-off before the SERFINSA API integration.</figcaption>
</figure>
```

Alt text rules:
- Describe the content AND function, not the appearance
- Don't say "image of..." or "picture of..."
- For charts: state the key finding, not just "bar chart"
- Max ~125 characters; longer descriptions use `aria-describedby` or `<figcaption>`

---

## Forms (Understandable)

```html
<!-- ✅ Every input needs a visible label -->
<label for="email">Email address</label>
<input type="email" id="email" name="email" 
       aria-describedby="email-hint"
       autocomplete="email">
<div id="email-hint">We'll send your statement here.</div>

<!-- Error message -->
<div role="alert" aria-live="polite">
  Please enter a valid email address.
</div>
```

- Never use `placeholder` as the only label — it disappears on focus
- Error messages: adjacent to the field, associated via `aria-describedby`
- Required fields: `aria-required="true"` + visible indicator (not color-only)

---

## Motion & Animation (Perceivable)

```css
/* Always wrap animations in this query */
@media (prefers-reduced-motion: no-preference) {
  .animated-element {
    transition: transform 0.3s ease, opacity 0.3s ease;
    animation: slideIn 0.4s ease;
  }
}

/* Reduced motion users get instant transitions */
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

WCAG 2.3.3: any animation triggered by interaction must be disableable.

---

## Color Alone (WCAG 1.4.1)

Never use color as the ONLY way to convey information.

```html
<!-- ❌ Only color distinguishes error -->
<input style="border-color: red">

<!-- ✅ Color + icon + text -->
<input aria-invalid="true" aria-describedby="err1">
<div id="err1" role="alert">
  <span aria-hidden="true">⚠</span> Invalid email format
</div>
```

Status badges: always pair color with text or icon.

---

## Accessibility Audit Checklist (WCAG 2.2 AA)

### Automated (catch ~30–40% of issues)
- [ ] Run axe DevTools or Lighthouse accessibility audit
- [ ] 0 critical violations

### Manual (the rest)
- [ ] Keyboard: navigate entire page without mouse
- [ ] Screen reader: test with VoiceOver (iOS/Mac) or NVDA (Windows)
- [ ] Zoom to 200%: no content loss or horizontal scroll
- [ ] Color: pass all contrast checks
- [ ] Motion: test with `prefers-reduced-motion` enabled
- [ ] Forms: all inputs have visible labels
- [ ] Images: all non-decorative have meaningful alt text
- [ ] Focus: visible at all times, logical order
- [ ] Touch targets: all ≥ 44×44px

---

## Quick Reference: Most Common Failures

| Failure | Fix |
|---|---|
| `outline: none` on focus | Use `:focus-visible` pattern |
| Low contrast text | Darken text or lighten background to meet ratio |
| `font-weight: 300` on small text | Use 400 minimum |
| Missing form labels | `<label for="id">` + matching `id` |
| `<div>` as button | Use `<button>` |
| Color-only status | Add text/icon alongside color |
| `user-scalable=no` | Remove — never block zoom |
| Missing alt on img | Add descriptive alt or `alt=""` for decorative |
| Animations no reduced-motion | Wrap in `prefers-reduced-motion: no-preference` |
| Touch target < 44px | Add padding to reach minimum |
