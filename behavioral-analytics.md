---
name: behavioral-analytics
description: Apply behavioral analytics frameworks and tool-specific patterns. Use when analyzing user behavior data, designing analytics instrumentation, interpreting Pendo/Mixpanel/GA4 data, writing analytics sections in case studies, or building analytics dashboards. Activates for Pendo, Mixpanel, Google Analytics, Qualtrics, Maze mentions.
---

# Behavioral Analytics Skill

Federico is a UX & Product Experience Analyst specializing in behavioral analytics. He has worked with Pendo at DollarCity and Chek, and uses Qualtrics for survey/feedback layers.

## Core Principle

**Never invent metrics.** If data isn't available, use explicit placeholders:
- `[RETENTION RATE — PLACEHOLDER]`
- `[EVENT COUNT — SOURCE: PENDO, DATE RANGE NEEDED]`

## Analytics Frameworks Federico Uses

### HEARTS (Google)
Used at Chek. Map to product areas:
- **H**appiness → Qualtrics CSAT/NPS
- **E**ngagement → Pendo feature adoption, session frequency
- **A**doption → First-use events, onboarding completion rate
- **R**etention → Cohort return rate, D7/D30
- **T**ask Success → Funnel completion, error rate, time-on-task

### Behavioral Event Taxonomy
Standard naming: `[Object]_[Action]_[Context]`
Examples:
- `card_view_dashboard`
- `onboarding_step_complete_serfinsa`
- `feature_first_use_financial_education`

## Tool-Specific Patterns

### Pendo
- **Guides**: in-app tooltips, walkthroughs, modals → measure guide_view → guide_cta_click
- **Paths**: sequence analysis — where do users go after X?
- **Funnels**: conversion from step A → B → C
- **NPS**: in-app surveys (connect to HEARTS Happiness)
- **Tagged Features**: track specific UI element interactions
- Key Pendo insight format: `"X% of users who completed [guide] went on to [action] within [timeframe]"`

### Mixpanel
- Event-based, not session-based
- Focus on: funnels, retention cohorts, user flows
- Power metric: "X% of users who did A also did B within N days"
- Use for: feature engagement depth, A/B test analysis

### Google Analytics 4 (GA4)
- Web/app behavior, traffic source analysis
- Key reports: Funnel Exploration, User Explorer, Path Exploration
- Events to always track: `page_view`, `scroll`, `click`, `form_submit`, `session_start`
- Custom dimensions for UX: `user_segment`, `onboarding_status`, `feature_flag`

### Qualtrics
- Survey + behavioral data integration
- Use for: post-task surveys (after Maze sessions), NPS programs, CSAT
- Connect Qualtrics responses to Pendo segments for behavioral context

### Maze
- Unmoderated usability testing
- Key metrics: task success rate, misclick rate, time on task, direct success vs. indirect success
- Always report: n= (sample size), date range, prototype fidelity level

## Writing Analytics in Case Studies

### Weak (avoid)
> "We used Pendo to track user behavior and found improvements."

### Strong (use)
> "Pendo path analysis showed 68% of users exited at the SERFINSA step before the API integration. Post-launch, funnel completion rose to [X%] — measured over [date range], n=[cohort size]."

## Analytics Section Template for Case Studies

```
## Measuring Impact

### Instrumentation
[What events/guides/surveys were set up and why]

### Baseline
[What the data showed before the intervention]
Metric: [VALUE or PLACEHOLDER] | Source: [TOOL] | Date: [RANGE]

### Post-Launch
[What changed]
Metric: [VALUE or PLACEHOLDER] | Source: [TOOL] | Date: [RANGE]

### Interpretation
[Why this matters — connect to business/user outcome]
```

## Red Flags in Analytics Narratives

- Correlation presented as causation without A/B test
- Sample size not mentioned
- Date range not specified
- Single metric without context (always pair: volume + rate, or engagement + satisfaction)
- Vanity metrics (page views alone, total users without segmentation)

## Federico's Analytics Stack by Project

| Project | Primary Tool | Secondary | Survey |
|---|---|---|---|
| Chek | Pendo | — | Qualtrics |
| DollarCity | Pendo | Dynamics | — |
| EY/Globant | — | GA4 | Qualtrics |
