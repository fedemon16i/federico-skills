# Monthly Audit Ritual — Knowledge, References & Patterns

**Status:** Done · **Priority:** High  
**Cadence:** Once per month (pick a fixed day, e.g. first Saturday).  
**Owner:** Federico + whichever agent is online (Grok / Claude). Log outcome in `ai-capability-os/coordination/AGENT-LOG.md`.

---

## Where things live

| What | Repo | Path |
|------|------|------|
| Domain knowledge, patterns, narrative | **federico-skills** | `knowledge/` |
| Best-in-class product references | federico-skills | `knowledge/ui-patterns/reference-products-best-in-class.md` |
| Anti-slop, tables, nav, domains | federico-skills | `knowledge/ui-patterns/` |
| Execution capabilities, skills installs | **ai-capability-os** | `capabilities/`, `sources/` |
| Cross-LLM status | ai-capability-os | `coordination/HANDOFF.md`, `AGENT-LOG.md` |

**Rule:** Knowledge = *what good looks like*. Capabilities = *how we run tools/agents*. Monthly audit touches **both**, but most “is this still true?” work is under `federico-skills/knowledge/`.

---

## 60–90 minute monthly checklist

### 1) Index health (10 min)
- Open `knowledge/_index.md`
- Mark anything **Done** that is stale or wrong → **Update** or **Deprecated**
- Promote one **Planned** item only if it blocked real work this month

### 2) Reference products (15 min)
- File: `ui-patterns/reference-products-best-in-class.md`
- For each major row: still a fair “best in class” signal?
- Add at most **2** new refs if the industry shifted; remove or demote clones/noise
- Note one sentence: *what changed in the market*

### 3) Pattern pages (20 min)
Spot-check (rotate each month — don’t reread everything):
- Navigation / tables / anti-slop / domain mobile / RBAC
- Ask: did a client project contradict this? If yes → patch the doc

### 4) Analytics & research methods (10 min)
- Pendo / Mixpanel / methods still match how Federico works?
- Link rot: preferred sources in `sources.md` still alive?

### 5) Capabilities cross-check (15 min)
- Skim `ai-capability-os/STATUS.md` + `coordination/HANDOFF.md`
- Skills installs still the shortlist? (frontend-design, grill-me, etc.)
- Debt still accurate (Playwright, MCP, VM)?

### 6) Close the loop (5 min)
Write in AGENT-LOG:
```
Monthly audit YYYY-MM
- Kept: …
- Updated: …
- Deprecated: …
- Next month focus: …
```
Update `federico-skills/STATUS.md` date + one-line phase.

---

## Deprecation rules

| Signal | Action |
|--------|--------|
| Tool dead or rebranded away | Remove or mark Deprecated |
| Pattern contradicted by 2+ real projects | Rewrite section |
| “Best product” became average | Demote; replace with stronger ref |
| Duplicate of another doc | Merge; leave redirect note in index |

**Do not** expand knowledge for its own sake in the audit. Prefer **delete/clarify** over new folders.

---

## Optional quarterly (every 3 months)

- Full pass on portfolio narrative pack (interview market shifts)
- Re-run shortlist of public skills marketplaces (only ADOPT if clearly better than current chain)
- Technology scan: one page of “what’s newly relevant to Federico’s stack”

---

## Prompt snippet for any agent

```text
Ejecutá el Monthly Audit Ritual (federico-skills/knowledge/emerging/monthly-audit-ritual.md).
No expandas knowledge salvo parches. Actualizá index/STATUS y dejá nota en coordination/AGENT-LOG.
```

*Last updated: 2026-08-22*
