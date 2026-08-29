# Product UI Animation Showcase

**Status:** Done · **Priority:** High  
**Use when:** Building an animated product demo, showcase page, or portfolio beat sequence. Prevents LLM hallucination around easing, sequencing, and tool choice.

---

## The two-phase rule (non-negotiable)

1. **RECONSTRUCT** — DOM parity first. Build the real UI structure (table, form, dashboard) before any animation exists. Confirm all data is accurate and layout is right.
2. **ORCHESTRATE** — Animation second. Once the DOM is correct, add motion on top. Never design the animation before the UI exists.

Violating this = animating placeholder markup that drifts from the real product.

---

## One-beat rule

Each beat has **one thing that moves**. Everything else is static.

The motion answers: *"what happened here?"* — not *"can I animate this?"*

Anti-pattern: every element bouncing in independently = demo reel. Motion without meaning.

---

## Animation vocabulary (8 types)

| Type | When | Easing |
|------|------|--------|
| `reveal` | Element enters viewport | `ease-out` or `var(--spring)` if it grows |
| `cursor-move` | Simulated pointer path | `easeInOutSine` (Anime.js) |
| `state-change` | Toggle, tab switch, filter apply | `ease-out` 200–300ms |
| `emphasis` | Pulse, scale pop on the key element | `var(--spring)` |
| `zoom` | Focus on a region of UI | `var(--spring)`, `transform-origin` anchored |
| `scene-transition` | Beat → beat switch | cross-fade 300–500ms or View Transitions API |
| `counter` | KPI number counting up | `easeOutCubic` rAF loop, ~1400ms |
| `stagger` | Row-by-row or card-by-card reveal | 60–100ms per item delay |

---

## Easing rules

```css
/* GROW / EXPAND / EMPHASIZE → spring (has overshoot, never flat) */
--spring: cubic-bezier(.34, 1.56, .64, 1);

/* FADE / SLIDE IN → ease-out */
transition: opacity 300ms ease-out, transform 300ms ease-out;

/* CURSOR PATH → sine in-out */
/* Set in anime.js: easing: 'easeInOutSine' */

/* ZOOM → NEVER use CSS `zoom:` property (participates in layout, not animatable) */
/* ALWAYS use: transform: scale(N); transform-origin: X% Y%; */
```

---

## Tool selection

| Tool | Use case | CDN |
|------|----------|-----|
| **CSS native** | Simple reveals, single-element transitions | none |
| **Motion One `inView()`** | Scroll-triggered reveals | `cdn.jsdelivr.net/npm/motion@10.16.4/dist/motion.js` |
| **Anime.js `anime.timeline()`** | 6+ sequential steps, staggers, cursor paths | `cdnjs.cloudflare.com/ajax/libs/animejs/3.2.2/anime.min.js` |
| **View Transitions API** | Shared-element morphing between beats | native, Chrome 114+, Firefox 129+ |
| **CSS scroll-driven** | Reveal on scroll, no JS | native, Chrome 115+, Firefox 125+ |
| **GSAP** | DON'T — license, bundle size, overkill for portfolio |
| **Framer Motion** | DON'T — requires React |

---

## Feature isolation timing

Elements must **settle** before the next element begins.

```js
// WRONG — overlap during overshoot:
// el1 starts at 0ms, el2 at 100ms — el2 starts while el1 is still overshooting

// CORRECT — wait for settle:
anime.timeline()
  .add({ targets: '.el1', scaleX: [0,1], duration: 400, easing: 'spring(1,80,10,0)' })
  .add({ targets: '.el2', opacity: [0,1], duration: 300 }, '+=200'); // 200ms AFTER el1 rests
```

Spring easing rests at ~550ms for `duration: 400`. Next element begins at 600ms.

---

## Code patterns (ready to use)

### Staggered row reveal
```js
const rows = stage.querySelectorAll('.data-row');
rows.forEach((el, i) => {
  el.style.opacity = '0';
  el.style.transform = 'translateY(8px)';
  setTimeout(() => {
    el.style.transition = 'opacity 300ms ease-out, transform 300ms ease-out';
    el.style.opacity = '1';
    el.style.transform = 'none';
  }, i * 80);
});
```

### Animated counter (KPI tile)
```js
function animateCounter(el, from, to, duration = 1400) {
  const start = performance.now();
  requestAnimationFrame(function tick(now) {
    const t = Math.min((now - start) / duration, 1);
    const ease = 1 - Math.pow(1 - t, 3);
    el.textContent = Math.round(from + (to - from) * ease).toLocaleString();
    if (t < 1) requestAnimationFrame(tick);
  });
}
```

### Anime.js timeline for complex beats
```js
anime.timeline({ easing: 'easeOutExpo', duration: 400 })
  .add({ targets: '.kpi-tiles .tile', opacity: [0,1], translateY: [10,0], delay: anime.stagger(80) })
  .add({ targets: '.cursor-dot', translateX: 180, duration: 500, easing: 'easeInOutSine' }, '+=200')
  .add({ targets: '.drill-panel', scaleY: [0,1], transformOrigin: 'top center', easing: 'spring(1,80,10,0)' });
```

### View Transitions (beat-to-beat morphing)
```js
el.style.viewTransitionName = 'beat-panel';
document.startViewTransition(() => {
  showNextBeat();
});
```
```css
::view-transition-old(beat-panel),
::view-transition-new(beat-panel) {
  animation-duration: 300ms;
  animation-timing-function: cubic-bezier(.34,1.56,.64,1);
}
```

### CSS scroll-driven reveal (no JS)
```css
@keyframes reveal {
  from { opacity: 0; transform: translateY(12px); }
  to   { opacity: 1; transform: none; }
}
.beat-row {
  animation: reveal linear both;
  animation-timeline: view();
  animation-range: entry 0% entry 40%;
}
```

---

## Pacing rules for beats

| Element | Duration |
|---------|----------|
| Problem frame (static) | 3–5s |
| One beat | 4–8s max |
| Annotation callout visible | 2–4s |
| Beat-to-beat transition | 0.3–0.5s |
| Total per showcase section | < 90s |

If a beat is longer than 12s, split it into two beats.

---

## prefers-reduced-motion (non-negotiable)

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: .01ms !important;
    transition-duration: .01ms !important;
  }
}
```

Or at the JS level: check `window.matchMedia('(prefers-reduced-motion: reduce)').matches` before starting any timeline.

---

## DOM reconstruction reference sources

For building real dashboard DOM (not AI-invented):
- **Tabler** (github.com/tabler/tabler) — KPI tiles, data tables, activity feeds. 41k stars, vanilla JS. Study HTML structure only.
- **Federico's own project screenshots** — ground truth for reconstruction; never invent layout

*Last updated: 2026-08-29*
