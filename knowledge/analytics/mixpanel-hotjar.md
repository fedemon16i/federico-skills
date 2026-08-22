# Mixpanel & Hotjar Patterns

**Status:** LEARN  
**Last updated:** 2026-08-22

---

## Purpose

Complement Pendo with two other common tools in the behavioral analytics / experience stack.

---

## Mixpanel (event-based product analytics)

**Strengths**
- Strong event model and flexible analysis
- Excellent for funnels, retention cohorts, and “users who did A also did B”
- Good for feature engagement depth and experimentation analysis

**When to prefer Mixpanel-style thinking**
- You care more about event sequences than page/session views
- You need cohort retention and behavioral paths
- You are running product experiments

**Useful patterns**
- Define a clean event taxonomy first (see Event Taxonomy doc)
- Focus on a small set of core events tied to decisions
- Pair volume with rates and always note date range + segment

---

## Hotjar (and similar session + feedback tools)

**Strengths**
- Session recordings and heatmaps
- Qualitative color on top of quantitative drop-offs
- Fast way to see friction that analytics only imply

**When it helps**
- “We see a drop-off here — what are people actually doing?”
- Validating whether a UI issue is real before redesigning
- Complementing usability tests with real production behavior

**Caveats**
- Privacy and sampling matter
- Recordings are not a substitute for structured research
- Easy to waste time watching without a clear question

---

## How they fit with Pendo

| Need | Lean toward |
|------|-------------|
| In-app guides + feature adoption + product analytics in one place | Pendo |
| Flexible event analysis, cohorts, experiments | Mixpanel-style |
| Watching real sessions / heatmaps | Hotjar-style |
| Connecting feedback to behavior | Pendo or Qualtrics + analytics |

Federico’s deepest practical experience is with Pendo. Mixpanel and Hotjar are important references and alternatives depending on the stack of the client or product.

---

## Related

- [Pendo Patterns](pendo-patterns.md)
- [Event Taxonomy](event-taxonomy.md)
- [HEARTS Framework](hearts-framework.md)
