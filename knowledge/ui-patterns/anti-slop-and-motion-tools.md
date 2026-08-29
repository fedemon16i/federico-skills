# Anti-Slop Design Skills & Motion Tools

**Status:** Done · **Priority:** High  
**Use when:** Generating or reviewing UI so it doesn’t look generic “AI”; choosing motion with intent.

---

## What “anti-slop” means

Avoid **on-distribution** UI: Inter/Arial defaults, purple gradients, identical card grids, glassmorphism decoration, fake metrics, layouts that could belong to any product.

**Filter vs direction:** Anti-slop stops generic output. **DESIGN.md / brand** supplies the positive direction. Without direction, “clean” still feels sterile.

---

## Skills & sources (curated)

| Resource | Role | Notes |
|----------|------|--------|
| **Anthropic `frontend-design`** | Official craft skill | Typography, color conviction, anti–AI-slop aesthetic; already in Federico’s install chain |
| **[design-anti-slop](https://mcpmarket.com/es/tools/skills/design-anti-slop)** (emraher) | MCP Market listing | Authenticity; kill cookie-cutter layouts/gradients |
| **[miqdadbadjuber/anti-slop](https://github.com/miqdadbadjuber/anti-slop)** | Rules filter (not a style guide) | Hard gates + purpose-gates + delivery PASS/FAIL; pairs with **your** DESIGN.md; modular skills (ui, copy, mobile, a11y) |
| **anti-ui-slop / UIZZE-style** | Reference real screens | Design contract from real product UI; reject generic patterns |
| **Impeccable / Pro Max–class** (ecosystem) | Deeper anti-pattern lists | Use only if they add signal beyond frontend-design — don’t stack duplicates |

**Federico default:** `frontend-design` + project **DESIGN.md** + **UI Integrity**. Add a stricter anti-slop pack only if still shipping generic UI.

---

## Motion tools (practical)

| Tool | Use | CDN |
|------|-----|-----|
| **CSS native** | Simple reveals, fade, single-element transitions | none |
| **[Anime.js](https://animejs.com)** `anime.timeline()` | 6+ sequential steps, staggers, cursor paths, spring easing | `cdnjs.cloudflare.com/ajax/libs/animejs/3.2.2/anime.min.js` |
| **[Motion One](https://motion.dev)** `inView()` + `scroll()` | Scroll-triggered reveals; already in CLAUDE.md CDN | `cdn.jsdelivr.net/npm/motion@10.16.4/dist/motion.js` |
| **View Transitions API** (native) | Shared-element morphing between beats/views | Chrome 114+, Firefox 129+, no library |
| **CSS scroll-driven** (native) | Reveal on scroll without JS; `animation-timeline: view()` | Chrome 115+, Firefox 125+ |
| **Anime.js easing editor** | Tune elastic/inelastic visually, export to code | animejs.com/easing-editor |

**Grow/expand/emphasis → `var(--spring)` = `cubic-bezier(.34,1.56,.64,1)`** — must have overshoot. Never use a flat ease for anything that changes size.  
**Zoom → `transform: scale(N)` always. NEVER CSS `zoom:` — it participates in layout and is not animatable.**  
**Reduced motion** is non-negotiable. See `product-ui-animation-showcase.md` for the media query.

**Rule:** Motion supports hierarchy and feedback (state change, attention). Not decoration on every card.

→ Full animation methodology: `product-ui-animation-showcase.md`

---

## Delivery gate (mental checklist)

Before shipping UI from an agent:

1. Could this be any SaaS landing page? → redesign  
2. Does DESIGN.md / brand show up?  
3. Required states (empty, error, loading, permission denied)?  
4. Motion intentional or noise?  
5. Tables/nav follow domain patterns (see domain files), not only pretty cards  

*Last updated: 2026-08-29*
