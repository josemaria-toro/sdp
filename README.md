# Software Development Platform (sdp)

Vamos a comenzar un nuevo proyecto que se llama Software Development Platform.
Es una plataforma agentica para el desarrollo de aplicaciones.

## Guías

Vamos a definir una serie de guías que nos ayudarán en el proceso de construcción de este producto.
Para almacenar las guías, se ha de crear la siguiente estructura de carpetas y ficheros:
Las instrucciones para Claude están en [CLAUDE.md](CLAUDE.md).

- harness
  - development: guías de desarrollo.
    - [git.md](harness/development/git.md): guía con la estrategia de ramas.
    - [python.md](harness/development/python.md): guía para el desarrollo de aplicaciones basadas en python.
    - [react.md](harness/development/react.md): guía para el desarrollo de aplicaciones basadas en react.
  - documentation: guías para la generación de documentación.
    - [code.md](harness/documentation/code.md): guía para la documentación del código fuente de las aplicaciones.
    - [projects.md](harness/documentation/projects.md): guía para la documentación de proyectos.
  - security: guías de seguridad.
    - [python.md](harness/security/python.md): guía de seguridad para aplicaciones basadas en python.
    - [react.md](harness/security/react.md): guía de seguridad para aplicaciones basadas en react.
  - technologies: guías de tecnologías admitidas.
    - [python.md](harness/technologies/python.md): guía que incluye las tecnologías admitidas en aplicaciones basadas en python.
    - [react.md](harness/technologies/react.md): guía que incluye las tecnologías admitidas en aplicaciones basadas en react.
  - guardrails: definición de guardarrailes.
    - [python.md](harness/guardrails/python.md): guía que incluye los guardarrailes definidos para aplicaciones basadas en python.
    - [react.md](harness/guardrails/react.md): guía que incluye los guardarrailes definidos para aplicaciones basadas en react.
  - licenses: licencias de software.
    - [python.md](harness/licenses/python.md): licencia para aplicaciones basadas en python.
    - [react.md](harness/licenses/react.md): licencia para aplicaciones basadas en react.
  - architecture: guías de arquitectura.
    - [python.md](harness/architecture/python.md): guía de arquitectura para proyectos de backend.
    - [react.md](harness/architecture/react.md): guía de arquitectura para proyectos de frontend.

## Ramas

Las ramas siguen la estrategia definida en [git.md](harness/development/git.md):

- `main`: código en producción. Recibe merges desde `pre-release` y desde `hotfix/*`.
- `pre-release`: código preparado para publicar. Recibe merges desde `develop`.
- `develop`: integración del trabajo en curso. Es la rama de la que salen las ramas de trabajo.
- Ramas de trabajo (`feature/`, `fix/`, `docs/`, `refactor/`, `chore/`): salen de `develop`.
- Ramas de `hotfix/*`: salen de `main` y se propagan a `pre-release` y `develop`.

Flujo de promoción: `feature/fix/docs/refactor/chore → develop → pre-release → main`.

Las ramas permanentes están protegidas: los cambios entran mediante PR con una aprobación, no se permite force push ni borrado, y el merge se hace solo por squash.
