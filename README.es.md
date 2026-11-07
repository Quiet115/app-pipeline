# app-pipeline

Una skill de Claude Code que dirige un proyecto de app completo, desde una idea (o una app de referencia) hasta su publicación, en orden fijo y con puntos de control para no gastar una sesión entera sin mirar.

Versión en inglés: [README.md](README.md). La skill está escrita en español.

## Qué hace

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

Copia `skills/app-pipeline/` en `~/.claude/skills/` (todos los proyectos) o en `.claude/skills/` (un proyecto) y abre una sesión nueva. Empieza con `/app-pipeline`.

## Límites

- Es una guía de proceso, no un generador de código: no tiene scripts ni ejecuta nada por sí sola.
- Aún no se ha probado de punta a punta en un proyecto completo.
- La puerta de licencias señala riesgos (código copyleft, códecs, recursos); **no es consejo legal**. Que lo revise un abogado antes de vender nada.
- Reconstruir una app significa reconstruir funciones y flujos. No se copian el código, los recursos, los textos, las plantillas, la música ni los logos de otros productos.

## Licencia

MIT. Ver [LICENSE](LICENSE).
