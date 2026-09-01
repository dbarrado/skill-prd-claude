# prd-claude + secciones-app

Dos skills para **Claude Code** que trabajan juntas: una te acompaña a escribir el **PRD** de una
aplicación —el documento que dice qué tiene que hacer— y la otra le agrega, sola, **las partes que
casi nadie pide y después salen caras**.

No hay que ser programador para usarlas.

## Cómo funcionan juntas

```
prd-claude
  ├── Fase 1  recopila, sección por sección
  ├── Fase 2  valida contigo lo reunido
  ├── Fase 2b ── llama a secciones-app ──► devuelve los bloques que aplican
  ├── Fase 3  pregunta el idioma
  ├── Fase 4  genera el PRD, ya con esas secciones incorporadas
  └── Fase 5  cierra
```

**El enganche es automático.** No hay que acordarse de invocar la segunda: la primera la llama en su
Fase 2b y suma al PRD lo que corresponda al caso.

Si instalas solo `prd-claude`, sigue funcionando: incluye un mínimo de seguridad y credenciales por
su cuenta, y te avisa que existe la biblioteca completa.

## Qué aporta cada una

### `prd-claude`

Recorre cinco fases sin saltear ninguna, sin asumir nada que no hayas dicho, y entrega un documento
listo para pegar en Claude Code. Stack por defecto: **Next.js + Vercel + Supabase**; si el proyecto
pide otra cosa, lo recomienda con su justificación.

La sección que más rinde es la que más cuesta escribir: **qué NO va a hacer** la primera versión.
Decir que sí a todo garantiza no terminar nada.

### `secciones-app`

Doce bloques ya probados, cada uno con **el error real que lo originó** — porque un bloque sin su
porqué se descarta a la primera discusión:

| | |
|---|---|
| Acceso y usuarios | Roles y permisos |
| Aislamiento por organización | **Seguridad de los datos** ⭐ |
| **Credenciales** ⭐ | **Auditoría automática** ⭐ |
| Estados y ciclo de vida | Concurrencia |
| Tareas programadas | Envío de correo |
| **Verificación final** ⭐ | **Memoria del proyecto** ⭐ |

Las cinco ⭐ entran en toda aplicación que guarde datos, **aunque nadie las pida**: son las que nadie
reclama hasta que fallan.

## Instalación

Pídeselo a Claude Code con tus palabras:

```
Instala las skills que están en https://github.com/dbarrado/skill-prd-claude
```

O a mano, copiando las dos carpetas a tus skills:

- **Global** (para todos tus proyectos): `~/.claude/skills/`
- **Solo un proyecto**: `<tu-proyecto>/.claude/skills/`

## Cómo se usa

Se activa sola cuando dices que quieres construir algo:

```
Quiero hacer una aplicación para <lo que necesites>. Ayúdame a escribir el PRD.
```

Conviene combinarla con el **modo plan** (`/plan`): Claude lee y propone, pero no toca ningún archivo
hasta que le des el visto bueno.

## Licencia

MIT. Úsalas, modifícalas y compártelas.
