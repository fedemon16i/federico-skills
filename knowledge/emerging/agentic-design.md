# Agentic Design

**Status:** LEARN / High Priority  
**Last updated:** 2026-08-22

---

## What it is

Designing products, interfaces, and workflows that work well when AI agents (LLMs + tools) are part of the system — either as co-pilots, autonomous workers, or intermediaries between the user and the product.

This is different from classical UI design. The “user” may be a human, an agent, or both.

---

## Why it matters now

- Building is faster and cheaper
- Feedback loops are much shorter
- Agents can already execute multi-step work
- Many product experiences will soon include an agentic layer

Ignoring this means designing only for the human path while the agent path becomes the real bottleneck or the real opportunity.

---

## Key design questions

1. What should the human decide vs what should the agent decide?
2. How does the human stay in control without micromanaging?
3. How do we make the agent’s actions visible and reviewable?
4. How do we handle errors, uncertainty, and partial results gracefully?
5. What does “success” look like when an agent is involved?

---

## Measurement (critical and still immature)

Classic metrics (task success, time on task, NPS) are not enough.

Emerging measurement needs:

| Dimension | Possible signals |
|-----------|------------------|
| **Task completion by agent** | Did the agent finish the job correctly? |
| **Human oversight cost** | How much time/attention did the human still need to spend? |
| **Trust calibration** | Did the human trust the agent the right amount (not too much, not too little)? |
| **Error recovery** | How quickly and cleanly were mistakes corrected? |
| **Handoff quality** | When the agent passes work back to the human, is the context clear? |

This area is still evolving. We should treat measurement of agentic experiences as an active research topic.

---

## Design principles (working set)

- Make agent actions legible
- Prefer progressive autonomy (start assisted, earn more autonomy)
- Design clear handoff points
- Show uncertainty instead of hiding it
- Optimize for the combined human + agent system, not only the human UI

---

## Relation to Federico’s stack

- Connects directly to the Supervised Agents architecture in `ai-capability-os`
- Relevant for Session Replay + Product Spec capabilities
- Will influence how we design future Product Intelligence features

---

## Next steps for this document

- Collect concrete examples of good and bad agentic UX
- Define a lightweight measurement checklist
- Link to relevant capabilities in `ai-capability-os`
