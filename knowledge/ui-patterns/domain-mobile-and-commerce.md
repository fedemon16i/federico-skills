# Domain Patterns: Mobile Verticals & Ecommerce

**Status:** Done · **Priority:** High  
**Use when:** Banking/fintech, fitness, education apps, or ecommerce storefronts — don’t invent from consumer landing-page aesthetics.

---

## Mobile banking & fintech

**Trust = restraint.** Color for **financial state**, not decoration.

| Pattern | Practice |
|---------|----------|
| Home verdict | Balance / position + trend first (“am I okay?”) |
| Money in motion | Explicit pending / processing timelines |
| Numerals | Tabular figures, consistent decimals |
| Auth | Biometrics + OTP; minimize password friction |
| KYC | OCR / guided capture; progress visibility |
| Primary CTA | One clear action above the fold (Transfer, Pay) |
| Cards by job | Spend / save / invest grouped; not random bento |
| Security copy | Calm, specific; never alarmist without path |

References in market: calm dashboards (Stripe/Mercury-class clarity), neobank gesture patterns — steal **principles**, not clones.

---

## Fitness / health consumer apps

| Pattern | Practice |
|---------|----------|
| Today-first home | Next workout / streak / recovery — not settings |
| Logging speed | Few taps; defaults from last session |
| Progress | Charts secondary to “what do I do now?” |
| Social | Optional; don’t block core loop |
| Accessibility | Large targets; outdoor readability |

---

## Education (learner / teacher mobile)

| Pattern | Practice |
|---------|----------|
| Role switch | Student vs teacher vs parent — clear context |
| Assignments queue | Due date + status as primary list |
| Offline / bad network | Explicit states for field use |
| Privacy | Minimal PII on shared screens |

---

## Ecommerce (web + mobile web)

Grounded in listing/checkout research (e.g. Baymard-class findings):

| Area | Patterns |
|------|----------|
| **Product list** | Scannable cards/rows; filters users understand; sort that matches intent |
| **Filtering** | Apply without losing place; mobile filter sheet; clear “active filters” |
| **PDP** | Above-fold: image, title, price, rating, variants, CTA, shipping/returns promise |
| **Cart** | Editable line items; total honesty (fees) |
| **Checkout** | One column; express pay early; sticky/collapsed summary on mobile; errors inline |
| **Trust** | Policies near CTA; reviews/Q&A; no dark patterns |

Abandonment often comes from **list/filter failure** and **checkout uncertainty**, not lack of gradients.

---

## Cross-cutting

- Domain patterns **override** generic AI card grids.  
- Pair with `anti-slop-and-motion-tools.md` and project DESIGN.md.  
- Enterprise back-office of the same company → `data-tables-and-rbac.md`.  

*Last updated: 2026-08-22*
