# Remote Work, Dispatch Mode & Virtual Machines

**Status:** LEARN / High Priority  
**Last updated:** 2026-08-22

---

## The need

Federico will not always be in front of a powerful local machine.  
He needs to be able to:

- Start work from a phone
- Let agents continue while the laptop is closed
- Use remote/cloud machines (dispatch mode)
- Keep quality and control without being physically present

---

## Current landscape (2026)

### Grok side
- **Grok Bot**: Agents with persistent cloud computers (browser + filesystem + terminal). Can keep working when your device is closed. Early beta, higher-tier access.
- **Grok Build**: Terminal coding agent (local-first, with subagents). Can be run headlessly / on a remote machine via SSH.

### Claude side
- **Claude Code Remote Control**: Control a local session from phone/browser (session stays on your machine).
- **Claude remote / cloud execution** (Cowork and related): Tasks can run on Anthropic infrastructure so work continues when your computer is closed.
- **Self-hosted / VPS setups**: Run Claude Code on a VPS (DigitalOcean, Hetzner, etc.) and connect via SSH or remote control patterns.

### Generic pattern
**Dispatch mode** = hand a clear job to an agent that runs on a remote machine (cloud VM or always-on box), then review results later.

---

## Recommended practical setup (working direction)

1. **For short interactive work** → This chat + Grok / Claude on phone or laptop
2. **For coding / longer agent runs** → Grok Build or Claude Code on a machine (local or VPS)
3. **For “start and walk away”** → Grok Bot (when available) or Claude remote execution / a small always-on VPS running the agent harness
4. **Always** → Clear brief + STATUS + structured output so remote work stays reviewable

---

## Design requirements for remote/dispatch work

- Jobs must be decomposable into clear briefs
- Outputs must be structured and easy to review on a phone
- Error handling and retries must be explicit
- Prefer writing results into the repos (GitHub) so state is not trapped in a session
- Token discipline becomes even more important (remote runs can burn quietly)

---

## What to explore next

- Concrete “dispatch” templates (brief → remote agent → result in repo)
- Minimal VPS recipe for Claude Code or Grok Build
- How Grok Bot fits when access is available
- Token metering while agents run unattended

---

## Related

- [Token Efficiency](token-efficiency.md)
- [Agentic Design](agentic-design.md)
- Supervised Agents architecture in `ai-capability-os`
