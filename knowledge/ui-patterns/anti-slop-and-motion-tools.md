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

| Tool | Use |
|------|-----|
| **[Anime.js easing editor](https://animejs.com/easing-editor/elastic/inelastic/)** | Tune elastic/inelastic and other easings visually; export intent, not random bounce |
| Project motion tokens | Duration + easing in DESIGN.md; same language across portfolio/product |
| `prefers-reduced-motion` | Always respect; decorative motion optional |

**Rule:** Motion supports hierarchy and feedback (state change, attention). Not decoration on every card.

---

## Delivery gate (mental checklist)

Before shipping UI from an agent:

1. Could this be any SaaS landing page? → redesign  
2. Does DESIGN.md / brand show up?  
3. Required states (empty, error, loading, permission denied)?  
4. Motion intentional or noise?  
5. Tables/nav follow domain patterns (see domain files), not only pretty cards  

*Last updated: 2026-08-22*
