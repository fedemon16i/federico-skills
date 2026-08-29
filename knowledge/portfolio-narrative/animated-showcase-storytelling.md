# Animated Showcase Storytelling

**Status:** Done · **Priority:** High  
**Use when:** Structuring an animated beat sequence as a narrative — deciding what to show, what order, what copy accompanies each beat, and how to close.

Complements `case-study-structure-2026.md` (written case study) and `product-ui-animation-showcase.md` (motion mechanics).

---

## Core insight

An animated UI showcase without narrative structure is a demo reel. The viewer sees motion but doesn't understand:
- What changed
- Why it mattered
- What the designer's role was

**The arc across beats IS the story.** Each beat is one moment in that arc — not an independent demo.

---

## Three storytelling frameworks

### PFFR — Problem / Friction / Fix / Result

Best for: most projects. Maps directly to beats (each stage = one or two beats).

```
P — Problem
   What was broken from the user's perspective? (one sentence, no jargon)
   What was the business consequence?

F — Friction
   Where specifically did users fail?
   What did the data show? (drop-off, rage clicks, session length, NPS)

F — Fix
   What decision was made and why?
   What was NOT chosen and why?

R — Result
   What measurably changed?
   Real number or honest range — never invented precision
```

### SCQA (Minto Pyramid) — Situation / Complication / Question / Answer

Best for: complex projects where the brief itself changed during research.

```
S — Situation      → what everyone agreed on (no drama yet)
C — Complication   → what was discovered that contradicted it (the pivot)
Q — Question       → what question that forced ("Are we solving the right problem?")
A — Answer         → the design response to that question
```

Use for: senior-register portfolios, projects where brief-shaping is part of the story.

### Design Spine — Worst Moment / Intervention / Systemic Change

Best for: strategic / organizational work (design systems, research that changed roadmap direction).

```
Worst Moment      → one specific painful user instance ("A store manager in Medellín had to...")
Intervention      → the one design decision that addressed it (specific, not a list)
Systemic Change   → how it changed the system beyond the screen
                    ("This led to a weekly instrumentation review across 3 teams")
```

Use for: principal/director-level register; feature-level UI work doesn't need this.

---

## Beat pacing rules

| Element | Duration | Rule |
|---------|----------|------|
| Problem frame (static, text) | 3–5s | No animation — text-first |
| First beat | 4–8s | One thing happens, one thing settles |
| Transition between beats | 0.3–0.5s | Cross-fade; slide only if spatial (step 2 of a flow) |
| Annotation callout | 2–4s visible | Appears → readable → disappears |
| Max single beat | 12s | If longer, split |
| Total per showcase section | < 90s | Over 90s = viewer won't reach the result |

---

## The annotated demo pattern

Presentation layer OVER real UI — not part of the product.

```css
.annotation {
  position: absolute;
  pointer-events: none;
  opacity: 0;
  transition: opacity 200ms ease-out;
}
.annotation.visible { opacity: 1; }
```

Timeline:
```
REAL UI plays → freezes → annotation appears → holds 2s → disappears → interaction continues
```

JS integration:
```js
// Appears after the animated element has settled
setTimeout(() => {
  document.querySelector('.annotation-kpi').classList.add('visible');
  setTimeout(() => document.querySelector('.annotation-kpi').classList.remove('visible'), 2000);
}, 3500); // 3.5s after beat starts = after KPI value settles
```

Reference: this is how Figma release pages and Stripe product pages do it.

---

## What recruiters actually scan

**First 5 seconds:**
1. What do you do?
2. What seniority?
3. What domain?

If they can't tell from the opening frame, the showcase fails before it plays.

**30 seconds of real attention:** opening problem statement → evidence → result → role attribution.

**2025–26 shift:** AI screening tools parse heading structure. Semantic HTML and clear `<h1>` copy matter more than before.

---

## AI-augmented work attribution (2026 expectation)

By 2026, showing AI in the workflow is expected. Attribution matters more than the tool.

**Do:**
- Name specifically where AI was used and for what
- Show what you changed from AI's output and why
- Frame as tool proficiency, not labor reduction

**Don't:**
- Use "AI-assisted" as a blanket disclaimer — implies you can't explain the process
- Claim AI decisions as design decisions — the decision was yours; the tool was a means
- Hide it — the question is now routine in interviews

---

## NDA'd work — the playbook

1. Get permission (even verbal "you can show the process")
2. Swap client identity → generic descriptor ("a Latin American retail network")
3. Change exact numbers → honest ranges ("~30% improvement")
4. Password-protect what's sensitive
5. Focus on process and decision rationale, not screen copy
6. Abstract the UI surface without changing the interaction pattern

**The interaction pattern is the work. The brand is not.**

---

## Copy red flags (catch before anything ships)

- "leveraged" → say what you actually did
- "holistic approach" → what specifically?
- "synergy" / "user-centric" → always delete
- Invented precision ("29.7% improvement") → use range
- Any claim that can't be sourced → remove

---

## Voice markers (Federico's)

- Impersonal / descriptive tone — no "I/yo" in copy
- Real numbers or honest ranges
- Tool names are instruments, not identity
- "Measured outcomes" phrasing, not specific metric tool names
- No buzzwords

---

## Links

- Motion mechanics: `ui-patterns/product-ui-animation-showcase.md`
- Case study written structure: `case-study-structure-2026.md`
- NDA detail: `confidential-and-nda-cases.md`
- Verbal interview: `interview-project-narrative.md`

*Last updated: 2026-08-29*
