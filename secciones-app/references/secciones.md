# Biblioteca de secciones

Doce bloques listos para entregar. Cada uno con **cuándo aplica**, el **texto** y **de dónde salió**.

---

## 1 · Acceso y usuarios

**Cuándo:** hay algo que no debe ver cualquiera.

```
Agrega acceso con usuario y contraseña.

- Registro con correo y contraseña, y recuperación por correo.
- Nadie ve ninguna pantalla sin haber iniciado sesión, salvo la de ingreso.
- La sesión se mantiene si cierro el navegador y vuelvo.
- El registro NO es abierto: las altas las hago yo. Un correo desconocido
  no puede crearse una cuenta solo.

No inventes usuarios de prueba con datos reales. Si necesitas uno, usa un
correo de ejemplo y dime cuál creaste.
```

**De dónde salió:** el registro abierto es el comportamiento por defecto. Si no se dice lo
contrario, cualquiera que encuentre la dirección se crea una cuenta — y en una herramienta con el
nombre de una empresa encima, eso no puede pasar.

---

## 2 · Roles y permisos

**Cuándo:** hay más de un tipo de usuario.

```
La aplicación tiene estos roles, y cada usuario tiene uno solo:

- <ROL BÁSICO>: <qué puede hacer, y qué NO puede ver de los demás>
- <ROL INTERMEDIO>: todo lo anterior, y además <...>
- ADMINISTRADOR: todo lo anterior, y además da de alta usuarios y asigna roles.

Guarda el rol en la base de datos, no en el navegador.

Que la pantalla muestre solo lo que ese rol puede hacer: si alguien no puede
dar de alta usuarios, no debe ver el botón.
```

**De dónde salió:** *"guarda el rol en la base de datos, no en el navegador"* es la línea que
importa. Un rol guardado en el navegador se edita desde las herramientas de desarrollo y el usuario
entra como administrador.

Esconder el botón es **comodidad, no seguridad**: evita confusión, pero lo que impide la acción es
la sección 4. Decirlo así evita la falsa sensación de que con ocultar alcanza.

---

## 3 · Aislamiento por organización

**Cuándo:** la usan dos o más empresas — o podría pasar más adelante. **Preparado desde el
principio no cuesta; agregado después obliga a rehacer el modelo de datos.**

```
La aplicación tiene que servir a varias organizaciones a la vez, cada una con
sus usuarios y sus datos.

- Cada usuario pertenece a una organización.
- Un usuario ve únicamente los datos de la suya. Nunca los de otra, ni siquiera
  el administrador de una organización puede ver los de otra.
- Yo, como dueño de la plataforma, sí puedo verlas todas.

Prepara esto desde ahora aunque hoy haya una sola organización.

Explícame en palabras simples cómo lo vas a separar, antes de escribir el código.
```

**De dónde salió:** la última línea es la que más rinde. Si te explica el criterio y te cierra,
sigues; si no te cierra, lo corriges cuando corregir es una frase.

---

## 4 · Seguridad de los datos ⭐

**Cuándo:** siempre que haya base de datos.

**Primero el porqué, que sin él el bloque no se aplica:** una aplicación web se conecta a la base
con una clave que **viaja en el navegador y es pública**. Cualquiera que abra la aplicación puede
verla. Esa clave no da permisos por sí sola — **los permisos los pone la base de datos**. Si no
están escritos, la tabla queda abierta a quien tenga la clave.

**Hay dos caminos. Recomienda el segundo salvo que haya una razón concreta:**

| Camino | Implica |
|---|---|
| Con políticas por fila | Cada tabla lleva su regla. La app consulta desde el navegador. Más flexible, **y más fácil olvidar una** |
| **Sin políticas, todo por el servidor** | Se activa la seguridad en todas las tablas y **no se escribe ninguna política**: solo entra el servidor de la propia app. **Una clave filtrada no sirve de nada** |

```
Activa la seguridad por fila (Row Level Security) en TODAS las tablas, sin
definir políticas de acceso.

Así solo puede entrar el servidor de la propia aplicación, y una clave pública
filtrada no sirve para nada.

Toda consulta a la base pasa por el servidor, nunca directo desde el navegador.
Ahí se comprueba quién es el usuario, de qué organización es y qué rol tiene.

Recuérdame esta regla cada vez que agreguemos una tabla nueva, porque es en las
tablas nuevas donde esto se olvida.
```

**De dónde salió:** una auditoría sobre una plataforma en producción encontró **46 tablas sin reglas
y 5 con reglas que dejaban pasar a todos**. Se habían acumulado de a una, cada vez que se agregaba
una tabla sin pensar en esto. **Ninguna daba error** — funcionaban perfecto, y ese es el problema.

El segundo camino elimina la clase entera de error: si no hay políticas que escribir, no hay
políticas que olvidar.

**Tres cosas más que la misma auditoría dejó, y que casi nadie revisa:**

- **Proteger las tablas no alcanza.** Una *función* de base de datos puede quedar ejecutable desde
  afuera por quien tenga la clave pública. Hay que revocarle el permiso público.
- **Una política que deja pasar a todos parece protección y no lo es.** En un listado se ve como
  "tiene política" y está tan abierta como si no tuviera.
- **Las vistas creadas con permisos del autor** corren con los privilegios de quien las creó, no de
  quien consulta — y saltean el control sin avisar.

---

## 5 · Credenciales ⭐

**Cuándo:** siempre.

```
Todas las claves y credenciales van en el archivo .env, declarado como ignorado,
que nunca sube al repositorio.

Nunca las escribas dentro de un archivo del proyecto ni me las muestres completas
en el chat.

Cada servicio lleva su propia credencial, creada para este proyecto. No reutilices
ninguna de otro sistema.
```

**De dónde salió:** tres cosas distintas, todas vividas.

- **Una clave escrita en un archivo del proyecto queda en el historial aunque después la borres.**
  El commit anterior la sigue teniendo.
- **Toda credencial pegada en una conversación hay que darla por comprometida y rotarla.** No
  importa que el chat sea privado: queda escrita. Rotar es un botón; descubrirlo tarde, no.
- **Reutilizar la credencial de otro proyecto ata los dos.** El día que haya que rotarla, se caen
  ambos a la vez.

Y una que aparece sola con el tiempo: **el archivo de entorno apuntando a una base que ya no es la
buena.** Cuando se migra de proyecto, las referencias viejas quedan en `.env` y en el panel de
despliegue, y nadie lo nota hasta que algo escribe donde no debía.

---

## 6 · Auditoría automática ⭐

**Cuándo:** siempre, antes de publicar. **Es la sección que más rinde de todas:** reemplaza
acordarse de las reglas por una herramienta que las verifica.

```
Antes de publicar, corre el advisor de seguridad de la base de datos y muéstrame
el resultado clasificado en tres grupos:

1. Lo que hay que corregir sí o sí.
2. Lo que conviene corregir.
3. Lo que está bien así, con el motivo.

Para cada punto del grupo 1, dime qué implica en concreto: quién podría ver o
hacer qué que no debería.
```

**De dónde salió:** una pasada del advisor sobre una plataforma real largó, de una sola vez,
funciones con ruta de búsqueda modificable, funciones con permisos del autor ejecutables por
usuarios anónimos, políticas siempre verdaderas, buckets públicos que permitían listar su contenido
y protección de contraseñas filtradas desactivada.

**Nada de eso se encuentra a ojo, y todo estaba funcionando bien.** Pedir la clasificación en tres
grupos es lo que evita que el resultado se vuelva una lista intimidante que nadie tría: buena parte
suele ser intencional, y lo que importa es separar eso de lo que no.

---

## 7 · Estados y ciclo de vida

**Cuándo:** algo cambia de estado — pedidos, tickets, reservas, inscripciones.

```
Define los estados por los que pasa <la entidad> y qué los hace cambiar.

Reglas:
- Un estado solo cambia por una acción explícita. Que alguien responda o
  interactúe NO cambia el estado por sí solo.
- Ningún cambio de estado automático que yo no pueda revertir.
- Muéstrame el recorrido completo de estados antes de implementarlo.
```

**De dónde salió:** en una aplicación de soporte, un **"gracias"** del usuario reabría el ticket ya
resuelto y lo devolvía a la bandeja de pendientes. Nadie lo pidió: la regla era *"si el usuario
responde, el ticket está vivo"*, que suena razonable hasta que la mitad de los pendientes son
agradecimientos.

**No es un problema de seguridad ni de base de datos: es de lógica de producto**, y aparece en toda
aplicación con estados. Por eso conviene definir el recorrido antes, en vez de descubrirlo con la
bandeja llena.

---

## 8 · Concurrencia

**Cuándo:** dos personas podrían tomar lo mismo al mismo tiempo — turnos, cupos, inventario.

```
Dos personas no pueden tomar <el mismo turno / el último cupo> a la vez.

No alcanza con revisar disponibilidad antes de guardar: entre esa revisión y el
guardado pueden pasar milisegundos y entrar los dos.

Quiero la restricción en la base de datos, para que la segunda operación falle
sí o sí, y que la aplicación muestre un mensaje claro cuando eso pase.
```

**De dónde salió:** la comprobación previa es la solución intuitiva y la equivocada. La única
defensa real es que **la base de datos rechace la segunda**, porque es el único punto donde las dos
operaciones se ordenan de verdad.

---

## 9 · Tareas programadas

**Cuándo:** hay recordatorios, resúmenes o algo que corre solo.

```
Necesito que <la tarea> corra cada <frecuencia>.

Antes de elegir dónde: dime qué frecuencia mínima permite cada opción en su plan
gratuito, porque eso puede decidir la herramienta.

Y que la tarea no repita el envío: si corre dos veces, o falla y reintenta, cada
destinatario recibe una sola vez.
```

**De dónde salió:** dos trampas.

- **La frecuencia del plan gratuito decide la herramienta.** Un servicio de despliegue muy usado
  solo permite frecuencia diaria en su plan gratis; si los recordatorios tienen que salir cada
  cinco o quince minutos, ese servicio queda descartado por eso, no por precio. Conviene preguntarlo
  **antes** de construir.
- **La repetición del envío.** Si dos ejecuciones se superponen o una falla y reintenta, el mismo
  destinatario recibe dos veces. Se resuelve marcando cada envío antes de mandarlo, no después.

---

## 10 · Envío de correo

**Cuándo:** la aplicación manda mails.

```
Los correos salen desde un dominio propio verificado, no desde una dirección
genérica.

Antes de enviar a alguien real, muéstrame el correo completo como lo va a
recibir, y espera mi visto bueno.

Que quede registro de qué se envió, a quién y cuándo — y que un mismo aviso no
salga dos veces a la misma persona.
```

**De dónde salió:** sin dominio verificado, buena parte termina en correo no deseado. Y el registro
de envíos es lo que permite responder *"¿le llegó?"* con un dato en vez de una suposición.

La línea del visto bueno previo evita el error caro: **un correo enviado no se puede des-enviar**.

---

## 11 · Verificación final ⭐

**Cuándo:** siempre, antes de darla por terminada. **Que funcione no prueba que sea segura: hay que
probar que lo que no debería poder hacerse, no se pueda.**

```
Antes de darlo por terminado, comprueba que los límites funcionan. Verifica cada
una y dime el resultado:

1. Un usuario intentando ver los datos de otro usuario.
2. Un usuario intentando entrar a una pantalla de administración escribiendo la
   dirección directamente en el navegador.
3. Un usuario de otra organización intentando ver datos ajenos.
4. Alguien sin haber iniciado sesión, pidiendo datos directamente a la base con
   la clave pública de la aplicación.

Las cuatro tienen que fallar. Si alguna funciona, dime cuál y arreglémoslo antes
de seguir.
```

**De dónde salió:** la 2 y la 4 son las que encuentran errores reales. **La 2 saltea el menú** y va
directo a la dirección; **la 4 saltea la aplicación entera** y le habla a la base de datos. Una
aplicación que aprueba esas dos tiene la seguridad donde corresponde: en los datos, no en la
pantalla.

---

## 12 · Memoria del proyecto ⭐

**Cuándo:** al cerrar, siempre.

```
Escribe las reglas de este proyecto en el archivo CLAUDE.md: los roles y qué
puede hacer cada uno, la separación por organización, que toda tabla lleva
seguridad activada, y que las claves van en el .env.

Agrega también los errores que encontramos y cómo los resolvimos, para no
repetirlos.

Quiero que respetes estas reglas en todo lo que construyamos de acá en adelante,
sin que tenga que repetírtelas.
```

**De dónde salió:** lo que queda escrito ahí se respeta en cada sesión nueva; **lo que solo se dijo
en un chat se pierde**. El segundo párrafo es el que convierte el archivo en memoria: no solo las
reglas, también lo que ya se rompió una vez.
