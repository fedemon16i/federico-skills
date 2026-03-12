---
name: mobile-first-resilience
description: Prevent UI components from breaking on mobile. Use when building tables, data grids, navigation, cards, forms, or any multi-column layout. Activates when a component might overflow, truncate, or become unreadable below 768px. Enforces mobile-first patterns and provides alternatives to horizontal scroll. ALWAYS read this skill when touching any table, card grid, or multi-column section in the Federico Monroy portfolio.
---

# Mobile-First Resilience Skill

No component should ever require horizontal scroll to be usable. No text should ever overflow its container. Every layout decision starts at 320px and scales up — never the reverse.

---

## Portfolio Tables — Known Instances (apply pattern every time)

| File | Table | Columns | Required Pattern |
|---|---|---|---|
| `projects/chek.html` | HEARTS framework table | 6 | Pattern 1: Stacked Card |
| `projects/dollarcity.html` | Types of Users | 5 | Pattern 1: Stacked Card |
| `projects/dollarcity.html` | Selecting Solutions | 4 | Pattern 1: Stacked Card |
| `projects/dollarcity.html` | Problems Found | 3 | Normal table — verify padding |

When editing any of these files, **always check these tables and apply the correct pattern if not already applied.**

---

## The Non-Negotiable Rules

```css
*, *::before, *::after { box-sizing: border-box; }

body {
  overflow-x: hidden;
  min-width: 320px;
}

img, video, iframe, table { max-width: 100%; }

p, li, td, th, label, span {
  overflow-wrap: break-word;
  word-break: break-word;
}
```

Never use `width: 100vw` on any element inside the document flow.

---

## Decision Matrix by Column Count

| Columns | Desktop | Mobile Pattern |
|---|---|---|
| 2–3 | Normal table | Normal table |
| 4–5 | Normal table | **Pattern 1: Stacked Card** |
| 6–7 | Normal table | **Pattern 1: Stacked Card** |
| 8+ | Never a plain table | **Pattern 3: Card List + Drawer** |

---

### Pattern 1: Stacked Card Table ← DEFAULT for this portfolio

```html
<div class="table-card-list" role="table" aria-label="[Description]">
  <div class="table-card" role="row">
    <div class="table-cell" role="cell">
      <span class="cell-label">Status</span>
      <span class="cell-value">Active</span>
    </div>
    <div class="table-cell" role="cell">
      <span class="cell-label">Amount</span>
      <span class="cell-value">$1,200</span>
    </div>
  </div>
</div>
```

```css
/* Mobile first */
.table-card-list {
  display: flex;
  flex-direction: column;
  gap: var(--space-16);
}

.table-card {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 0.5rem 1rem;
  padding: var(--space-20);
  border-radius: 8px;
  background: var(--bg-surface);
  border: 1px solid var(--border);
}

.cell-label {
  font-size: 0.65rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.12em;
  color: var(--text-secondary);
  margin-bottom: 2px;
}

.cell-value {
  font-size: 0.9rem;
  font-weight: 400;
  color: var(--text-primary);
  line-height: 1.5;
}

/* Desktop: restore as actual table */
@media (min-width: 768px) {
  .table-card-list { display: table; width: 100%; border-collapse: collapse; }
  .table-card { display: table-row; background: transparent; border: none; padding: 0; }
  .table-cell { display: table-cell; padding: 14px 20px; border-bottom: 1px solid var(--border); vertical-align: top; line-height: 1.6; }
  .cell-label { display: none; }
}
```

---

### Pattern 2: Priority Columns + "Show More" (6–8 columns)

```css
@media (max-width: 767px) {
  .col-secondary { display: none; }
  .row-detail-grid {
    display: grid;
    grid-template-columns: auto 1fr;
    gap: 0.25rem 1rem;
    padding: var(--space-16);
  }
}
```

---

### Pattern 3: Card List + Detail Drawer (8+ columns)

Table disappears on mobile. Rows become summary cards. Tap opens a drawer.

```html
<ul class="card-list" role="list">
  <li class="summary-card">
    <div class="card-primary">
      <span class="card-title">Row title</span>
      <span class="card-badge">Status</span>
    </div>
    <div class="card-secondary">Secondary info</div>
    <button class="card-detail-btn" aria-haspopup="dialog">View details</button>
  </li>
</ul>
<div class="detail-drawer" role="dialog" aria-modal="true" hidden>
  <button class="drawer-close" aria-label="Close">✕</button>
  <dl class="detail-list"></dl>
</div>
```

---

## Cards: Mobile Rules

```css
.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: var(--space-24);
  align-items: stretch;
}

@media (max-width: 767px) {
  .card-grid { grid-template-columns: 1fr; }
}

.card {
  display: flex;
  flex-direction: column;
  gap: var(--space-12);
  padding: var(--space-20); /* ALL 4 sides — no exceptions */
  min-height: 0;
}

.card img {
  width: 100%;
  height: 200px;
  object-fit: cover;
  display: block;
}
```

Card padding rules:
- Minimum `var(--space-20)` on all 4 sides
- Never `padding: 20px 20px 0` or any shorthand that drops vertical
- Left-border accent cards: `padding-left: var(--space-20)` on inner content

---

## Project Hero — Mobile Rules

```css
.project-hero {
  min-height: 60vh;
  padding: var(--space-48) var(--space-24);
}

@media (max-width: 767px) {
  .project-hero {
    padding: var(--space-32) var(--space-16);
    background-position: center center;
  }
  .project-title {
    font-size: clamp(2rem, 8vw, 4rem);
  }
  .project-stat-row {
    grid-template-columns: 1fr 1fr;
    gap: var(--space-16);
  }
}
```

---

## Tag Chips — Mobile Rules

```css
.tags {
  display: flex;
  flex-wrap: wrap; /* NEVER nowrap */
  gap: 8px;
}
.tag-chip {
  white-space: nowrap;
  flex-shrink: 0;
}
```

---

## Text Overflow Patterns

```css
.truncate-single {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  max-width: 100%;
}
/* Always add title attribute for full value */

.wrap-natural {
  overflow-wrap: break-word;
  hyphens: auto;
}

.text-clamp-3 {
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.no-break { white-space: nowrap; }
/* Use for: prices, dates, IDs, percentages */
```

---

## Spacing at Mobile

```css
@media (max-width: 767px) {
  .project-section { margin-bottom: var(--space-40); }
  .container { padding-inline: var(--space-16); }
}

@media (max-width: 480px) {
  .card, .panel, .box { padding: var(--space-16); }
}
```

---

## Navigation on Mobile

Mobile menu must have:
- X close button inside panel: `position:absolute; top:16px; right:16px`
- `aria-expanded` on hamburger
- `position:fixed; inset:0; height:100dvh; overflow-y:auto`
- Focus trap + Escape key closes

---

## Pre-Ship Testing Checklist

- [ ] Tested at 320px (iPhone SE)
- [ ] No horizontal scrollbar at any breakpoint
- [ ] No text overflows container
- [ ] All tables use a defined mobile pattern — never `overflow-x: scroll`
- [ ] Card padding ≥ var(--space-20) all 4 sides
- [ ] Tag chips wrap correctly
- [ ] Hero background-position adjusted for mobile
- [ ] Touch targets ≥ 44×44px
- [ ] Zoom to 200% — layout holds
- [ ] **Portfolio-specific:** HEARTS (chek.html), Types of Users + Selecting Solutions (dollarcity.html) use Pattern 1
