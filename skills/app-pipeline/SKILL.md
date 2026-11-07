---
name: app-pipeline
description: Director de proyecto para crear una app completa a partir de una idea o de una app de referencia (por ejemplo "mi versión definitiva de CapCut"). Encadena en orden fijo y por secciones (Inicio, Fundamentos, Construcción, Publicación) las skills app-workflow, replica-* (recon, entrepreneur, architect, brand, design, build, backend, test, diff, launch, deploy), el sistema de diseño propio del usuario, tdd y revisión de seguridad. Guarda el progreso en PIPELINE.md para retomar en otra sesión y pide el visto bueno del usuario entre fases. Úsalo cuando el usuario diga "crea mi app de principio a fin", "app-pipeline", "sigue el pipeline", "haz una réplica mejorada de X", "retoma el pipeline" o quiera orquestar todo el proceso.
---

# app-pipeline

Orquesta las skills existentes; no las reemplaza. Cada fase carga **solo** la skill que necesita (nunca todas al inicio) y termina con un punto de control. Habla con el usuario en español.

## Reglas fijas

1. **Una fase por vez, con puntos de control.** Nunca encadenes fases por tu cuenta ni trabajes en silencio durante mucho rato: aplica los dos tipos de punto de control de la sección "Puntos de control" (Aclaración y Pausa).
2. **El estado vive en archivos, no en la conversación.** Al empezar: busca `PIPELINE.md` en la carpeta del proyecto. Si existe, retoma en la primera fase no marcada como hecha. Si no, crea el proyecto con la plantilla `PIPELINE.template.md` de esta carpeta.
3. **Pregunta solo cuando la respuesta cambie lo que harás.** Lo trivial se decide con un valor razonable y se anota en `PIPELINE.md` (sección Decisiones); lo importante va a un punto de control de Aclaración.
4. **Sala limpia.** Se reconstruye funcionalidad y patrones de uso, nunca código, recursos, textos, plantillas, efectos, música, fuentes ni logotipos ajenos. Los recursos incluidos en la app (efectos, plantillas, música, fuentes, iconos) salen de fuentes con licencia permisiva o propias; cada uno se anota en `LICENCIAS.md`.
5. **Puerta de licencias antes de escribir código** (Fase 1b). No es consejo legal: señala el riesgo y recomienda revisión de un abogado antes de vender.
6. **Idioma y modos desde el día 1**, no como retoque final: textos en archivos de recursos desde la primera pantalla, y modo Simple / Avanzado como requisito de la especificación y de la arquitectura.
7. Haz la mayor parte en línea. Si el usuario ya tiene subagentes configurados para barridos amplios o para decisiones difíciles de arquitectura o seguridad, úsalos solo para eso; si no los tiene, hazlo todo tú.
8. **Skills que faltan.** Si una skill de apoyo (`replica-*`, `tdd`, etc.) no está instalada, dilo al empezar esa fase y haz el trabajo a mano siguiendo la descripción de la fase, sin inventar que la ejecutaste.
9. No hacer commit, push, publicar ni comprar dominios sin que el usuario lo pida. No se escribe atribución a Claude en commits ni PRs.

## Las fases

### SECCIÓN A · INICIO

**Fase 1 · Brief** (en línea). Una ronda de preguntas, como máximo, sobre: app o apps de referencia, plataforma (escritorio, Android, web), qué significa "mejor" (3 a 5 mejoras), modos Simple/Avanzado, idiomas (por defecto inglés y español), cómo se distribuirá (gratis con código abierto, gratis con código privado, de pago) y qué recursos de marca ya existen. Escribe todo en `PIPELINE.md`.

**Fase 1b · Puerta de licencias y panorama abierto** (en línea; busca en la web y anota la fecha de cada dato). Para cada proyecto de código abierto que pueda aportar (en la categoría video: Kdenlive, Shotcut, OpenShot, Olive, Flowblade, Blender VSE, OpenCut, y las bases: FFmpeg, MLT, GStreamer/GES, Remotion, ffmpeg.wasm, MoviePy) apunta licencia real **verificada**, qué aporta y cómo se usaría:

- **Código copyleft (GPL/AGPL)**: usarlo o enlazarlo obliga a publicar la app con esa licencia. Compatible solo si la app será de código abierto.
- **Permisivo o LGPL (MIT, Apache, BSD, LGPL con enlace dinámico)**: apto para app cerrada.
- **Solo referencia**: se estudia cómo funciona y se reescribe desde cero.
- **Programa externo**: se ejecuta como proceso separado (ffmpeg), con sus propias condiciones.

Resultado: una tabla en `LICENCIAS.md` y una decisión explícita: *¿base fork, bibliotecas permisivas, o sala limpia?* Si la distribución prevista choca con una licencia, avísalo aquí, antes de construir. Si hay ambigüedad, pídelo al usuario como Control 1 y recomienda revisión legal.

**Fase 2 · Reconocimiento.** Carga `replica-recon` por cada app de referencia (también las de código abierto y las de otros editores: solo funciones y flujos). La matriz `replica/features.csv` lleva una columna por fuente, para ver qué tiene cada una.

**Fase 3 · Mejoras.** Carga `replica-entrepreneur` (primera pasada) para saber qué critican los usuarios y qué falta. Si la skill exige el diff, haz una pasada ligera ahora y la completa después en la Fase 10. Resultado: `replica/fixes.md` (la lista de mejoras y el ángulo diferencial).

**Fase 4 · Especificación.** Sigue las fases 1 a 3 de `app-workflow`: `docs/SPEC.md` (con modo Simple/Avanzado, idiomas y las mejoras de la Fase 3), `GLOSSARY.md` y ADRs de las decisiones difíciles. Recorta a un MVP real y deja el resto en "Después". Punto de control: el usuario aprueba el alcance.

### SECCIÓN B · FUNDAMENTOS

**Fase 5 · Arquitectura.** Carga `replica-architect` pero **sobrescribe su pila por defecto** si no es una web SaaS: la pila Next.js/Supabase/Stripe no sirve para un editor de video de escritorio. Pregunta o decide la plataforma y la pila a partir de las decisiones de la Fase 1b. Añade `codebase-design` de `app-workflow` para pocos módulos profundos y los puntos de prueba.

**Fase 6 · Marca.** Carga `replica-brand` ahora, antes del diseño (así el diseño ya usa tu identidad): nombre con las comprobaciones de marca y dominio, paleta con contraste, guía de voz, **brief del logo**. Para producir el logo y el icono de la app, usa `canvas-design` (PNG o PDF) o SVG a mano; no copies iconos de nadie. Deja el barrido `sweep.py` para la Fase 12.

**Fase 7 · Diseño.** Lee primero el sistema de diseño propio del usuario: la ruta indicada en el Brief (campo "Ruta de tu sistema de diseño") y úsalo tal cual como base. Si no hay ninguno, `frontend-design` define uno nuevo. Carga `replica-design` solo para sacar los **patrones de uso** de las referencias (disposición de la línea de tiempo, paneles, atajos), nunca su aspecto, y `frontend-design` para elegir un elemento firma propio. Salida: `replica/design/tokens.json` con tus valores y `components.md`.

### SECCIÓN C · CONSTRUCCIÓN

**Fase 8 · Construir.** `replica-build` pantalla por pantalla, con la rebanada vertical primero (para un editor: importar, cortar, previsualizar, exportar). Dentro de cada historia usa `tdd` (prueba roja, código mínimo, verde). Si hay cuentas, nube o pagos, añade `replica-backend`; si todo es local, sáltalo y anótalo. Textos siempre en recursos, y cada función marca su modo (Simple o Avanzado).

**Fase 9 · Pruebas.** `replica-test` para flujos y casos límite. Para apps de Android o Windows, `tiny-monkey` busca fallos de estabilidad. Corrige antes de seguir.

**Fase 10 · Paridad y mejoras.** `replica-diff` mide cuánto cubre la app frente a la matriz de funciones. Repite `replica-entrepreneur` si faltaba su segunda pasada. Si la paridad de las funciones obligatorias es menor que el objetivo del usuario, **vuelve a la Fase 8** con la lista de lo que falta. Es un bucle: se repite hasta cumplir el objetivo.

### SECCIÓN D · PUBLICACIÓN

**Fase 11 · Revisión final.** Fase 6 de `app-workflow` (estándares, fidelidad a la spec, lista de lanzamiento) y `security-review-deep`. Si algo es delicado, plantéaselo al usuario (Control 1) antes de seguir.

**Fase 12 · Barrido de marca y licencias.** Ejecuta `replica-brand`/`sweep.py` buscando restos de las apps de referencia. Revisa `LICENCIAS.md`: ¿todo lo incluido tiene su licencia y su aviso? Genera el archivo de avisos de terceros.

**Fase 13 · Lanzamiento.** `replica-launch`: página de aterrizaje, precios, ficha de las tiendas, plan. Sin testimonios ni cifras inventadas.

**Fase 14 · Despliegue.** `replica-deploy` solo con el visto bueno explícito del usuario para publicar. Respeta la forma de distribución elegida en el Brief (por ejemplo, lanzamientos públicos con código privado).

## Puntos de control

Hay dos tipos. Los dos se registran en `PIPELINE.md` (sección Registro de controles) con fecha y respuesta del usuario.

### Control 1 · Aclaración (preguntas)

Sirve para decidir juntos cuando aparece algo que cambia el rumbo. Se activa **en cualquier momento de una fase**, no solo al final, cuando ocurra alguno de estos casos:

- Un **hallazgo** que contradice o amplía lo previsto (una función que nadie cubre, una licencia que choca con la distribución, una función que los usuarios odian).
- Un **problema** que no puedes resolver solo (una prueba que falla por una decisión de diseño, una dependencia que no encaja, un coste o plazo inesperado).
- Una **ambigüedad** donde dos caminos razonables dan resultados distintos.
- Un **riesgo** (legal, de seguridad, de datos del usuario).

Cómo hacerlo:
1. Para el trabajo en ese punto y agrupa las dudas pendientes (hasta 4) en **una sola tanda** con `AskUserQuestion`: cada pregunta cita el hallazgo o problema concreto, da 2 a 4 opciones con su consecuencia y marca tu recomendación primero.
2. No preguntes lo trivial ni lo que ya decidió el usuario. Si dudas entre preguntar o decidir, mira si el otro camino es caro de deshacer: si lo es, pregunta.
3. Anota cada respuesta en Decisiones, y si cambia fases anteriores márcalas "a revisar".

### Control 2 · Pausa (continuar, guardar o parar)

Sirve para que el usuario decida si sigue gastando sesión. Se activa:

- **Al terminar cada fase** (siempre).
- **Dentro de las fases largas** (8, 9 y 10): cada ~3 pantallas o historias, o cada hito (rebanada vertical lista, suite de pruebas en verde). Antes de la pausa, **enseña algo para revisar**: captura de pantalla, el resumen de las pruebas, la lista de funciones hechas. Una pausa sin nada que mirar no cuenta.
- Cuando la conversación se haga muy larga, o antes de un paso caro (muchas búsquedas, muchas pruebas en el teléfono).

Formato, con `AskUserQuestion` y estas opciones:
1. **Continuar** con la siguiente fase o bloque. Di cuál es y cuánto supone (poco, medio o mucho trabajo).
2. **Pausar y guardar:** actualiza `PIPELINE.md` (casillas, "Siguiente paso", y los **Pendientes**: lo que quedó a medias, las dudas abiertas, lo que hay que probar) y termina con una línea para retomar: "Para seguir, abre una sesión nueva y di `/app-pipeline`".
3. **Cambiar algo** antes de seguir (el usuario describe el cambio; se anota como decisión).

Si el usuario no contesta o dice "para", haz la opción 2 sin preguntar más. No continúes en silencio.

## Revisión de huecos (al final de cada fase, antes del Control 2)

Cada fase termina buscando lo que falta, no solo con lo hecho. Recorre la lista de la fase, responde cada pregunta con "cubierto" (y dónde), "hueco" o "no aplica" (y por qué), y anota los huecos en `PIPELINE.md` (sección Huecos) con estado abierto o cerrado. Un hueco que bloquea la fase siguiente, o que es caro de arreglar más tarde, pasa al **Control 1** como pregunta; el resto se apunta en Pendientes. Adapta las preguntas a la app (los ejemplos son de un editor de video).

| Fase | Preguntas para encontrar huecos |
| --- | --- |
| 1 Brief | ¿Quién es el usuario concreto? ¿Qué NO hará la app? ¿Plataformas mínimas? ¿Cómo se mide el éxito (paridad, usuarios)? ¿Tiempo y presupuesto reales? ¿Quién la mantendrá? |
| 1b Licencias | ¿Cada dependencia tiene licencia verificada y con fecha? ¿Son compatibles entre sí? ¿Y los códecs y sus patentes (H.264, H.265, AAC)? ¿Las licencias de fuentes, efectos y música? ¿Las condiciones de las tiendas? |
| 2 Recon | ¿Cubrí funciones ocultas o de pago? ¿Formatos de importación y exportación? ¿Atajos, accesibilidad y rendimiento? ¿Estados vacíos y de error? ¿Escritorio frente a móvil? ¿Qué hace cada fuente que las otras no? |
| 3 Mejoras | ¿Cada mejora tiene evidencia con enlace? ¿Hay quejas frecuentes sin cubrir? ¿Se puede medir si se logró? ¿Cabe en el MVP? |
| 4 Especificación | ¿Cada función de la matriz tiene historia o está en "Después"? ¿Criterios de aceptación? ¿Modo Simple y Avanzado definido por función? ¿Guardado, recuperación tras un fallo y versión del archivo de proyecto? ¿Límites de rendimiento (archivos grandes)? ¿Textos listos para traducir? |
| 5 Arquitectura | ¿Cada historia tiene un módulo? ¿Hilos, memoria y rendimiento? ¿Formato de proyecto versionado con migraciones? ¿Autoguardado? ¿Plan de extensiones o efectos? ¿Empaquetado, instalador y actualizaciones? ¿Dónde están los puntos de prueba? |
| 6 Marca | ¿Las comprobaciones se ejecutaron (con fecha) o están "por hacer"? ¿El nombre funciona en los idiomas objetivo? ¿El logo sirve en tamaño de icono y en claro y oscuro? ¿La voz está en ambos idiomas? |
| 7 Diseño | ¿Todos los estados de cada componente? ¿Contraste verificado? ¿Se diseñó el modo Simple y el Avanzado? ¿Atajos de teclado y accesibilidad? ¿Textos largos (el español y el alemán ocupan más)? ¿Hay elemento firma? |
| 8 Construir | ¿Cada pantalla tiene vacío, carga y error? ¿Todos los textos están en recursos? ¿Qué pasa con archivos corruptos o no compatibles? ¿Está actualizada la matriz de funciones? ¿Hay prueba por historia? |
| 9 Pruebas | ¿Casos límite (archivo enorme, formato raro, sin espacio, cancelar a medias)? ¿Probado en ambos idiomas y modos? ¿En dispositivos reales? ¿Rendimiento medido? |
| 10 Paridad | ¿Falta alguna función obligatoria? ¿Hay algo "parcial" sin nota? ¿Hay regresiones frente a la última medición? |
| 11 Revisión final | ¿Existe trazabilidad matriz → historia → prueba → código? ¿Secretos, permisos y privacidad? ¿Qué datos envía la app y están declarados? |
| 12 Barrido | ¿Restos de marcas ajenas? ¿Avisos de terceros completos? ¿Licencias de todos los recursos? ¿Capturas de referencia fuera del paquete? |
| 13 Lanzamiento | ¿Todas las afirmaciones se pueden demostrar? ¿Precios coherentes? ¿Privacidad y términos? ¿Canal de soporte y comentarios? ¿Ficha en ambos idiomas? |
| 14 Despliegue | ¿Copia de seguridad y plan para volver atrás? ¿Actualizaciones y monitorización? ¿Quién responde los reportes? |

**Revisión global (en la Fase 11):** reúne los huecos abiertos de todas las fases, ciérralos o decide con el usuario cuáles se aceptan, y deja el resultado en `PIPELINE.md`.

## Al terminar cada fase

Actualiza la casilla en `PIPELINE.md`, la fecha, los archivos generados y las decisiones nuevas. Haz la **Revisión de huecos** de la fase. Escribe un resumen de 5 líneas (qué se hizo, qué quedó fuera, qué huecos y hallazgos hubo). Si quedaron huecos o dudas que bloquean, lanza el Control 1; después, el Control 2.

## Al retomar en otra sesión

Lee `PIPELINE.md` y `LICENCIAS.md`, di en una línea dónde se quedó el proyecto, enseña los **Pendientes** guardados (dudas abiertas, cosas por probar) y lanza primero el Control 1 con las dudas abiertas, si las hay. Luego propón la fase siguiente con el Control 2. No releas todo el proyecto.

## Cuando el usuario cambia de idea

Si cambia la app de referencia o la distribución, anótalo como decisión nueva, marca como "a revisar" las fases afectadas (casi siempre 1b, 3, 4 y 5) y vuelve a la primera. No rehagas lo que no cambia.
