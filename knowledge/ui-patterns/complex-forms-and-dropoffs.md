# Complex Forms, Validation & Drop-offs (Enterprise)

**Status:** Done · **Priority:** High  
**Use when:** Marketplace publish flows, multi-step configure, serial/codes/equipment fields, approval gates — EY-class pain.

---

## Problem pattern

Publishers (or ops users) start a “simple” product setup, then hit **late discovery** of required fields: codes, serials, equipment, org data. Drop-offs cluster on those steps. Redesigning the *home* or skin does not fix it.

---

## Measure before redesign

| Signal | Action |
|--------|--------|
| Step completion / Pendo funnel | Rank steps by loss |
| Field error rates | Which inputs fail validation most |
| Time on step | Confusion vs abandonment |
| Rage clicks / dead clicks | Mis-hit controls |
| Support tickets by field name | Qualitative map |

**Hypothesis rule:** one primary hypothesis per high-bleed step (not a full IA rewrite).

---

## Design patterns that reduce form death

| Pattern | When |
|---------|------|
| **Progressive disclosure** | Required complexity cannot be deleted; show only what this role needs *now* |
| **Early requirement visibility** | “You’ll need: serial, site, approver” *before* deep investment |
| **Save & resume** | Enterprise sessions are interrupted |
| **Inline validation** | On blur / as-you-go; never only on final Submit |
| **Human field labels + examples** | Codes/serials need format examples |
| **Grouped related fields** | Equipment block together; don’t scatter |
| **Role-aware forms** | Publisher vs approver vs consumer see different density |
| **Error summary + jump links** | Long forms: list errors at top with anchors |
| **Don’t punish optional** | Mark optional clearly; don’t block on nice-to-haves |

---

## What not to do

- One “short form” that violates compliance/approver rules  
- Cosmetic redesign of the hub while configure still bleeds  
- Inventing success metrics when only directional evidence exists  

---

## Link to Federico stack

- Analytics: Pendo patterns, event taxonomy, HEARTS  
- Research: rapid validation, interviews  
- Portfolio: ownership of measurement loop + form UX proposals  
- Modernize experience vs skin  

*Last updated: 2026-08-23*
