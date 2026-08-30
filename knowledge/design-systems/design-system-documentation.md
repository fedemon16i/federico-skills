# Design System Documentation — Best Practices 2026

**Status:** Done · **Priority:** High  
**Use when:** Documenting a component, building a token system, structuring a DS for LLM agents to apply correctly.

---

## The Storybook Graveyard Problem

Most DS docs fail not because of wrong tools, but wrong ownership. Two root causes:

1. **No DRI** — documentation is everyone's job, so it's no one's job. Each component needs one owner who ships both the code and the doc.
2. **Describing instead of deciding** — "The button has three variants" is observation. "Use `primary` for one action per screen; use `ghost` only in toolbars where context already signals the hierarchy" is a decision. Only decisions survive.

**Fixes:**
- Co-locate docs with code (MDX in Storybook, or markdown alongside the component file)
- Lint for missing docs in CI — if a component ships without a doc, the PR fails
- Archive unused stories rather than letting them drift out of sync

---

## Token Naming — DTCG or Simple?

**2025 standard:** [DTCG format](https://design-tokens.github.io/community-group/) for any system that needs tool interop (Figma Tokens plugin, Style Dictionary, Theo). JSON with `$value`, `$type`, `$description` keys.

**When to use component-level tokens:** only in public design systems (Material, shadcn) where consumers need to theme individual components. For internal systems, stop at semantic tokens — the extra abstraction costs more than it saves.

**Tier table:**

| Tier | Example | Owner |
|---|---|---|
| Raw/primitive | `--color-purple-500: #7c3aed` | Design |
| Semantic | `--color-action: var(--color-purple-500)` | Design + Engineering |
| Component | `--button-primary-bg: var(--color-action)` | Component owner |

**Naming convention:** `{category}-{property}-{variant}-{state}`  
`--text-primary`, `--bg-surface-raised`, `--border-interactive-hover`

---

## What Top Design Systems Share

Distilled from Radix UI, shadcn/ui, Material Design 3, Apple HIG, Atlassian:

**Shared skeleton — in this order:**
1. **Anatomy** — labeled diagram of the component parts
2. **Behavior** — how it responds to interaction (click, keyboard, disabled, loading) — *this is the primary documentation target, not appearance*
3. **When NOT to use** — the constraint that makes the "when to use" meaningful
4. **Accessibility** — keyboard nav, ARIA roles, screen reader behavior
5. **Code** — usage snippet, props table, variants

The order is intentional: behavior before code, constraints before examples.

---

## Single Source of Truth — Ownership Split

Three sources, each with explicit ownership:

| Source | Owns | Does not own |
|---|---|---|
| **Figma** | Visual spec: spacing, color, typography, layout | Interaction behavior, state logic |
| **Code** | Behavior: state machine, keyboard, animation timing | Why decisions were made |
| **Docs** | Rationale: why this pattern, when to use it, what it replaces | Implementation details |

Figma Code Connect (2024+) reduces but doesn't eliminate drift. The rule: if it can be derived from the code, it should be — don't manually sync what tooling can automate.

---

## Component Documentation Structure

Every component doc has these 8 items in order:

1. **One-line description** — what it is, not what it looks like
2. **Anatomy** — labeled parts (header, body, action, dismiss, etc.)
3. **Variants** — table with name + when to use + when not to use
4. **States** — default, hover, active, focus, disabled, loading, error
5. **Do / Don't** — visual pairs, not prose; maximum 4 pairs
6. **Accessibility** — keyboard navigation, ARIA role, screen reader output
7. **Code** — minimal usage snippet; props table only if >3 props
8. **Changelog** — date + what changed + who changed it

---

## Storybook vs. Plain HTML

| Use Storybook | Use plain HTML |
|---|---|
| Team of 3+ engineers | Solo or 2-person team |
| Design tokens need live preview | Tokens are simple enough to scan |
| You have a dedicated DS maintainer | DS is maintained alongside product work |
| Reviewers need interactive controls | A screenshot + code snippet is enough |
| You'll maintain it for 2+ years | You'll ship it and iterate as needed |

Plain HTML demos are underrated for portfolio/product DS work — zero maintenance overhead, Playwright-testable, and they actually load.

---

## Tokens Workflow — Figma to CSS

```
Figma Variables
    ↓ (export via Tokens Studio or Figma Variables REST API)
tokens.json  (DTCG format)
    ↓ (Style Dictionary transforms)
tokens.css  (CSS custom properties)
    ↓ (imported in component stylesheets)
Component uses var(--token-name)
```

**The PR-the-diff practice:** whenever tokens change, the CSS diff in the PR should be human-readable — a reviewer should be able to see `--color-action: #7c3aed → #6d28d9` without decoding the build. If the diff is unreadable, the pipeline is too opaque.

---

## LLM-Friendly Documentation (2026)

Design system docs written for humans fail when an agent reads them — they rely on implication and visual context. To write docs an LLM can apply correctly:

1. **Explicit rules over implicit convention** — "Never use `ghost` without a surrounding container that provides contrast" not "use ghost contextually"
2. **Named prohibitions** — list what this component must NOT do; agents follow constraints more reliably than suggestions
3. **Rationale separated from constraints** — tag rationale as `[WHY]` and constraints as `[RULE]` so an agent can skip rationale when applying rules
4. **Predictable structure** — same 8 items in the same order for every component; agents navigate by structure, not by reading prose
5. **No visual-only cues** — "see diagram" breaks in text context; write the rule in prose even if the diagram also shows it
6. **State machine language** — "when X, do Y" beats "in the active state" — conditional logic is more parseable than state labels

---

## Component Documentation Template

```markdown
## ComponentName

**What it is:** one sentence. Not what it looks like — what it does.

### Anatomy
- **Part A:** description
- **Part B:** description

### Variants

| Variant | Use when | Do not use when |
|---|---|---|
| primary | ... | ... |
| secondary | ... | ... |

### States
- **Default:** ...
- **Hover:** ...
- **Focus:** visible ring, 2px offset, `--color-focus`
- **Disabled:** 40% opacity, cursor not-allowed, no interaction
- **Loading:** spinner replaces label, width locked to prevent reflow

### Do / Don't
- ✅ Do: ...
- ❌ Don't: ...

### Accessibility
- **Keyboard:** Tab to focus, Enter/Space to activate
- **ARIA role:** `button` (or `link` if it navigates)
- **Screen reader:** reads label text; avoid icon-only without `aria-label`
- **Focus visible:** always — never remove outline without a visible replacement

### Code
```tsx
<Button variant="primary" onClick={handleClick}>
  Label
</Button>
```

### Changelog
| Date | Change | Author |
|---|---|---|
| 2026-08-30 | Initial | — |
```

---

*Last updated: 2026-08-30*
