# Onboarding & Activation Patterns

**Status:** Done · **Priority:** High  
**Use when:** First-run, account opening, card product onboarding, “empty product” risk (Chek-class, SaaS aha).

---

## Jobs of onboarding

1. Reach **aha** (user does the core job once)  
2. Establish **trust** (fintech / health / gov)  
3. Collect **only** what the next step needs  
4. Teach **in context**, not a 12-slide tour  

---

## Patterns from strong products

| Pattern | Practice |
|---------|----------|
| **Aha-first** | Sample data or first real action in ≤2 meaningful steps (Linear/Figma class) |
| **Progressive setup** | Minimal day-one; power features later (Notion/Slack class) |
| **Checklist with value** | “Do X to unlock Y” — not busywork |
| **OCR / prefill** | KYC and ID: reduce typing |
| **Biometrics early** | Fintech return visits |
| **Education embedded** | Tooltips / short tips at decision points (financial literacy) — not a separate course wall |
| **Skip where safe** | Optional personalization after core path |

---

## Fintech / card / account specifically

- One primary CTA per screen  
- Pending / verification states explicit  
- Microcopy calm and specific  
- Onboarding success = first successful action (transfer, card view, bill pay) — define it with product  

Chek-class narrative: modernize card-adjacent UI + account/debit flows + **financial education** in flow (see also brand links like “corta y clara” as *outcome of education intent*, not a feature dump).

---

## Tipos de guía en Pendo (implementación táctica)

| Tipo | Cuándo usarlo |
|------|--------------|
| **Tooltip** | Brief info anclada a un elemento; responde FAQs en contexto |
| **Lightbox** | Notificaciones importantes que requieren acknowledgement (inicio de onboarding, cambios mayores) |
| **Walkthrough** | Multi-step para completar un workflow completo (ej: setup de perfil post-login) |
| **Resource Center** | On-demand: release notes, tutorial videos, walkthroughs accesibles en cualquier momento desde el producto |

**Personalization triggers:** metadata del visitor (rol, plan, industry) + respuestas a polls in-app → diferentes flows por segmento. No one-size-fits-all.

---

## Benchmarks reales (Pendo data)

| Empresa | Intervención | Resultado |
|---------|-------------|-----------|
| UserTesting | Onboarding personalizado por nivel de experiencia | **29% más usuarios llegando al "Draft Test"** (activation event) |
| GoTo / Grasshopper | Guides basados en features que correlacionan con conversión | **4% ↑ trial-to-paid → $130k adicionales** |
| Cin7 | Tour personalizado por metadata + industry (Pendo polls) | **65% completaron tour; 50% faster time-to-value (3d → 1.5d); 75% más likely to convert** |
| Essity | Onboarding para empleados con guides contextuales | **1,200 empleados onboarded en 38 países** |

**Lectura de estos números:** la personalización (rol, industria, comportamiento previo) es el multiplicador — no el número de pasos.

---

## Anti-patterns

- Empty dashboard con ningún path de sample  
- Pedir 20 campos de perfil antes del valor  
- Tour overlay que bloquea la UI real  
- Mismo onboarding para todos los roles  
- Pedir feedback cuando el usuario está en mitad del flow (esperar a que complete un paso)

---

## Measure

Activation rate to agreed aha · time-to-aha · drop by onboarding step · support contacts in first 7 days  
Guide effectiveness: `guide_view → guide_cta_click → downstream action` (el evento que define aha)  

*Last updated: 2026-08-23*
