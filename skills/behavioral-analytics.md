---
name: behavioral-analytics
description: Apply behavioral analytics frameworks and tool-specific patterns. Use when analyzing user behavior data, designing analytics instrumentation, interpreting Pendo/Mixpanel/GA4 data, writing analytics sections in case studies, or building analytics dashboards.
---

# Behavioral Analytics Skill

Federico is a UX & Product Experience Analyst specializing in behavioral analytics. He has worked with Pendo at DollarCity and Chek, and uses Qualtrics for survey/feedback layers.

## Core Principle

**Never invent metrics.** If data isn't available, use explicit placeholders:
- `[RETENTION RATE — PLACEHOLDER]`
- `[EVENT COUNT — SOURCE: PENDO, DATE RANGE NEEDED]`

## Frameworks

### HEARTS (Google)
- **H**appiness → Qualtrics CSAT/NPS
- **E**ngagement → feature adoption, session frequency
- **A**doption → first-use events, onboarding completion
- **R**etention → cohort return (D7/D30)
- **T**ask Success → funnel completion, error rate, time-on-task

### Event Taxonomy
Format: `[Object]_[Action]_[Context]`

## Tool patterns (summary)

- **Pendo**: guides, paths, funnels, NPS, tagged features
- **Mixpanel**: event-based funnels, retention, “did A also did B”
- **GA4**: traffic + basic behavior, custom dimensions for UX
- **Qualtrics**: surveys linked to behavioral segments
- **Maze**: unmoderated usability metrics (success, time, misclicks)

## Strong insight format

> “X% of users who completed [action] went on to [desired behavior] within [timeframe]. Measured over [date range], n=[cohort size].”

## Red flags

- Correlation presented as causation without testing
- Missing sample size or date range
- Vanity metrics without context
- Single metric with no pair (volume + rate, engagement + satisfaction)

See also the Knowledge Center docs under `knowledge/analytics/` for deeper curated guidance.
