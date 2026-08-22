# Measuring Agentic Experiences

**Status:** LEARN / High Priority  
**Last updated:** 2026-08-22

---

## The problem

Classic UX metrics (task success, time on task, NPS, SUS) were designed for human-only interfaces.  
When an agent is part of the system, those metrics become incomplete or misleading.

We need new ways to answer:

> “Is this human + agent system actually working well?”

---

## Core dimensions to measure

| Dimension | Question it answers | Example signals |
|-----------|---------------------|-----------------|
| **Agent Task Completion** | Did the agent finish the job correctly? | Success/failure rate, verification checks |
| **Human Oversight Cost** | How much attention did the human still need to give? | Time spent reviewing, number of interventions |
| **Trust Calibration** | Did the human trust the agent the right amount? | Over-reliance vs under-reliance incidents |
| **Handoff Quality** | When work moves between human and agent, is context clear? | Clarification requests, rework after handoff |
| **Error Recovery** | How well does the system recover from mistakes? | Time to detect + fix, user frustration |
| **Progressive Autonomy** | Is the agent earning more autonomy over time? | Reduction in required approvals |

---

## Practical starting metrics (lightweight)

For early agentic features, track at least:

1. **Completion rate** of the end-to-end job (human + agent together)
2. **Number of human interventions** required per successful job
3. **Time from request to verified result**
4. **Explicit user correction rate** (how often the human has to fix the agent)
5. **Abandon / retry rate**

These are imperfect but better than only measuring the human UI.

---

## Research methods that still help

- Observational sessions where the user works *with* the agent
- Think-aloud while the agent is acting
- Post-task questions focused on trust and control (“Did you feel in control?”, “Where did you hesitate?”)
- Comparison of assisted vs unassisted paths when possible

---

## Anti-patterns

- Only measuring the chat interface and ignoring the quality of the agent’s work
- Treating every agent output as a “success” if the user didn’t complain
- Optimizing for speed of agent response instead of correctness + low oversight cost
- Ignoring the cost of human verification

---

## Relation to the rest of the system

- Complements [Agentic Design](agentic-design.md)
- Should influence how we evaluate capabilities in `ai-capability-os`
- Relevant for Session Replay (watching human + agent interactions) and UI Integrity Guardian

---

## Next evolution of this document

- Concrete scorecards for different types of agentic features
- Examples from real products
- Link to evaluation framework in the Capability OS
