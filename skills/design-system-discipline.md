---
name: design-system-discipline
description: Enforce design system consistency, prevent refactors, manage CSS tokens and component patterns. Use when adding new UI sections, building components, reviewing existing code for drift, or planning design system documentation. Activates when building portfolio pages or any multi-section HTML/CSS project.
---

# Design System Discipline Skill

This skill prevents the #1 portfolio killer: **incremental drift** — where each new section introduces slightly different spacing, colors, or type scales that eventually require a full refactor.

## The Core Rule

**Before writing any new CSS: check what already exists.**

New code must use existing CSS variables. New variables may only be added if there is genuinely no existing token that serves the purpose — and even then, they must follow the naming convention.

## Federico's Portfolio Token Conventions

When in doubt, check `style.css` or the root `:root {}` block first.

### Naming Pattern
```css
--[category]-[variant]-[modifier]

/* Examples */
--color-primary
--color-surface-glass
--space-section
--space-card-gap
--type-size-body
--type-weight-heading
--radius-card
--radius-pill
--shadow-card
--shadow-hero
--motion-duration-default
--motion-easing-default
```

### Categories
- `--color-*` — all colors, including backgrounds, borders, text
- `--space-*` — spacing, gap, padding, margin values
- `--type-*` — font-size, font-weight, line-height
- `--radius-*` — border-radius values
- `--shadow-*` — box-shadow values
- `--motion-*` — transition durations and easings
- `--z-*` — z-index layers

## Before Adding Any New Section

Run this mental checklist:

1. **Color**: Does the color I need have a variable? → Use it. No? → Is it a variant of an existing one? → Add with systematic name. First use? → Define in `:root`.
2. **Spacing**: Does `--space-section` or `--space-card-gap` already work? → Use it. Creating custom spacing? → Ask: will this need to be consistent anywhere else? If yes → tokenize.
3. **Typography**: Is there already a class for this text style? → Use it. Don't create inline `font-size` or `font-weight` declarations.
4. **Component pattern**: Does a similar component exist in the codebase? → Copy its structure, don't reinvent.

## Anti-Patterns to Prevent

```css
/* ❌ NEVER — hardcoded values */
padding: 48px;
color: #1a1a2e;
border-radius: 12px;
font-size: 14px;
font-weight: 300;

/* ✅ ALWAYS — tokenized */
padding: var(--space-section);
color: var(--color-text-primary);
border-radius: var(--radius-card);
font-size: var(--type-size-small);
font-weight: var(--type-weight-body);
```

## CSS Component Checklist (every new component)

- [ ] Uses only existing CSS variables (or adds new ones to `:root`)
- [ ] Has a clear, reusable class name
- [ ] Mobile-first: works at 320px without horizontal scroll
- [ ] Focus states: `:focus-visible` styled on all interactive elements
- [ ] Reduced motion: `@media (prefers-reduced-motion: reduce)` applied
- [ ] Contrast: WCAG AA minimum
- [ ] Font weight: nothing below 400
- [ ] No inline styles

## When You MUST Refactor

Refactor is justified only when:
1. A token is used with 5+ different hardcoded overrides across files
2. A component structure has forked into 3+ incompatible variants
3. A new section can't be built without contradicting an existing pattern

In these cases: refactor the token/component globally, don't add another variant.
