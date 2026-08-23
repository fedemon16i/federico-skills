# Data Tables, Users, Roles & RBAC UI Patterns

**Status:** Done · **Priority:** High  
**Domains:** SaaS admin, pharma ops, government, education platforms, software consoles — shared patterns.

---

## Tables that people actually work in

A good table answers: **find the row → decide → act** without leaving the screen.

| Pattern | Why |
|---------|-----|
| Column hierarchy | Identity first (name/ID); numbers right-aligned; don’t equal-width everything |
| Sticky header | Always |
| Sort + filter as primary loop | Not “power user extras” |
| Saved views | “My queue”, “Pending approval”, “Expiring licenses” |
| Bulk actions | Clear selection count; confirm destructive |
| Row actions | Visible or predictable overflow; 44px targets on touch |
| Empty / loading / error | Designed states |
| Semantics + keyboard | Real table/ARIA; focus visible |

Link: mobile density → `dense-data-to-mobile.md` (cards, master–detail, not naïvely stacked columns).

---

## Users, roles, permissions (RBAC UI)

Permissions usually stack:

1. **Page / module** — can open User Management?  
2. **Operation** — invite, edit, deactivate?  
3. **Data** — only own region / tenant / students?

### UI patterns that scale

| Pattern | Use |
|---------|-----|
| **Roles as bundles** | Assign role, not 40 checkboxes per user (unless break-glass) |
| **Base + additive roles** | Standard access + optional capability packs |
| **Tier roles** | Full / standard / restricted — one tier per user |
| **Department / job roles** | Map to real jobs (Teacher, Registrar, Auditor) |
| **Permission preview** | “This role can…” plain language before save |
| **Audit trail** | Who changed role/permissions — critical in gov/pharma |
| **Least privilege defaults** | New users start narrow |

### Admin screens that usually exist

- Users list (table + invite + status)  
- Roles list + permission matrix (readable, not a sea of identical checks)  
- Org / tenant / school / site scope  
- Session / security (SSO, MFA indicators)  

Don’t mirror the database schema in the nav; mirror **jobs** (see `complex-navigation-alternatives.md`).

---

## Domain flavor (same skeleton, different constraints)

| Domain | Extra pressure on tables/RBAC |
|--------|------------------------------|
| **Pharma / life sciences** | Audit, segregation of duties, controlled vocabulary, validation states |
| **Government** | Accessibility mandatory, plain language, strict role separation, FOIA-ish transparency |
| **Education** | Term/roster cycles, guardian vs student vs staff, FERPA-like data care |
| **Horizontal SaaS** | Multi-tenant isolation, seat limits, plan-gated columns/actions |
| **Internal software tools** | Density, keyboard, bulk, saved views for ops |

---

## Anti-patterns

- 20 columns on mobile with horizontal chaos and no detail view  
- Permission UI as raw ACL without roles  
- Hiding destructive actions with no confirm  
- “Admin sees everything” with no audit  

*Last updated: 2026-08-22*
