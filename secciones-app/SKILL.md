---
name: secciones-app
description: >
  Biblioteca de secciones de prompt ya probadas para construir aplicaciones web con Claude Code.
  En vez de escribir desde cero cada vez cómo se pide el login, los roles, el aislamiento entre
  empresas, la seguridad de la base o las tareas programadas, esta skill tiene esos bloques ya
  redactados y sabe cuál corresponde según lo que la aplicación necesite. Usa esta skill cuando
  alguien vaya a construir o esté construyendo una aplicación web —sola o después de escribir un
  PRD—, cuando pida agregarle usuarios, permisos, multiempresa, seguridad, notificaciones, cobros
  o tareas automáticas, o cuando diga "qué le falta a esta app", "agregale login", "que cada
  cliente vea solo lo suyo", "revisa la seguridad", "buenas prácticas para esta app". Cada sección
  sale de una aplicación real en producción, y trae el porqué además del texto.
---

# Secciones de prompt para construir aplicaciones

Esta skill no escribe el producto: **aporta los bloques que casi siempre faltan** y que se descubren
tarde, cuando corregirlos ya cuesta caro.

Cada sección de `references/secciones.md` trae tres cosas:

1. **Cuándo aplica** — para no pegar lo que no corresponde.
2. **El bloque, listo para usar** — se entrega tal cual, sin reescribirlo.
3. **De dónde salió** — el error real que lo originó. **Esto no se omite**: un bloque sin su porqué
   se descarta a la primera discusión.

---

## Cómo se usa

### Paso 1 — Entender qué se está construyendo

**Si te llama `prd-claude`** (su Fase 2b), el contexto ya está relevado: no vuelvas a
preguntar nada. Leé lo que esa skill reunió, aplicá la tabla de decisión del Paso 2 y devolvé las
secciones que correspondan para que se incorporen al PRD.

Si actúas por tu cuenta y hay un PRD escrito, leerlo. Si no, alcanza con saber **quién la usa, si hay más de una
organización involucrada, y si guarda datos de personas**.

**No preguntes de a una.** Haz las preguntas que falten juntas, y solo las que cambien qué secciones
entran.

### Paso 2 — Elegir las secciones

Usa esta tabla. Las marcadas **siempre** entran en toda aplicación que guarde datos, aunque nadie
las pida — son las que nadie reclama hasta que fallan.

| Sección | Entra cuando |
|---|---|
| **Acceso y usuarios** | Hay algo que no debe ver cualquiera |
| **Roles y permisos** | Hay más de un tipo de usuario |
| **Aislamiento por organización** | La van a usar dos o más empresas, o *podría* pasar más adelante |
| **Seguridad de los datos** ⭐ | **Siempre**, si hay base de datos |
| **Credenciales** ⭐ | **Siempre** |
| **Auditoría automática** ⭐ | **Siempre**, antes de publicar |
| **Estados y ciclo de vida** | Algo cambia de estado: pedidos, tickets, reservas, inscripciones |
| **Concurrencia** | Dos personas podrían tomar lo mismo a la vez |
| **Tareas programadas** | Hay recordatorios, resúmenes o algo que corre solo |
| **Envío de correo** | La aplicación manda mails |
| **Verificación final** ⭐ | **Siempre**, antes de darla por terminada |
| **Memoria del proyecto** ⭐ | **Siempre**, al cerrar |

### Paso 3 — Entregarlas

- **En orden**, no todas juntas: el que las va a usar tiene que poder revisar el resultado de una
  antes de pegar la siguiente.
- **Con su porqué.** Sobre todo en seguridad, donde el bloque parece exagerado hasta que se entiende
  qué evita.
- **Adaptadas al caso**: si la aplicación es para una escuela, los roles se llaman alumno e
  instructor, no *user* y *manager*. Cambia los nombres; no cambies la regla.
- **Nunca inventes una sección nueva y la presentes como probada.** Si hace falta algo que no está
  en la biblioteca, se escribe y se dice que es nuevo.

### Paso 4 — Proponer sumar lo aprendido

Cuando en el trabajo aparezca un error que **valga para cualquier otra aplicación**, ofrécelo como
sección nueva. Así la biblioteca crece con lo que se rompió de verdad, que es lo único que la hace
valer.

---

## Reglas

- **El porqué no es opcional.** Sin él, la sección se lee como burocracia y se saltea.
- **No las pegues todas de una.** Doce bloques juntos es ruido; tres bien elegidos se aplican.
- **Las marcadas ⭐ no se negocian.** Si alguien dice que no hace falta seguridad porque "es una
  aplicación chica", esa es exactamente la aplicación donde después aparece el problema.
- **Adapta el idioma a quien lee.** Estas secciones están en español neutro; si el destinatario es
  de un país con voseo o tuteo marcado, ajústalo.

---

## Qué NO hace esta skill

- **No reemplaza el PRD.** El PRD dice qué tiene que hacer el producto; esto, cómo pedir las partes
  que no se ven. Van en ese orden, y `prd-claude` la invoca sola en su Fase 2b.
- **No escribe el código.** Entrega los bloques para que se los pidas a Claude Code.
- **No decide la arquitectura.** Cuando una sección ofrece dos caminos —como la de seguridad de
  datos— explica los dos y recomienda, pero la elección es de quien construye.

Las secciones están en **`references/secciones.md`**.
