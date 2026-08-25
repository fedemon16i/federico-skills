# PostHog Patterns

**Status:** ADOPT  
**Last updated:** 2026-08-25  
**Context:** Open-source behavioral analytics platform. Replaces Pendo + FullStory + Amplitude + LaunchDarkly in a single self-hostable stack. Federico's recommended anchor tool for clients without an existing analytics stack.

---

## Purpose

Capture the high-leverage ways to use PostHog for product decisions across the full stack: events, session recordings, feature flags, and LLM observability. Focus is on what moves the needle — not on feature exploration for its own sake.

---

## Why PostHog over alternatives

| Scenario | Tool |
|---|---|
| Client has nothing | PostHog — full stack, < 2h setup, free tier generous |
| Client has Pendo | Extend Pendo with PostHog for session replay + feature flags |
| Client has Mixpanel | Add PostHog for session replay; migrate events over time |
| Need session recording + events together | PostHog is the cleanest integration |
| LLM agents in the product | PostHog LLM Analytics addon |

---

## Core use cases Federico cares about

1. **Behavioral funnels** — Where exactly do users drop off in critical flows?
2. **Session recordings** — Watch abandonment, hesitation, and confusion in context
3. **Feature flags** — Ship to 10% first; measure behavior delta before full rollout
4. **Cohorts** — Segment users by what they actually do, not demographics
5. **LLM observability** — Token cost, latency, quality signals per AI feature (see `llm-tracking.md`)
6. **Retention** — Who comes back? What did they do in session 1 that predicts session 5?

---

## Recommended patterns

### 1. Event taxonomy — instrument once, slice forever

Use the `[Object]_[Action]_[Context]` convention from `event-taxonomy.md`.

```js
posthog.capture('checkout_completed', {
  plan: 'pro',
  source: 'upgrade_modal',
  trial_days_remaining: 3
})
```

Properties are cheap. Retrofitting them costs a sprint. Add context now.

### 2. Funnels for critical flows

- Keep funnels 3–6 steps. Longer = harder to act on.
- Always compare conversion AND absolute volume.
- Create "before" baseline BEFORE shipping any change.
- Use `Breakdown by` to find which segments drop off more.

Critical funnels to instrument first (in order):
1. Signup → first core action (activation)
2. Core task start → completion
3. Upgrade intent → payment complete

### 3. Session recordings — targeted, not exhaustive

Recording everything is expensive and noisy. Instead:

```js
// Record only sessions that hit a specific funnel step
posthog.startSessionRecording()
// Or: enable via PostHog's sampling config (e.g. 20% of sessions)
```

**What to watch:**
- Sessions where funnel step 2 → 3 dropped (look for hesitation pattern)
- Sessions with rage clicks (PostHog flags these automatically)
- Sessions that hit an error state
- First session of new users (onboarding quality signal)

### 4. Feature flags — measure before shipping wide

```js
if (posthog.isFeatureEnabled('new-checkout-flow')) {
  // show new experience
}
```

PostHog logs `$feature_flag_called` automatically. After shipping:
- Compare funnel conversion: flag ON vs flag OFF
- Compare session duration and engagement
- Look for error spikes in flag-ON cohort

### 5. Cohorts as behavioral segments

Avoid demographic-only cohorts. Build them from behavior:

- "Activated but not retained" — completed activation funnel, no session in 14 days
- "Power users" — core action ≥ 5x in first week
- "At risk" — used product week 1-2, nothing since

Use cohorts to filter recordings, target feature flags, and power retention analysis.

### 6. Retention charts

PostHog's retention report: pick the "activation event" → see what % of users who did that came back in weeks 1, 2, 3, 4.

The goal is to identify which first actions correlate with retention. That becomes the onboarding north star.

---

## What to instrument first (priority order)

1. Core task completion event (the single most important thing a user does)
2. Activation moment (first time they get value)
3. Signup and onboarding steps
4. Any upgrade / paywall interaction
5. Error states and empty states (often missed)
6. Feature-specific events once #1–4 are clean

---

## Setup speed reference

| Task | Time |
|---|---|
| Install JS snippet | 5 min |
| First events firing | 15 min |
| First funnel live | 30 min |
| Session recording on | 5 min (toggle in UI) |
| Feature flag deployed | 20 min |
| LLM observability (Helicone bridge) | 30 min |
| Full stack operational | < 2 hours |

---

## Insight format that works

> "X% of users who [completed activation event] came back within 7 days, vs Y% of users who didn't. Cohort: [date range], n=[size]. Suggests [activation event] is a leading indicator of retention."

> "Users who saw the [feature flag variant] completed the checkout funnel at Z% vs W% in control. Δ = [+/-]X pp. Confidence: [high/medium/low based on n]."

Avoid: "engagement improved", "users liked the new flow", "metrics are trending up".

---

## Anti-patterns

- Instrumenting everything before defining what decisions the data will drive
- Using PostHog as a dashboard-display tool without ever watching recordings
- Not setting up a "before" baseline before A/B testing anything
- Ignoring the `$pageview` autocapture — it's free signal, use it
- Creating cohorts based on demographics when behavioral cohorts are available
- Treating the free tier limit as a ceiling — it's 1M events/month, more than enough for early products

---

## PostHog vs Pendo — when each fits

| | PostHog | Pendo |
|---|---|---|
| Best for | Developer-centric teams, full-stack visibility | Non-technical PMs, heavy in-app guidance |
| Session replay | Yes (included) | Yes (add-on cost) |
| Feature flags | Yes (included) | No |
| In-app guides | Basic (surveys) | Core feature |
| LLM observability | Yes (addon) | No |
| Self-hostable | Yes | No |
| Price at scale | Lower | Higher |

If client already has Pendo: don't replace it, extend it. PostHog for session replay + flags, Pendo for guides + NPS.

---

## Related

- [Pendo Patterns](pendo-patterns.md)
- [LLM Tracking](llm-tracking.md)
- [HEARTS Framework](hearts-framework.md)
- [Event Taxonomy](event-taxonomy.md)
- [Hiring Scenarios](hiring-scenarios.md)
