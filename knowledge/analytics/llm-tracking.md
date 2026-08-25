# LLM Agent Tracking

**Status:** LEARN  
**Last updated:** 2026-08-25  
**Context:** New frontier — most teams ship AI features with zero observability. This doc covers what to measure, which tools exist, and how to distinguish real impact from perceived impact.

---

## The gap

Most analytics setups track what users click. LLM features add a new layer: what the AI generates, how long it takes, what it costs, and whether it actually helped. Almost no product team has this wired up. The tools exist. The patterns don't yet exist as shared knowledge.

This doc is Federico's working reference for that gap.

---

## What to measure — the metrics that matter

### Operational (infrastructure)

| Metric | Why it matters |
|---|---|
| Token cost per session/user | Find expensive prompts before they eat budget |
| Latency p50 / p95 | p50 = typical experience; p95 = worst regular experience |
| Error rate (API failures, timeouts) | LLM APIs fail; users don't get explanations |
| Cache hit rate | Are you regenerating the same outputs repeatedly? |
| Model version in production | Know which model served each request for retrospective analysis |

### Quality signals

| Metric | What it captures |
|---|---|
| Thumbs up / thumbs down rate | Explicit user signal on output quality |
| Regeneration rate | User asked again — implicit rejection of first output |
| Copy / use rate | User actually used the output (copy to clipboard, applied suggestion) |
| Abandon rate after AI response | User left after seeing the output — strong negative signal |
| Follow-up question rate | Did the AI answer land, or did it generate more questions? |

### Impact (the one most teams skip)

| Metric | How to measure |
|---|---|
| Task completion rate: AI users vs non-AI | Feature flag: AI ON vs OFF, compare funnel |
| Time-to-complete: AI users vs non-AI | Session duration on the core task |
| Error rate: AI users vs non-AI | Downstream errors after using AI feature |
| Retention: AI power users vs others | Week-4 retention cohort split |
| NPS delta | Survey promoters vs detractors — do AI users score higher? |

The impact metrics require a baseline. Instrument the "before" state BEFORE shipping the AI feature.

---

## Tools — stack by scenario

### Scenario A — PostHog already installed (ADOPT path)

PostHog has an LLM observability addon. Instrument directly:

```js
// Capture LLM interaction
posthog.capture('llm_response_received', {
  model: 'claude-sonnet-4-6',
  input_tokens: 1240,
  output_tokens: 380,
  latency_ms: 1820,
  feature: 'chat-assistant',
  session_id: currentSessionId
})

// Capture quality signal
posthog.capture('llm_feedback_given', {
  rating: 'thumbs_up', // or 'thumbs_down'
  feature: 'chat-assistant',
  response_id: responseId
})
```

PostHog will correlate these with user behavior, funnels, and session recordings automatically.

### Scenario B — Need dedicated LLM proxy (ADOPT path)

**Helicone** — transparent proxy between your code and Anthropic/OpenAI. 5-minute setup.

```js
// Before
import Anthropic from '@anthropic-ai/sdk'
const client = new Anthropic({ apiKey: process.env.ANTHROPIC_API_KEY })

// After (add baseURL only)
const client = new Anthropic({
  apiKey: process.env.ANTHROPIC_API_KEY,
  baseURL: 'https://anthropic.helicone.ai',
  defaultHeaders: { 'Helicone-Auth': `Bearer ${process.env.HELICONE_API_KEY}` }
})
```

Helicone captures: every request/response, token counts, latency, cost estimates, model version. Zero changes to prompt code.

Add custom properties for user/session context:

```js
defaultHeaders: {
  'Helicone-Auth': `Bearer ${process.env.HELICONE_API_KEY}`,
  'Helicone-Property-User-Id': userId,
  'Helicone-Property-Feature': 'chat-assistant',
  'Helicone-Property-Session': sessionId
}
```

### Scenario C — Need evaluation / prompt A/B testing (LEARN path)

**Braintrust** — for comparing prompt versions in production.

Use case: "Is prompt version B better than version A across real user queries?"

```js
import { initLogger, wrapAI } from 'braintrust'
import Anthropic from '@anthropic-ai/sdk'

initLogger({ projectName: 'your-product', apiKey: process.env.BRAINTRUST_API_KEY })

const client = wrapAI(new Anthropic())
// Every subsequent call is logged + scored automatically
```

Braintrust runs evaluators (LLM-as-judge, custom functions) against production traces. Not needed on day 1 — introduce when you're actively iterating on prompts.

### Scenario D — LangChain in the stack (REFERENCE)

LangSmith is the default for LangChain tracing. Same concept as Helicone/Braintrust but LangChain-native. If the codebase uses LangChain, LangSmith is the path of least resistance.

### OpenTelemetry standard (STUDY)

OpenLLMetry adds LLM calls to standard OTel traces. Useful if the team already has Datadog/Grafana and wants LLM calls visible in the same trace as API calls. Not needed if using PostHog or Helicone.

---

## Minimal viable observability — what to wire up first

Priority order for a new AI feature:

1. **Latency logging** — capture ms per LLM call, log it somewhere (PostHog event, Helicone, or even a simple backend log)
2. **Token cost tracking** — Anthropic/OpenAI return token counts in the response; log them
3. **Quality signal** — add thumbs up/down or a "was this helpful?" inline question
4. **Task completion funnel** — funnel for the core task, split by "used AI feature" vs "didn't"
5. **Error state capture** — what does the user see when the LLM call fails? Are you tracking it?

That's it for v1. Braintrust-level evaluation comes later.

---

## Prompt A/B testing pattern

Run prompt variations like you'd run a feature flag:

```js
const promptVariant = posthog.isFeatureEnabled('prompt-v2') ? PROMPT_V2 : PROMPT_V1

const response = await anthropic.messages.create({
  model: 'claude-sonnet-4-6',
  messages: [{ role: 'user', content: promptVariant + userInput }]
})

posthog.capture('llm_prompt_variant_used', {
  variant: posthog.isFeatureEnabled('prompt-v2') ? 'v2' : 'v1',
  output_tokens: response.usage.output_tokens,
  latency_ms: Date.now() - startTime
})
```

After N sessions, compare: regeneration rate, thumbs up rate, task completion funnel — by variant.

---

## The framing that lands in client conversations

Most teams ask: "Are users happy with the AI feature?"

The right question: "Did the AI feature change user behavior in the direction we wanted?"

Signals that prove behavior change:
- Task completion rate went up after the AI feature shipped (compare cohorts)
- Time-on-task went down for users who used the AI feature
- Error rate in the downstream step dropped
- Week-4 retention is higher for users who adopted the AI feature

Survey satisfaction is a leading indicator. Behavior change is the proof.

---

## Insight format that works

> "Users who interacted with the AI assistant completed [core task] at X% vs Y% for users who skipped it. Measured over [date range] via feature flag cohorts, n=[size]. Task time delta: [Δ]s. Cost per successful interaction: $[N]."

> "Prompt variant B reduced regeneration rate from X% to Y% across [N] sessions. p95 latency unchanged at [ms]. Thumbs-up rate: +[Δ]pp."

---

## Anti-patterns

- Shipping an AI feature with zero observability ("we'll add it later")
- Measuring only thumbs up/down with no behavioral correlation
- Treating latency as an afterthought — users notice > 2s before first token
- Running prompt experiments without a baseline metric to compare against
- Using LLM output in a UI without capturing whether the user actually used it
- Calling an AI feature successful because users tried it (usage ≠ impact)

---

## Cost awareness — quick reference

Anthropic pricing (approximate, check current rates):
- claude-haiku-4-5: fastest, cheapest — use for high-volume, simple tasks
- claude-sonnet-4-6: balanced — use for most product features
- claude-opus-5: most capable, most expensive — use for complex reasoning tasks

Rule of thumb: instrument cost from day 1. A feature that works in prototype can become expensive at scale if prompt length isn't controlled.

---

## Related

- [PostHog Patterns](posthog-patterns.md)
- [Hiring Scenarios](hiring-scenarios.md)
- [HEARTS Framework](hearts-framework.md)
