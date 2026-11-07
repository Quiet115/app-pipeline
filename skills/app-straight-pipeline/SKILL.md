---
name: app-straight-pipeline
description: Versión "derecha" de app-pipeline. Construye una app completa de principio a fin sin hacer preguntas por el camino, decidiendo con su propio criterio a partir de lo que el usuario dijo o insinuó (por ejemplo "una copia para mí solo" o "que cobre mensualmente"), y al final entrega el producto hecho con un resumen (qué incluye, qué no se pudo hacer, qué supuestos tomó) y una sola ronda de preguntas para corregir; con las respuestas decide qué corregir y continúa. Úsalo cuando el usuario diga "/app-straight-pipeline", "hazlo todo derecho", "hazlo de una vez sin preguntar", "construye la app completa y pregunta al final".
---

# app-straight-pipeline

Mismo recorrido que `app-pipeline`, pero **sin detenerse a preguntar**. Las dudas no frenan el trabajo: se resuelven con criterio propio, se anotan, y se preguntan todas juntas **al final**, cuando ya hay un producto que enseñar y las preguntas sirven para corregir. Habla con el usuario en español.

## Reglas fijas

1. **No preguntes durante la construcción.** Ni al empezar, ni entre fases. Si algo falta, decide con la regla de inferencia y sigue. Las únicas paradas permitidas son las de la regla 4.
2. **Todo supuesto queda escrito.** Cada decisión que tomaste por tu cuenta va a `PIPELINE.md` (sección Inferencias) con: la señal que la motivó, la decisión, la confianza (alta/media/baja) y lo que costaría cambiarla.
3. **El estado vive en archivos.** Al empezar busca `PIPELINE.md` en la carpeta del proyecto: si existe, retoma en la primera fase sin terminar. Si no, crea una carpeta con el nombre provisional de la app dentro de la carpeta de trabajo actual y copia ahí `STRAIGHT-PIPELINE.template.md`. Actualiza el archivo al cerrar cada fase, para sobrevivir a una conversación larga o cortada.
4. **Lo que NO se hace sin permiso, aunque sea modo directo.** Prepáralo listo para ejecutar, pero no lo ejecutes, y anótalo en el resumen como "pendiente de tu OK": publicar o desplegar, hacer push o commit, crear repos, comprar dominios, cobros o pagos reales, crear cuentas, introducir credenciales o claves, enviar mensajes en nombre del usuario, y obtener datos de una forma que las condiciones de un sitio prohíban. El modo directo elimina las preguntas de producto, no las de seguridad.
5. **Sala limpia** (las reglas de `replica-recon`/`replica-build` no se negocian): se reconstruyen funciones y flujos, nunca el código, los recursos, textos, plantillas, efectos, música, fuentes ni logos de otros. Esto no depende de lo que el usuario diga, ni siquiera de "es para mí solo".
6. **Lo que no se pueda hacer, se salta y se cuenta.** Si algo exige una cuenta de pago, un dispositivo que no tienes, una credencial o una prueba que no puedes ejecutar, déjalo como "no se pudo hacer", con el motivo y lo que haría falta, y sigue con lo demás. Nunca digas que algo está probado si no lo ejecutaste.
7. **No pongas atribución a Claude** en commits, PRs ni documentos del producto.
8. Haz lo más posible en línea. Usa subagentes solo si el usuario ya los tiene configurados, y solo para barridos amplios o decisiones difíciles de arquitectura o seguridad.
9. Si falta una skill de apoyo (`replica-*`, `tdd`, etc.), dilo en el resumen y haz esa fase a mano siguiendo la descripción; no finjas haberla ejecutado.

## Cómo decidir sin preguntar

**Paso 0 · Lectura de señales** (antes de la Fase 1). Reúne todo lo que el usuario dijo o insinuó, en este mensaje y en el contexto (instrucciones del usuario, memoria, archivos del proyecto), y escríbelo en una tabla: *señal → qué implica → decisión*. Las señales pueden ser explícitas o implícitas:

| Señal | Qué se deduce |
| --- | --- |
| "una copia para mí solo", "uso personal" | El riesgo de licencias y marcas de publicarla baja: no hay ficha de tienda, ni comprobación de marca registrada, ni exigencia de licencia permisiva. Se anota igualmente y se mantiene la sala limpia. |
| "que cobre mensualmente", "de pago", "suscripción" | La estructura incluye planes, derechos por plan (qué desbloquea cada uno), proveedor de cobros detrás de una interfaz y estado de la suscripción, aunque el cobro real quede sin conectar (regla 4). |
| "gratis", "regalarla" | Sin cobros; las licencias de código abierto (también GPL) son aceptables. |
| "código privado", "que no me la clonen" | Evitar dependencias copyleft; licencia propia de uso libre y sin redistribución. |
| "con mi diseño", "mi estilo" | Usar el sistema de diseño propio del usuario si existe; si no, definir uno y registrarlo. |
| "simple y avanzado", "para principiantes y expertos" | Dos modos desde la especificación. |
| "en varios idiomas" | Textos en recursos desde la primera pantalla. |
| Plataforma o herramientas que el usuario ya usa | Seguir su pila y sus convenciones antes que inventar otra. |
| Palabras de ambición ("la definitiva", "con todo") | Alcance amplio, pero el MVP primero y el resto en "Después", con orden. |

**Si no hay señal**, elige lo más conservador y fácil de deshacer: pila aburrida y gestionada, menos dependencias, lo que el usuario ya conoce. Si una decisión es **cara de deshacer** (plataforma, motor de datos, licencia, modelo de negocio) y la confianza es baja, **no preguntes**: construye una capa que haga barato cambiarla y sube esa decisión al primer puesto de las preguntas finales.

## Las fases (las mismas 14 de app-pipeline, sin paradas)

Carga cada skill de apoyo solo en su fase. Al cerrar cada una haz su **revisión de huecos** (la lista de preguntas de abajo, que te respondes tú): cubre el hueco si es barato, y si no, anótalo en Preguntas finales.

| # | Fase | Qué haces | Huecos que buscas |
| --- | --- | --- | --- |
| 1 | Brief | Completa el brief con las señales; fija el alcance del MVP y las mejoras | ¿Usuario concreto? ¿Qué NO hará? ¿Plataformas? ¿Cómo se mide la paridad? |
| 1b | Licencias | Tabla `LICENCIAS.md` con licencias verificadas con fecha (búsqueda web) y la decisión (base, bibliotecas, sala limpia o solo referencia) según las señales | ¿Licencias de dependencias, códecs y patentes, fuentes, efectos y música compatibles? |
| 2 | Reconocimiento | `replica-recon` por cada referencia; matriz `replica/features.csv` con una columna por fuente | ¿Funciones ocultas? ¿Formatos? ¿Estados de error? |
| 3 | Mejoras | `replica-entrepreneur`: qué critican los usuarios; `replica/fixes.md` | ¿Evidencia enlazada? ¿Cabe en el MVP? |
| 4 | Especificación | Fases 1 a 3 de `app-workflow`: `docs/SPEC.md`, `GLOSSARY.md`, ADRs | ¿Cada función con historia o en "Después"? ¿Modos e idiomas por función? |
| 5 | Arquitectura | `replica-architect` con la pila elegida según la plataforma (no el valor por defecto web si no encaja) y `codebase-design` | ¿Un módulo por historia? ¿Formato de datos versionado? ¿Autoguardado? ¿Planes de suscripción previstos si hay señal? |
| 6 | Marca | `replica-brand`: nombre, paleta, voz, logo hecho con `canvas-design` o SVG propio. Las comprobaciones que exigen registros externos se marcan "por verificar" con la fecha | ¿Nombre válido en los idiomas? ¿Logo en tamaño de icono? |
| 7 | Diseño | Sistema de diseño propio del usuario o uno nuevo con `frontend-design`; `replica-design` solo para patrones de uso | ¿Todos los estados? ¿Contraste? ¿Ambos modos? |
| 8 | Construir | `replica-build` con rebanada vertical primero y `tdd` en cada historia; `replica-backend` solo si hay cuentas, nube o cobros | ¿Estados vacío, carga y error? ¿Textos en recursos? ¿Archivos corruptos? |
| 9 | Pruebas | `replica-test` y, si es app de Android o Windows, `tiny-monkey`. Corrige lo que falle | ¿Casos límite? ¿Ambos idiomas y modos? |
| 10 | Paridad | `replica-diff`; si faltan funciones obligatorias, vuelve a la 8 (máximo 2 vueltas; lo que quede, a "no se pudo hacer") | ¿Algo "parcial" sin nota? ¿Regresiones? |
| 11 | Revisión | Fase 6 de `app-workflow` y `security-review-deep`; revisión global de huecos | ¿Trazabilidad función → historia → prueba → código? ¿Datos y permisos? |
| 12 | Barrido | `sweep.py` de `replica-brand`, avisos de terceros, licencias de recursos | ¿Restos de marcas ajenas? |
| 13 | Lanzamiento | `replica-launch`: página, precios, ficha. Sin testimonios ni cifras inventadas. Si la señal es "solo para mí", se omite y se anota | ¿Afirmaciones demostrables? |
| 14 | Despliegue | Deja el paquete y la guía listos (`replica-deploy` solo como preparación). **No publiques** (regla 4) | ¿Copia de seguridad y vuelta atrás? |

## Entrega final

Cuando termines (o no puedas avanzar más), escribe `RESUMEN-FINAL.md` en la carpeta del proyecto y enseña su contenido al usuario. Estructura fija:

1. **Qué se construyó:** una frase y cómo ejecutarlo o probarlo.
2. **Qué incluye:** funciones hechas, por modo (Simple/Avanzado) e idioma.
3. **Qué no se pudo hacer y por qué:** cada ausencia con su motivo y lo que haría falta.
4. **Qué se verificó y qué no:** pruebas ejecutadas con su resultado, y lo que quedó sin probar. Sin adornos.
5. **Decisiones por inferencia:** la tabla señal → decisión → confianza.
6. **Licencias:** lo que condiciona la distribución.
7. **Pendiente de tu OK:** lo que preparaste pero no ejecutaste (publicar, desplegar, cobros, dominios).
8. **Preguntas para ti:** ver abajo.

### Las preguntas finales

Ordénalas por **coste de equivocarse**, las caras primero. Cada una dice: qué asumiste, por qué, qué pasa si estaba mal y cuánto cuesta corregirlo. Haz las preguntas con `AskUserQuestion` en tandas de hasta 4, con tu recomendación como primera opción. Solo preguntas que cambien algo del producto; lo que no se puede preguntar (porque es un hecho) se dice en el resumen.

### Ronda de corrección

Con las respuestas, clasifica cada una:
- **Confirma lo asumido:** no se toca nada; se marca como confirmada.
- **Cambio pequeño:** corrígelo en línea.
- **Cambio estructural:** identifica las fases afectadas, rehazlas solo a ellas (siguiendo las mismas reglas, sin volver a preguntar salvo que algo nuevo bloquee), actualiza la documentación y las pruebas.

Después actualiza `RESUMEN-FINAL.md` con una sección **Correcciones aplicadas** y vuelve a preguntar solo lo que haya surgido de nuevo. Repite hasta que no queden dudas o el usuario diga que basta. No hagas commit ni publiques sin que lo pida.

## Al retomar

Si hay `PIPELINE.md` a medias, di en una línea dónde se quedó y continúa sin preguntar. Si ya hay `RESUMEN-FINAL.md` con preguntas sin responder, empieza por la ronda final.
