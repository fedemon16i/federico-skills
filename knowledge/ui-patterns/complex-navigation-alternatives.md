# Complex Navigation: Beyond Nested Menus, Accordions & Trees

**Status:** Done · **Priority:** High  
**Use when:** Product is full of dropdowns, sub-submenus, mega-trees, or accordion hell — ops/enterprise density without learnable structure.

---

## The problem

Deep menus fail because users must **learn the product’s hierarchy**, not express their **job**.  
Past ~2 levels of nesting, failure rates climb (steering problem on hover, lost context, mobile collapse chaos).

**Rule of thumb:** If someone says “it’s under Admin → Config → Advanced → Module X → Settings,” the IA is the bug.

---

## Prefer familiar mental models

Borrow models people already use daily:

| Mental model | Product pattern | Beats |
|--------------|-----------------|--------|
| **Spotlight / launcher** (macOS, VS Code, Linear, Notion) | **Command palette** ⌘K — pages + actions + recents | Fourth-level menus |
| **Search box** (Google, Slack) | Global search for *records* and destinations | Hunting in trees |
| **Inbox / queue** | Work sorted by priority/state, not folder taxonomy | Deep file trees for tasks |
| **Tabs + local tools** | Few top areas; tools appear *in context* | One global mega-menu for everything |
| **Breadcrumb + back** | Drill-down with clear path | Infinite accordion without wayfinding |
| **Filters on a table/board** | “Show me X where Y” | Nested “Reports → Sales → Q3 → Region” |
| **Saved views** | Named perspectives (My approvals, At risk) | Rebuilding the same filter path every time |
| **Ask / intent** | Natural language → system routes | Memorizing label trees |

---

## Pattern catalog (when to use what)

### 1. Command palette (⌘K / Ctrl+K)
- **Job:** Navigate + run actions in one surface  
- **Include:** Recents on empty query, pages, actions (“Create invoice”), settings, keyboard hints  
- **When:** >~20 screens or heavy power-user use  
- **Not a substitute for** visible IA for first-time users — pair with simple primary nav  

### 2. Search-as-navigation
- Content/records first; destinations second  
- Results must be **actionable** (“Open project Alpha”), not vague category names  

### 3. Flat primary nav + contextual secondary
- 5–7 top destinations max  
- Sub-tools live **on the object** (project, invoice, asset), not in global submenus  

### 4. Mega menu (careful)
- Only if taxonomy is wide but **shallow** (≤2 levels), scannable columns  
- Avoid cascading hover chains  

### 5. Sidebar with progressive disclosure
- Atlassian-style: density for cross-project work; pin/recents; don’t dump entire enterprise tree  

### 6. Task-based / role-based home
- Default screen = decisions of the role, not org chart of the database  
- Everything else via search/palette/views  

### 7. Drill-down (mobile)
- One level per screen + clear back + breadcrumb  
- Better than 4-level accordion on small screens  

### 8. Trees (when unavoidable)
- Only for true hierarchies (file systems, org charts)  
- Always pair with **search**, expand-to-selection, and “jump to”  
- Never as the *only* way to reach frequent tasks  

### 9. Accordions
- FAQs, optional detail, settings groups — **not** primary app navigation  

---

## Agentic / intent-first layer (2026)

When the product supports it, navigation shifts from “walk the tree” to “state the goal.”

| Pattern | Meaning |
|---------|--------|
| **Intent box** | User describes goal; system chooses layout/tools |
| **Intent preview** | Agent shows plan before acting (approve/edit) |
| **Adaptive UI** | Chat when ambiguous; structured table/form when comparing or editing |
| **Disambiguation** | “Did you mean approve request or edit policy?” — not a wrong page |

**Design work moves to:** intent taxonomy, permissions, recovery, and **which UI shape** fits the task — not only prettier menus.

Agentic does **not** delete IA; it **hides** hierarchy behind routing. Bad IA still produces wrong answers.

---

## Decision guide (quick)

| Symptom | Try first |
|---------|-----------|
| Users can’t find frequent actions | Palette + recents + promote actions to object toolbar |
| 4+ menu levels | Flatten IA; move depth to object pages |
| Mobile accordion maze | Drill-down + search |
| Tree of 1000 nodes | Search + filters + saved views; tree as secondary |
| “Modernize” = more nested UI | Prefer experience change: task home + palette (see `modernize-experience-vs-skin.md`) |
| Dense ops data | Filters/views first — see `dense-data-to-mobile.md` |

---

## Anti-patterns

- Mega-menu that mirrors the org chart  
- Hover-only third-level menus  
- Accordion as the main nav on desktop *and* mobile  
- Command palette with only page names, no actions/recents  
- Chat-only UI for tasks that need dense comparison  
- Hiding critical legal/ops paths only inside a tree  

---

## Link to stack

- Execution: `frontend-design`, project DESIGN.md, UI Integrity  
- Knowledge: dense data mobile, working inside existing DS, agentic-design (emerging)  
- Measure: task success to reach X, time to first action, search/palette usage vs menu depth clicks  

*Last updated: 2026-08-22*
