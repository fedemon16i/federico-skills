---
name: github-workflow
description: Gestiona el flujo de trabajo de GitHub para el portfolio de Federico Monroy. Usar siempre que se mencionen branches, PRs, push, merge, git, deploy, "subir cambios", "aplicar cambios", "ver en vivo", o cuando Claude Code termine una tarea y haya que publicar. Incluye: crear branches correctos, armar el prompt para Claude Code con instrucciones git, y el checklist post-merge. También activa cuando el usuario pregunta "¿cómo subo esto?", "¿cómo lo mergeo?", "¿no puedo pushear a main?" o cualquier variante de problema de deploy en GitHub Pages.
---

# GitHub Workflow — Federico Monroy Portfolio

## Contexto del repo
- **Repo:** `github.com/fedemon16i/federico-portfolio`
- **Hosting:** GitHub Pages — rama `main`
- **Restricción crítica:** GitHub Pages bloquea push directo a `main` (HTTP 403)
- **Consecuencia:** Claude Code SIEMPRE trabaja en un branch nuevo y abre un PR

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

```
## Git
- Branch: `<tipo>/<nombre-unico>`
- Comandos al terminar:
  git add -A
  git commit -m "<tipo>(<scope>): <descripción corta en inglés>"
  git push origin HEAD
- NO pushear a main
- NO reusar branches de sesiones anteriores
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
3. Aparece el PR automáticamente — hacer click en **"Compare & pull request"**
4. Revisar el diff rápidamente
5. Click **"Merge pull request"** → **"Confirm merge"**
6. Esperar **~2 minutos**
7. Verificar en: `fedemon16i.github.io/federico-portfolio`

### Si no aparece el PR automáticamente:
1. Ir a `github.com/fedemon16i/federico-portfolio/branches`
2. Buscar el branch por nombre
3. Click **"New pull request"** → merge

---

## Checklist pre-merge

Antes de mergear, revisar en el diff de GitHub:

- [ ] Solo se modificaron los archivos esperados
- [ ] No hay `png2pdf.pdf` ni rutas rotas nuevas
- [ ] No se tocó el nav, footer, ni shared.css sin haberlo pedido
- [ ] No hay `font-weight: 300` nuevo
- [ ] No hay colores hardcodeados fuera de CSS vars

---

## Errores comunes y soluciones

| Error | Causa | Fix |
|---|---|---|
| `403 forbidden` en push | Intentó pushear a `main` | Verificar que Claude Code use `git push origin HEAD` desde un branch |
| PR no aparece | Branch no fue pusheado | Ir a /branches y crear PR manualmente |
| Cambios no se ven tras merge | GitHub Pages tarda | Esperar 2 min más, hard refresh (Cmd+Shift+R) |
| Branch ya existe | Reutilización de nombre | Agregar sufijo `-2` o timestamp al nombre |
| Imagen no carga tras deploy | Path incorrecto | Verificar mayúsculas/minúsculas exactas del filename |

---

## Cómo incluir esto en un prompt para Claude Code

Al final de cada prompt generado, agregar esta sección:

```markdown
## Git instructions
- Crear branch: `fix/nombre-descriptivo` (nunca main)
- Al terminar: `git add -A && git commit -m "fix(scope): descripción" && git push origin HEAD`
- Reportar el nombre exacto del branch al terminar
```

---

## Nota sobre paths de imágenes en GitHub Pages

GitHub Pages es **case-sensitive**. Si el archivo es `Hero.png`:
- ✅ `../forecast/Hero.png`
- ❌ `../forecast/hero.png`
- ❌ `../forecast/HERO.PNG`

Siempre verificar el nombre exacto del archivo en el repo antes de escribir el path.
