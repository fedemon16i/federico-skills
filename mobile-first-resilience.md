---
name: mobile-first-resilience
description: Prevent UI components from breaking on mobile. Use when building tables, data grids, navigation, cards, forms, or any multi-column layout. Activates when a component might overflow, truncate, or become unreadable below 768px. Enforces mobile-first patterns and provides alternatives to horizontal scroll.
---

# Mobile-First Resilience Skill

No component should ever require horizontal scroll to be usable. No text should ever overflow its container. Every layout decision starts at 320px and scales up — never the reverse.

## The Non-Negotiable Rules

```css
/* Global — always present in any project */
*, *::before, *::after {
  box-sizing: border-box;
}

body {
  overflow-x: hidden;
  min-width: 320px;
}

img, video, iframe, table {
  max-width: 100%;
}

p, li, td, th, label, span {
  overflow-wrap: break-word;
  word-break: break-word; /* fallback */
}
```

Never use `width: 100vw` on any element inside the document flow — it ignores scrollbar width and causes overflow.

---

## Tables: The Critical Problem

**A table with 8 columns does NOT fit on mobile. It also barely fits on desktop at < 1200px.** Horizontal scroll is the lazy solution — and the worst UX. Use pattern-matching below.

### Decision Matrix by Column Count

| Columns | Desktop | Mobile Pattern |
|---|---|---|
| 2–3 | Normal table | Normal table |
| 4–5 | Normal table | Stacked cards OR priority columns |
| 6–7 | Normal table with fixed first col | Stacked cards with expand |
| 8+ | **Never a plain table** | Card list + detail drawer OR collapsible rows |

---

### Pattern 1: Stacked Card Table (4–6 columns)

Each row becomes a card. Each cell becomes a label+value pair.

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
.table-card-list { display: flex; flex-direction: column; gap: var(--space-card-gap); }

.table-card {
  display: grid;
  grid-template-columns: 1fr 1fr; /* 2-up label/value pairs */
  gap: 0.5rem 1rem;
  padding: 1rem;
  border-radius: var(--radius-card);
  background: var(--color-surface-glass);
}

.cell-label {
  font-size: 0.75rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  color: var(--color-text-muted);
}

.cell-value {
  font-size: 1rem;
  font-weight: 400;
  color: var(--color-text-primary);
}

/* Desktop: show as actual table */
@media (min-width: 768px) {
  .table-card-list { display: table; width: 100%; }
  .table-card { display: table-row; }
  .table-cell { display: table-cell; padding: 0.75rem 1rem; }
  .cell-label { display: none; } /* headers in <thead> instead */
}
```

---

### Pattern 2: Priority Columns + "Show More" (6–8 columns)

Show only the 2–3 most important columns on mobile. User taps to expand the row.

```html
<table class="table-priority">
  <thead>
    <tr>
      <th>Name</th>
      <th>Status</th>
      <th class="col-secondary">Date</th>
      <th class="col-secondary">Amount</th>
      <th class="col-secondary">Category</th>
      <th></th> <!-- expand toggle -->
    </tr>
  </thead>
  <tbody>
    <tr class="row-collapsible">
      <td>Card payment</td>
      <td><span class="badge">Active</span></td>
      <td class="col-secondary">Mar 5</td>
      <td class="col-secondary">$240</td>
      <td class="col-secondary">Food</td>
      <td><button class="row-expand-btn" aria-expanded="false" aria-label="Show more details">+</button></td>
    </tr>
    <tr class="row-details" hidden>
      <td colspan="6">
        <!-- Secondary info rendered as a mini card -->
        <dl class="row-detail-grid">
          <dt>Date</dt><dd>Mar 5</dd>
          <dt>Amount</dt><dd>$240</dd>
          <dt>Category</dt><dd>Food</dd>
        </dl>
      </td>
    </tr>
  </tbody>
</table>
```

```css
@media (max-width: 767px) {
  .col-secondary { display: none; }

  .row-detail-grid {
    display: grid;
    grid-template-columns: auto 1fr;
    gap: 0.25rem 1rem;
    padding: 0.75rem;
  }
}
```

---

### Pattern 3: Card List + Detail Drawer (8+ columns, complex data)

The table disappears entirely on mobile. Rows become summary cards. Tap opens a drawer/panel with full detail.

**Use this for**: transaction lists, fraud event logs, user management tables, audit trails.

```html
<!-- Mobile: card list -->
<ul class="card-list" role="list">
  <li class="summary-card" data-detail-id="txn-001">
    <div class="card-primary">
      <span class="card-title">Card payment — Mar 5</span>
      <span class="card-badge status-active">Active</span>
    </div>
    <div class="card-secondary">$240 · Food</div>
    <button class="card-detail-btn" aria-haspopup="dialog">View details</button>
  </li>
</ul>

<!-- Detail drawer -->
<div class="detail-drawer" role="dialog" aria-modal="true" aria-label="Transaction detail" hidden>
  <button class="drawer-close" aria-label="Close">✕</button>
  <dl class="detail-list">
    <!-- All 8+ fields here, full labels, readable layout -->
  </dl>
</div>
```

---

## Text Overflow: Critical Patterns

### Long strings (URLs, emails, codes, file names)

```css
.truncate-single {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  max-width: 100%;
}

/* Always pair with title attribute for full value */
/* <span class="truncate-single" title="full@email.com">full@email.com</span> */
```

### Long natural text (names, descriptions, addresses)

```css
.wrap-natural {
  overflow-wrap: break-word;
  hyphens: auto; /* requires lang attribute on <html> */
}
```

### Fixed-width containers (badges, tags, chips)

```css
.badge {
  max-width: 12ch; /* character-based max */
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
```

### Numbers and codes (never break mid-number)

```css
.no-break {
  white-space: nowrap;
}
/* Use for: phone numbers, IDs, prices, dates, percentages */
```

---

## Critical Text Cases: Suggestions

| Scenario | Problem | Solution |
|---|---|---|
| Product name > 30 chars | Overflows card | Truncate with ellipsis + title attr |
| Amount in non-Latin numerals | Direction issues | `dir="ltr"` on numeric cells |
| Email in narrow column | No break point | `word-break: break-all` scoped to `.email-cell` |
| Long URL in body text | Breaks layout | `overflow-wrap: anywhere` + no underline decoration |
| Name in ALL CAPS | Hard to read at small size | Avoid CSS `text-transform: uppercase` on names |
| Multi-line badge/tag | Breaks chip shape | `white-space: nowrap` + max-width + tooltip |
| Address block | Unpredictable length | Fixed 3-line clamp with expand |

```css
/* 3-line clamp pattern */
.text-clamp-3 {
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
```

---

## Navigation on Mobile

```css
/* No horizontal nav on mobile — ever */
@media (max-width: 767px) {
  nav ul {
    flex-direction: column; /* or use hamburger pattern */
  }
}
```

Mobile menu **must** have:
- Visible X close button (not just "tap outside")
- `aria-expanded` on toggle button
- Focus trap inside open menu
- `Escape` key closes it

---

## Testing Checklist

Before shipping any layout:

- [ ] Tested at 320px width (iPhone SE / smallest viewport)
- [ ] No horizontal scrollbar at any breakpoint
- [ ] No text overflows its container
- [ ] Tables have a defined mobile pattern (not `overflow-x: scroll`)
- [ ] Long strings truncate gracefully with `title` fallback
- [ ] Touch targets ≥ 44×44px
- [ ] Zoom to 200% — does layout hold?
- [ ] Tested with actual content (not Lorem Ipsum — real long names/numbers)
