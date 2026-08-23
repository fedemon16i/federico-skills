# Portfolio OS Shell Patterns

**Status:** Done · **Priority:** High  
**Use when:** Building FM OS / desktop-metaphor portfolio (Chat A). Complements portfolio-narrative (what cases say) with **how the shell navigates**.

---

## Core idea

The site behaves like a **small OS**: persistent chrome, a **main window** for content, optional desktop of shortcuts — not a infinite scroll of case cards only.

**Primary structural reference:** PostHog.com (side chrome + center surface; mobile window vs closed → icon desktop).  
**Aesthetic:** Federico’s cyberpunk / glitch / optional E-Ink — applied to the **shell**, not forced onto client case brands.

---

## Patterns to keep

| Pattern | Why |
|---------|-----|
| **Window = context** | URL can point at a window (share Resume, Skills, one project index entry) |
| **Constant nav region** | Orient without learning a new IA every click |
| **Mobile close → desktop** | See all destinations; labels for recruiters |
| **Human labels** | “Projects”, “Resume” — not only cryptic icons |
| **One-liners on project tiles** | What you did / impact class (Martin Refi / Amit class) |
| **Case brand ≠ OS skin** | Inside a case, typography/color can follow EY, Ripley, etc. |

---

## Patterns to avoid

| Anti-pattern | Why |
|--------------|-----|
| CTAs of list vs CTAs inside preview look the same | Nitin-class confusion |
| Desktop of files with no hierarchy for recruiters | Cute but slow to “show me work” |
| Extra click to reach any teaser | Empty ritual |
| AI-slop card grids in the shell | No story |
| Unreadable full-screen window spam | vrtxforge motion without hierarchy |

---

## Recruiter path (non-negotiable)

≤2 interactions to: (1) who you are, (2) strongest case or projects list.  
Resume and Contact must remain **high-trust, production-parity** surfaces.

---

## Hologram / avatar

Optional companion on About: rotating lines, light Q&A. Must feel on-brand OS, not generic chatbot chrome.

Execution details live in `PORTFOLIO-NEW-INSTRUCTIONS.md` (repo portfolio). This file is the **knowledge** summary for any agent.

*Last updated: 2026-08-23*
