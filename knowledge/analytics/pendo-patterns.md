# Pendo Patterns

**Status:** ADOPT  
**Last updated:** 2026-08-22  
**Context:** Federico has deep practical experience with Pendo (DollarCity, Chek)

---

## Purpose

Capture the high-leverage ways to use Pendo for product decisions, not just for reporting.

---

## Core use cases Federico cares about

1. **Feature adoption** — Who is using what, and how deeply?
2. **Funnels & paths** — Where do users drop off or take unexpected routes?
3. **Guides** — In-app guidance and measurement of their effectiveness
4. **Feedback + NPS** — Connecting qualitative signal to behavioral segments
5. **Retention & cohorts** — Who comes back and who doesn’t

---

## Recommended patterns

### 1. Tagged Features + Adoption
- Tag the real UI elements that matter
- Track first-use and repeat-use
- Segment by role, plan, or acquisition source when possible

### 2. Funnels for critical flows
- Keep funnels short and meaningful (3–6 steps)
- Always look at both conversion rate and absolute volume
- Compare before/after changes

### 3. Guides as measurable interventions
- Measure: guide_view → guide_cta_click → downstream action
- Treat guides as experiments when possible

### 4. NPS / Feedback linked to behavior
- Connect survey responses to actual usage segments
- Look for differences between promoters and detractors in feature usage

---

## Insight format that works

Prefer statements like:

> “X% of users who completed [guide/action] went on to [desired behavior] within [timeframe]. Measured over [date range], n=[cohort size].”

Avoid vague statements such as “engagement improved”.

---

## What to instrument first (priority order)

1. Core task completion events
2. Key feature first-use events
3. Onboarding step completion
4. Guide interactions (if used)
5. Feedback / NPS submissions

---

## Anti-patterns

- Tracking everything “just in case”
- Creating dashboards that no one reviews
- Reporting vanity metrics without context
- Ignoring qualitative feedback that comes through Pendo

---

## Related

- [HEARTS Framework](hearts-framework.md)
- Event Taxonomy & Instrumentation (planned)
- Rapid Validation / Usability Testing
