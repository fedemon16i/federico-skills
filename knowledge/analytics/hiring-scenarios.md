# Hiring Scenarios — Analytics Stack

**Status:** ADOPT  
**Last updated:** 2026-08-25  
**Context:** What to do on day 1 depending on what analytics stack the client/employer already has. Designed for Federico to walk into any environment and immediately know the play.

---

## The three scenarios

### Scenario A — Client has Pendo

**What this means:** They've invested in product analytics. They have tagged features, probably some funnels, maybe NPS running. They may or may not be using it well.

**Day 1 audit:**
1. Ask to see their Pendo dashboards — what are they actually tracking?
2. Find the last decision they made using Pendo data. What was it? How did they validate?
3. Check if they have session replay configured (Pendo add-on — often not purchased)
4. Find the most important funnel for their core product flow. Is it in Pendo? Is it clean?

**What to do:**

| Situation | Action |
|---|---|
| Pendo installed, barely used | Run the real-time audit (Layer 02) on their core flow. Build the funnel they should have had from day 1. |
| Pendo used but funnels are wrong | Rebuild critical funnels with correct step definitions. Show the difference in drop-off numbers. |
| No session replay | Add PostHog alongside Pendo for session recording. They coexist. |
| Data looks fine, decisions don't | Shift focus: are they asking the right questions? Start with HEARTS framework alignment. |

**Quick win:** Find one funnel that matters, show the real drop-off rate with severity, and propose the fix + tracking spec for before/after measurement. Takes 2–3 hours with Pendo access.

**References:** [pendo-patterns.md](pendo-patterns.md)

---

### Scenario B — Client has PostHog / Mixpanel / Amplitude

**What this means:** They have a developer-centric analytics setup. Events are probably firing. Whether they're being used for decisions is a separate question.

**Day 1 audit:**
1. Review the event taxonomy. Is it consistent? (`[Object]_[Action]_[Context]` or chaos?)
2. Find the activation funnel — do they have one? What's the current conversion rate?
3. Check session recordings — are they on? Are they being watched?
4. Ask: what was the last A/B test or feature flag experiment they ran? What did they learn?

**What to do (PostHog):**

| Situation | Action |
|---|---|
| Events firing but no funnels | Build the activation funnel + core task funnel immediately |
| Funnels exist but no recordings | Turn on session recording for the drop-off step |
| No feature flags in use | Propose the next feature as a flag experiment — show the pattern |
| LLM features with no tracking | Add LLM observability (PostHog addon or Helicone) |

**What to do (Mixpanel/Amplitude):**

- Both are solid event analytics tools. Approach the same way: audit event taxonomy, find missing funnels, find what they're not watching.
- Mixpanel: strong on cohort analysis and retention. Push that direction.
- Amplitude: strong on behavioral cohorts and predictive analytics. Use their Journey Maps feature.
- Session recording: both require a separate tool (FullStory, Hotjar, or PostHog). See if one is already installed.

**Quick win:** Find one cohort that reveals something unexpected — e.g., "users who completed X in the first week have 3x week-4 retention vs users who didn't." Takes 1–2 hours with dashboard access.

**References:** [posthog-patterns.md](posthog-patterns.md), [mixpanel-hotjar.md](mixpanel-hotjar.md), [hearts-framework.md](hearts-framework.md)

---

### Scenario C — Client has nothing

**What this means:** No analytics tool. Possibly Google Analytics 4, but with no product event tracking. Decision-making is anecdotal.

**This is the highest-leverage scenario.** The gap between where they are and where they should be is biggest. The differentiation is clearest.

**Immediate play — deploy PostHog in under 2 hours:**

```html
<!-- Step 1: Add snippet to <head> (5 min) -->
<script>
  !function(t,e){...}(window, document)
  posthog.init('your-api-key', { api_host: 'https://us.i.posthog.com' })
</script>
```

Then, in order:
1. Verify autocapture is firing (pageviews, clicks) — free signal, already there
2. Instrument the 3 most important events (activation, core task, any conversion)
3. Build the activation funnel
4. Turn on session recording (sampling: 20% of sessions is enough to start)
5. Set up a weekly review cadence — who looks at this data, when, to decide what?

**What to propose:**

> "I can have the first meaningful data in your hands within the week. No new vendors to negotiate with — PostHog has a generous free tier. Here's what we'll know by Friday: where users drop off in [core flow], how many sessions we're losing at [specific step], and what the first-session behavior looks like for users who came back vs users who didn't."

**Cost:** Free tier covers 1M events/month + 5K session recordings. Enough for most early products. Self-hostable if data sovereignty is a concern.

**What NOT to do:** Don't pitch a 6-week analytics overhaul. Pitch the first funnel. Get one decision made from data. Then expand from there.

**References:** [posthog-patterns.md](posthog-patterns.md), [event-taxonomy.md](event-taxonomy.md)

---

## Cross-scenario patterns

### The real-time audit play (works in any scenario)

The Layer 02 audit from the AI Capabilities Framework runs regardless of what analytics stack they have:

1. Lighthouse + axe in 5 minutes — performance baseline, a11y issues
2. Manual walkthrough of core flow — friction points annotated
3. Funnel hypothesis — where users probably drop off, what to track to confirm
4. Tracking spec — specific events to instrument, regardless of tool

This works as a demo in an interview or on day 1 with a new client. The audit is tool-agnostic; the instrumentation spec adapts to whatever they have.

### The baseline rule

Before any change:
1. Document the current conversion rate / behavior metric
2. Set up the "after" tracking spec in advance
3. Ship the change
4. Measure delta

Without step 1, there's no proof the change worked. This applies in every scenario.

### When they ask "what would you do first?"

Answer: "Find your one most important user journey, measure where it breaks, and give you a number. Then propose one change and measure whether it works."

Not: "I'd do a full analytics audit and taxonomy review and stakeholder interviews and..."

Start small. Show the loop working. Expand.

---

## The LLM/AI feature overlay

If the product has AI features, add this question to any scenario audit:

- What LLM calls are running? At what cost? At what latency?
- Is there a quality signal (thumbs up/down, regeneration rate)?
- Do you know if the AI feature actually changes user behavior vs users who skip it?

Almost always the answer is "we don't know." That's the gap. See [llm-tracking.md](llm-tracking.md) for the instrumentation play.

---

## Related

- [PostHog Patterns](posthog-patterns.md)
- [Pendo Patterns](pendo-patterns.md)
- [LLM Tracking](llm-tracking.md)
- [HEARTS Framework](hearts-framework.md)
- [Event Taxonomy](event-taxonomy.md)
