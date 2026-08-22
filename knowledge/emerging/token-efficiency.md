# Token Efficiency & Cost Control

**Status:** ADOPT  
**Last updated:** 2026-08-22

---

## Why this matters

Multi-agent and coding-agent workflows can burn **4x–15x** (sometimes far more) the tokens of normal chat.  
Without discipline, costs and rate limits become the real bottleneck.

Goal: get high-quality work while spending as few tokens as possible.

---

## Highest-leverage techniques (2026)

| Technique | Typical impact | Notes |
|-----------|----------------|-------|
| **Prompt / context caching** | Up to ~90% on stable prefixes | Turn on first. Huge for repeated system prompts and docs |
| **Retrieve once, reuse** | Large | Don’t re-fetch the same context in every agent step |
| **Structured / schema outputs** | Medium–High | Reduces retry loops and garbage tokens |
| **Model routing** | High | Cheap/fast model for simple steps, frontier only when needed |
| **Bound agent loops** | High | Hard limits on steps, retries, and context growth |
| **Prune tool definitions** | High | MCP tool schemas can cost tens of thousands of tokens per call |
| **Summarize intermediate results** | Medium–High | Don’t resend full logs/history every turn |
| **Batch non-urgent work** | ~50% | When the provider supports batch discounts |

---

## Practical rules for Federico’s stack

1. **Prefer curated knowledge over dumping raw web results** into context (this is exactly why the Knowledge Center exists).
2. **STATUS.md + indexes first** — agents should read short entry points, not entire repos.
3. **One Supervisor, limited Workers** — unbounded fan-out is expensive.
4. **Reject malformed outputs early** — retries with clear format instructions beat long confused conversations.
5. **Cache stable context** (principles, architecture, capability definitions).
6. **Measure** — know roughly which workflows burn the most tokens.

---

## Token “metering” (what we need)

We need visibility into:
- Tokens per session / per capability run
- Which steps consume the most
- Remaining budget / risk of hitting limits

Options to explore:
- Provider dashboards (Anthropic, xAI, OpenAI)
- Logging token usage in STATUS or learning records
- Lightweight wrappers / harnesses that report usage
- Tools that surface real-time usage in coding agents

This should become a small capability or checklist in `ai-capability-os`.

---

## Anti-patterns

- Launching many parallel agents “just in case”
- Resending full file contents or logs on every turn
- Using the strongest model for trivial formatting tasks
- Letting agents loop without a hard stop
- Loading every MCP tool definition when only 2–3 are needed

---

## Related

- [Agentic Design](agentic-design.md)
- [Measuring Agentic Experiences](measuring-agentic-experiences.md)
- Supervised Agents architecture in `ai-capability-os`
