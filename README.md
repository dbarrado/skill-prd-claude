# prd-claude

Skill para **Claude Code** que te acompaña a escribir el **PRD** de una aplicación —el documento
que dice qué tiene que hacer el producto— antes de escribir una sola línea de código.

No hay que ser programador para usarla. La skill pregunta, sección por sección, y al final entrega
un documento completo listo para pegar en Claude Code y empezar a construir.

## Por qué un PRD

Corregir el PRD cuesta un mensaje. Corregir la aplicación ya construida cuesta muchísimo más.

La sección que más rinde es la que más cuesta escribir: **qué NO va a hacer** la primera versión.
Decir que sí a todo garantiza no terminar nada.

## Qué hace

Recorre cinco fases, sin saltear ninguna:

1. **Recopilación** — sección por sección, una a la vez, sin asumir nada que no hayas dicho
2. **Validación** — repasa lo reunido antes de generar
3. **Idioma** — pregunta si el PRD sale en español o en inglés
4. **Generación** — el documento completo, con estructura fija
5. **Cierre** — qué hacer con el PRD ya escrito

Stack por defecto: **Next.js + Vercel + Supabase**. Si el proyecto pide otra cosa, lo recomienda
con su justificación.

## Instalación

Pídeselo a Claude Code con tus palabras:

```
Instala la skill que está en https://github.com/dbarrado/skill-prd-claude
```

O a mano: copia `SKILL.md` a una carpeta `prd-claude/` dentro de tus skills.

- **Global** (para todos tus proyectos): `~/.claude/skills/prd-claude/SKILL.md`
- **Solo un proyecto**: `<tu-proyecto>/.claude/skills/prd-claude/SKILL.md`

## Cómo se usa

Se activa sola cuando le dices que quieres construir algo:

```
Quiero hacer una aplicación para <lo que necesites>. Ayúdame a escribir el PRD.
```

Conviene combinarla con el **modo plan** (`/plan`): así Claude lee y propone, pero no toca ningún
archivo hasta que le des el visto bueno.

## Licencia

MIT. Úsala, modifícala y compártela.
