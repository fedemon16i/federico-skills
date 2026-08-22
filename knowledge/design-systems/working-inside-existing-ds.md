# Working Inside an Existing Design System & Design Team

**Status:** Done · **Priority:** High  
**Use when:** Federico joins a product that already has designers, components, or a partial DS — the common case.

---

## Default stance

You are **not** inventing a new brand system. You are:

1. Finding what already governs UI (Figma library, Storybook, CSS tokens, “how we do buttons”).
2. Respecting it unless the brief is explicitly “modernize / replace the system”.
3. Extending with the **same language** (spacing scale, type, components).

Discovery of *the business problem* is optional. Discovery of *the design constraints* is mandatory before drawing.

---

## First 30 minutes (practical)

| Ask / find | Why |
|------------|-----|
| Who owns design decisions? | Avoid redesigning in a vacuum |
| Figma library / tokens / coded components? | Source of truth |
| What’s sacred vs flexible? | Brand color vs experimental layout |
| Dark mode / a11y rules already defined? | Don’t fight existing theme |
| Handoff format they use? | Spec, Figma comments, tickets |

If nothing is documented: **infer from production screenshots** and write a one-page “observed system” (colors, type, spacing, components seen) before proposing new UI.

---

## Modes of work

| Mode | Meaning | Don’t |
|------|---------|--------|
| **Fit-in** | New feature matches current DS | Invent new button styles |
| **Modernize skin** | Same flows, updated visual language | Change IA without asking |
| **Modernize experience** | Flows + UI change | Call it a “reskin” |
| **System migration** | Old library → new tokens/components | Big-bang without inventory |

Always name the mode out loud with the team.

---

## Collaboration with an existing design team

- Align on **problem + constraints** before pixels.
- Prefer **variants of existing components** over one-offs.
- Document exceptions (why this screen breaks the grid).
- For AI-assisted UI: point the agent at **their** tokens/DESIGN rules, not a generic “make it pretty”.
- Conflicts (your proposal vs their system) → surface as decision, not silent override.

---

## Link to execution stack

- Enforce in code: `ui-integrity-guardian` + project DESIGN.md / CLAUDE.md
- Visual direction without generic AI look: `frontend-design` **inside** their constraints
- Verify: Playwright / a11y when available
- Domain skill: `skills/design-system-discipline.md`

---

## Anti-patterns

- Redesigning the button because “it felt old” without a modernization brief
- Parallel design system in a side Figma file that never ships
- Ignoring content density (tables, enterprise data) to force a consumer-app look

*Last updated: 2026-08-22*
