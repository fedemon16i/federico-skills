# Dense Data UI → Mobile (Tables, Grids, Dashboards)

**Status:** Done · **Priority:** High  
**Use when:** Desktop tables/grids work well; product needs usable mobile (or responsive) without losing critical data tasks.

---

## Problem

Enterprise and ops UIs often optimize for **density on large screens**. Naïve responsive CSS (“stack all columns”) destroys scanability and actions. Need **intentional patterns**, not only breakpoints.

---

## Decide before designing

1. **Primary job on mobile** — view status, approve, search one record, edit field, or full power-user parity?
2. **Must-have columns** — usually 2–4; the rest behind detail.
3. **Actions frequency** — row actions vs bulk vs read-only.
4. **Offline / field use?** — bigger hit targets, fewer steps.

If product wants “full table on phone”, push back with a job story; offer progressive disclosure.

---

## Pattern catalog (pick, don’t stack all)

| Pattern | Best for | Notes |
|---------|----------|--------|
| **Card list** | Few fields per item + clear primary action | Most common replacement for wide tables |
| **Master–detail** | List + full record | List stays simple; detail is the real form |
| **Horizontal scroll table** | Compare many columns occasionally | Freeze first column; show scroll hint |
| **Column priority** | Some columns always visible | `priority` / show-hide columns |
| **Summary row + expand** | KPIs then drill-down | Good for dashboards |
| **Filters first** | Large datasets | Search/filter before any grid |
| **Stacked key-value** | Record detail | Label above value; group sections |
| **Bottom sheet actions** | Row actions on mobile | Avoid tiny icon-only overflow menus |

Combine: e.g. filters → card list → detail sheet.

---

## Interaction details that matter

- Touch targets ≥ 44px for row actions
- Don’t rely on hover states
- Pagination / infinite scroll: prefer explicit “Load more” in ops tools
- Bulk actions: often **desktop-only** or a dedicated mobile mode
- Empty and loading states for slow enterprise APIs

---

## Modernize without lying

- **Visual refresh** of the same table ≠ mobile strategy
- If desktop stays dense, document **two complementary layouts**, not one broken layout

---

## Link to stack

- Knowledge: `mobile-first-resilience.md` (broader mobile)
- Execution: `frontend-design` + project tokens; verify with Playwright viewports
- Integrity: no hardcoded spacing that only works at 1280px

---

## Anti-patterns

- Hiding critical legal/ops columns with no detail view
- Tiny checkboxes for bulk select on mobile
- Eight-column “responsive” table that requires pinch-zoom

*Last updated: 2026-08-22*
