---
name: ux-case-study
description: Structure, write, and build UX portfolio case studies. Use when creating or editing case study pages, writing UX project narratives, planning case study content, or generating HTML/CSS sections for portfolio projects. Activates for Chek, DollarCity, GOES, and similar project documentation.
---

# UX Case Study Skill

Federico is a UX & Product Experience Analyst based in Córdoba, Argentina. His portfolio lives at `fedemon16i.github.io/federico-portfolio` (GitHub Pages). He works on iPad + Magic Keyboard using Claude Code via Safari.

## Core Workflow

**Claude's role**: Prepare detailed prompt documents for Claude Code to execute. Never ask Claude Code to deliberate — all decisions are made upfront in chat.

**Claude Code's role**: Execute without deliberation. Always pushes to new branches (HTTP 403 blocks direct push to main). Federico merges PRs manually on GitHub.

**Deploy cycle**: Claude Code finishes → Federico merges PR → ~2 min deploy.

## Case Study Structure

Every case study follows this hierarchy:

```
1. Hero — Project name, one-line impact statement, role/year/tools tags
2. Context — What was the product, who were the users, what was the business
3. Problem — Specific friction or gap (no vague "I wanted to improve UX")
4. Process — Research → Insight → Design → Validation (with actual methods)
5. Solution — What changed and why it worked
6. Impact — Metrics or observable outcomes (NEVER invent numbers)
7. Reflection — What you'd do differently, what you learned
```

## Federico's Active Projects

### Chek (fintech credit card, Chile/LATAM → acquired by Banco Ripley)
- Hero image: `IMG_0187.jpeg` in `/chek/` folder, used as CSS background-image with glass gradient
- Hi-fi screens: `IMG_0188–0192.jpeg`
- Design System images: `IMG_0193–0194.jpeg`
- Key narrative: SERFINSA onboarding friction fix (external WebView → backend API integration)
- Analytics framework used: HEARTS
- Include: financial education impact, design system contribution

### DollarCity (fraud management SaaS — retail chains, LATAM + Canada)
- Next case study to build — same workflow as Chek
- Key narrative: fraud prevention tooling for operations teams

### GOES (El Salvador customs system)
- Government/enterprise UX
- Focus: complex form flows, operator efficiency

## HTML/CSS Standards

- **CSS-only components preferred** — no images for diagrams, wireframes, mockups
- **No reference/documentation images** alongside sections where content is already coded in HTML
- **No horizontal scroll** — test at 320px minimum width
- **Accessibility**: WCAG AA contrast minimum, focus-visible on all interactive elements, `prefers-reduced-motion` respected
- **No thin typography** — minimum font-weight 400, body text ≥ 16px
- **No frameworks** unless explicitly requested (no Bootstrap, Tailwind, etc.)

## Card Patterns (established)

```
- Carousel on hover (desktop) / swipe (mobile)
- Glass blur overlay
- data-carousel attribute for image paths
- Tags: thematic tags ON image, tool tags BELOW image
- +N tooltip for overflow tags
- Mobile menu requires X close button
```

## Tool Tags (always include these for Federico's projects)

Primary tools to always show: **Figma, Pendo, Maze, Qualtrics, Dynamics, AI Tools**
- Show top 3 in card, rest in +N tooltip
- EY card: add Qualtrics (missing)
- All tools must appear in project detail pages

## Writing Voice

- Analytical and metrics-focused
- First person, present tense for process steps
- Avoid: "I wanted to improve UX", "I noticed that", "I leveraged"
- Use: specific problem descriptions, method names, observable outcomes
- Never invent metrics — use `[METRIC PLACEHOLDER]` if data isn't available

## Prompt Document Format (for Claude Code handoff)

When preparing a prompt document for Claude Code:

```markdown
# Task: [Section Name]

## Context
[File path, branch, what exists currently]

## What to build
[Exact HTML structure, class names, CSS variables to use]

## Copy
[Exact text content — no placeholders unless flagged]

## Assets
[Exact file paths for any images]

## Do NOT
[Specific things to avoid for this task]

## Branch name
feature/[descriptive-name]
```

## Quality Checks Before Handing Off to Claude Code

- [ ] All copy is final (no "lorem ipsum", no invented metrics)
- [ ] All asset paths are confirmed
- [ ] Mobile behavior is specified
- [ ] Accessibility requirements are stated
- [ ] CSS variable names match existing codebase conventions
- [ ] No frameworks introduced without Federico's approval
