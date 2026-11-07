# app-pipeline

Dos skills de Claude Code que dirigen un proyecto de app completo, desde una idea (o una app de referencia) hasta su publicación, en orden fijo.

| Skill | Estilo |
|---|---|
| [`app-pipeline`](skills/app-pipeline) | Con puntos de control: se detiene a preguntar cuando un hallazgo cambia el rumbo y hace pausas entre fases para no gastar una sesión entera sin mirar. |
| [`app-straight-pipeline`](skills/app-straight-pipeline) | Derecha: no pregunta por el camino, decide con su propio criterio a partir de lo que dijiste o insinuaste ("una copia solo para mí", "que cobre mensualmente"), entrega un producto hecho con un resumen y hace todas sus preguntas al final para corregirlo. |

Versión en inglés: [README.md](README.md). Las skills están escritas en español.

## app-straight-pipeline en breve

- **Lee primero tus indicios:** las señales explícitas o implícitas se convierten en decisiones, cada una anotada con su confianza y lo que costaría cambiarla.
- **No pregunta por el camino**, pero no se salta la seguridad: publicar, desplegar, hacer push, pagos reales, cuentas, credenciales y mensajes en tu nombre quedan preparados y "pendientes de tu OK".
- **Termina con `RESUMEN-FINAL.md`:** qué se construyó, qué incluye, qué no se pudo hacer y por qué, qué se verificó y qué no, y una ronda de preguntas ordenada por el coste de equivocarse. Tus respuestas deciden qué se corrige y qué fases se rehacen.

## Qué hace app-pipeline

No reemplaza a otras skills: las ejecuta en orden, cargando cada una solo en su fase, y guarda el progreso en un archivo `PIPELINE.md` para que una sesión nueva retome donde se quedó la anterior.

| Sección | Fases |
|---|---|
| A · Inicio | Brief, puerta de licencias y panorama de código abierto, reconocimiento, mejoras, especificación |
| B · Fundamentos | Arquitectura, marca y logo, diseño |
| C · Construcción | Construir, pruebas, paridad (vuelve a construir hasta cumplir tu objetivo) |
| D · Publicación | Revisión final y seguridad, barrido de marca y licencias, lanzamiento, despliegue |

### Dos tipos de punto de control

- **Aclaración:** cuando un hallazgo, un problema, una ambigüedad o un riesgo cambia el rumbo, se detiene, agrupa las dudas (hasta 4) en una sola ronda de preguntas con opciones y recomendación, y registra tus respuestas.
- **Pausa:** al final de cada fase, y cada pocas pantallas dentro de las fases largas, enseña algo para revisar (captura, resumen de pruebas, lista de funciones hechas) y pregunta si continuar, pausar y guardar (con la lista de pendientes) o cambiar algo antes.

### Revisión de huecos

Cada fase termina con una lista de preguntas pensadas para encontrar lo que falta (licencias de códecs, estados vacíos y de error, archivos corruptos, los dos idiomas y los dos modos, etc.). Los huecos se siguen en `PIPELINE.md` hasta cerrarse.

## Requisitos

Orquesta skills que debes tener ya instaladas. **No se incluyen** aquí:

- `app-workflow`, `tdd`, `codebase-design`, `domain-modeling`, `frontend-design`, `security-review-deep`
- El paquete Replica: `replica-recon`, `replica-entrepreneur`, `replica-architect`, `replica-brand`, `replica-design`, `replica-build`, `replica-backend`, `replica-test`, `replica-diff`, `replica-launch`, `replica-deploy` (de sus propios autores, con su propia licencia)

Si falta alguna, la skill lo avisa y hace esa fase a mano.

## Instalación

Copia `skills/app-pipeline/` y/o `skills/app-straight-pipeline/` en `~/.claude/skills/` (todos los proyectos) o en `.claude/skills/` (un proyecto) y abre una sesión nueva. Empieza con `/app-pipeline` o `/app-straight-pipeline`.

## Límites

- Es una guía de proceso, no un generador de código: no tiene scripts ni ejecuta nada por sí sola.
- Aún no se ha probado de punta a punta en un proyecto completo.
- La puerta de licencias señala riesgos (código copyleft, códecs, recursos); **no es consejo legal**. Que lo revise un abogado antes de vender nada.
- Reconstruir una app significa reconstruir funciones y flujos. No se copian el código, los recursos, los textos, las plantillas, la música ni los logos de otros productos.

## Licencia

MIT. Ver [LICENSE](LICENSE).
