# Event Taxonomy & Instrumentation

**Status:** ADOPT  
**Last updated:** 2026-08-22  
**Context:** Critical for clean analytics in Pendo, Mixpanel, and similar tools

---

## Purpose

Define a consistent way to name and structure events so that analytics remain useful over time and across people/tools.

Bad taxonomy = noisy data that no one trusts.  
Good taxonomy = faster insights and less rework.

---

## Recommended naming convention

**Format:** `[Object]_[Action]_[Context]`

Examples:
- `card_view_dashboard`
- `onboarding_step_complete_serfinsa`
- `feature_first_use_financial_education`
- `guide_cta_click_welcome`
- `form_submit_support_request`

Rules:
- Use lowercase and underscores
- Be specific but not overly long
- Prefer verbs that describe the user action
- Avoid generic names like `click` or `button_press` without object/context

---

## Core event types to prioritize

1. **Key task completion** events
2. **Feature first-use** events
3. **Onboarding step** events
4. **Guide / education** interactions
5. **Error or friction** events (when detectable)
6. **Feedback / survey** submissions

---

## Instrumentation principles

- Instrument for decisions, not for completeness
- Prefer fewer high-quality events over many low-value ones
- Always document what each event means and when it fires
- Include relevant properties (user role, plan, source, etc.) when they add decision value
- Review the taxonomy periodically and clean dead events

---

## Properties (event metadata)

Useful properties often include:
- `user_role` or `user_segment`
- `plan_type`
- `source` / `campaign`
- `feature_flag` (if relevant)
- `onboarding_status`

Only add properties that will actually be used in analysis.

---

## What Federico should own vs delegate

| Own | Can delegate |
|-----|--------------|
| Deciding which events matter for product decisions | Implementing the tracking code |
| Naming convention and taxonomy rules | Maintaining the tracking plan document |
| Reviewing data quality periodically | Setting up the events in the tool |

---

## Anti-patterns

- Tracking every click “just in case”
- Inconsistent naming across features
- No documentation of what events mean
- Never cleaning up unused events
- Changing event names without versioning or migration plan

---

## Related

- [HEARTS Framework](hearts-framework.md)
- [Pendo Patterns](pendo-patterns.md)
