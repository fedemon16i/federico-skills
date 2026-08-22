# HEARTS Framework

**Status:** ADOPT  
**Last updated:** 2026-08-22  
**Origin:** Google  
**Federico context:** Already used at Chek

---

## Purpose

A practical framework to choose what to measure in product analytics so that metrics stay connected to user experience and business outcomes.

---

## The dimensions

| Letter | Dimension | What it measures | Typical sources |
|--------|-----------|------------------|-----------------|
| **H** | Happiness | Satisfaction, delight, NPS, CSAT | Qualtrics, in-app surveys, Pendo NPS |
| **E** | Engagement | Depth and frequency of use | Pendo, Mixpanel session/feature data |
| **A** | Adoption | First-time use and onboarding success | Pendo first-use events, funnels |
| **R** | Retention | Whether users return over time | Cohort analysis (D7, D30) |
| **T** | Task Success | Ability to complete core tasks | Funnels, error rates, time-on-task |
| **S** | (sometimes used as) | — | — |

Note: Some versions stop at HEART. The extra S is less standardized.

---

## How Federico uses it

1. Map each major product area or feature to the most relevant HEART dimensions.
2. Choose 1–2 primary metrics per dimension (avoid metric sprawl).
3. Instrument those metrics properly.
4. Review them regularly and connect changes back to product decisions.

---

## Example mapping (illustrative)

| Product area | Primary HEART focus |
|--------------|---------------------|
| Onboarding | Adoption + Task Success |
| Core feature | Engagement + Task Success |
| New capability | Adoption first, then Engagement |
| Support / Help | Happiness + Task Success |

---

## Rules of thumb

- Never track a metric just because the tool makes it easy.
- Pair volume metrics with rate metrics when possible.
- Always note sample size, date range, and segment.
- Prefer metrics that can influence a decision.

---

## Related

- Pendo Patterns (to be expanded)
- Event Taxonomy & Instrumentation
- Rapid Validation / Usability Testing (for Task Success qualitative side)
