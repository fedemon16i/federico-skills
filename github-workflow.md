---
name: github-workflow
description: Gestiona el flujo de trabajo de GitHub para el portfolio de Federico Monroy. Usar siempre que se mencionen branches, PRs, push, merge, git, deploy, "subir cambios", "aplicar cambios", "ver en vivo", o cuando Claude Code termine una tarea y haya que publicar. También activa cuando el usuario pregunta "¿cómo subo esto?", "¿cómo lo mergeo?", "¿no puedo pushear a main?" o cualquier variante de problema de deploy en GitHub Pages.
---

# GitHub Workflow — Federico Monroy Portfolio

## Contexto del repo
- **Repo:** `github.com/fedemon16i/federico-portfolio`
- **Hosting:** GitHub Pages — rama `main`
- **Restricción crítica:** GitHub Pages bloquea push directo a `main` (HTTP 403)
- **Consecuencia:** Claude Code SIEMPRE trabaja en un branch nuevo y abre un PR

## Skills disponibles del usuario

Todos los skills de Federico están en `/mnt/skills/user/`. Claude Code los tiene disponibles automáticamente. Al armar un prompt, indicar cuáles son relevantes para la tarea:

| Skill | Cuándo incluirlo en el prompt |
|---|---|
| `ux-case-study` | Crear o editar páginas de caso de estudio |
| `ux-storytelling` | Escribir narrativa UX, copy de portfolio |
| `behavioral-analytics` | Secciones de analytics, Pendo, Mixpanel |
| `design-system-discipline` | Agregar componentes, tokens CSS, consistencia visual |
| `mobile-first-resilience` | Tablas, grids, cards, cualquier layout multi-columna |
| `accessibility-standards` | Contraste, focus, ARIA, cualquier componente interactivo |
| `github-workflow` | Este mismo skill — flujo de branch y PR |

---

## Regla de oro para Claude Code

```
NUNCA pushear a main.
SIEMPRE crear un branch nuevo con nombre único.
Al terminar: git push origin HEAD
```

Incluir esto textualmente en cualquier prompt para Claude Code.

---

## Formato de nombre de branch

```
<tipo>/<descripcion-corta>
```

| Tipo | Cuándo usarlo |
|---|---|
| `fix/` | Bug fix, imagen rota, padding, color |
| `feat/` | Sección nueva, proyecto nuevo |
| `content/` | Cambio de copy, métricas, textos |
| `style/` | CSS, spacing, tipografía |
| `chore/` | Limpieza, renombrar archivos, README |

**Ejemplos válidos:**
- `fix/forecast-hero-image`
- `feat/blockchain-case-study`
- `content/chek-metrics-update`
- `style/card-padding-fix`

**Nunca reusar un branch anterior** — auto-delete está activado en el repo, pero igual generar nombre único por sesión.

---

## Bloque git estándar para Claude Code

Agregar al final de CADA prompt:

```markdown
## Git instructions
- Crear branch: `<tipo>/<nombre-descriptivo>` — NUNCA main
- Al terminar: `git add -A && git commit -m "<tipo>(<scope>): descripción" && git push origin HEAD`
- Reportar el nombre exacto del branch al terminar
```

### Formato de commit message
```
fix(forecast): replace broken pdf src with Hero.png
feat(blockchain): add hero section with case study assets
style(cards): normalize card image height to 280px
content(dollarcity): update task time metric to 50%
```

---

## Flujo completo — paso a paso

### Claude Code termina →
1. Claude Code dice el nombre del branch (ej: `fix/forecast-hero-image`)
2. Ir a: `github.com/fedemon16i/federico-portfolio/pulls`
3. Click en **"Compare & pull request"**
4. Revisar el diff
5. Click **"Merge pull request"** → **"Confirm merge"**
6. Esperar **~2 minutos**
7. Verificar en: `fedemon16i.github.io/federico-portfolio`

### Si no aparece el PR automáticamente:
1. Ir a `github.com/fedemon16i/federico-portfolio/branches`
2. Buscar el branch por nombre
3. Click **"New pull request"** → merge

---

## Checklist pre-merge

- [ ] Solo se modificaron los archivos esperados
- [ ] No hay rutas rotas ni `png2pdf.pdf`
- [ ] No se tocó nav, footer, ni `shared.css` sin haberlo pedido
- [ ] No hay `font-weight: 300` nuevo
- [ ] No hay colores hardcodeados fuera de CSS vars

---

## Errores comunes y soluciones

| Error | Causa | Fix |
|---|---|---|
| `403 forbidden` en push | Intentó pushear a `main` | Verificar que use `git push origin HEAD` desde un branch |
| PR no aparece | Branch no fue pusheado | Ir a /branches y crear PR manualmente |
| Cambios no se ven tras merge | GitHub Pages tarda | Esperar 2 min más, hard refresh (Cmd+Shift+R) |
| Branch ya existe | Reutilización de nombre | Agregar sufijo con fecha o número |
| Imagen no carga tras deploy | Path case-sensitive | Verificar mayúsculas exactas del filename |

---

## Nota sobre paths de imágenes

GitHub Pages es **case-sensitive**:
- ✅ `../forecast/Hero.png`
- ❌ `../forecast/hero.png`
- ❌ `../forecast/HERO.PNG`
