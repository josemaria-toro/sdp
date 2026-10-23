# Instrucciones para Claude

Este repositorio es Software Development Platform (sdp), una plataforma agentica para el desarrollo de aplicaciones.

## Guías obligatorias

Antes de escribir, modificar o revisar código, lee las guías correspondientes en `harness/` y aplícalas. Las guías son la fuente de verdad. Si una instrucción de este fichero contradice una guía, avisa al usuario y sigue la guía.

- Código Python: `harness/development/python.md`, `harness/security/python.md`, `harness/technologies/python.md`, `harness/guardrails/python.md`.
- Código React: `harness/development/react.md`, `harness/security/react.md`, `harness/technologies/react.md`, `harness/guardrails/react.md`.
- Documentación de código: `harness/documentation/code.md`.
- Documentación de proyectos: `harness/documentation/projects.md`.
- Ramas, commits y PR: `harness/development/git.md`.
- Licencias: `harness/licenses/`.

Si una tecnología no aparece en la guía de tecnologías admitidas, no la uses. Pregunta al usuario antes de incorporarla.

## Reglas clave

- **Guardarraíles**: cualquier cambio debe cumplir los guardarraíles de su lenguaje (`harness/guardrails/`). Si incumple uno, el merge queda bloqueado. Debes comprobarlo antes de proponer un cambio como listo.
- **Idioma**: el código fuente, incluidos nombres de módulos, clases, métodos, propiedades, variables y constantes, va en inglés. Los docstrings, los comentarios y la documentación van en español. Los mensajes de log y las excepciones van en inglés.
- **Serialización JSON**: las propiedades expuestas en JSON van en `camelCase`, nunca en `snake_case`.
- **Pruebas**: el código nuevo o modificado necesita pruebas unitarias, de integración y end-to-end según corresponda. La cobertura del proyecto debe ser al menos del 80 %.
- **Documentación**: los cambios que afecten a un proyecto deben actualizar su documentación en el mismo PR.
- **Secretos**: nunca escribas contraseñas, claves, tokens ni otra información sensible en el código, en la documentación ni en los ejemplos.

## Git

- No hagas commits directos en `main`, `pre-release` ni `develop`.
- Crea ramas de trabajo desde `develop` con el formato `<tipo>/<descripción-corta>` (`feature/`, `fix/`, `docs/`, `refactor/`, `chore/`). Los `hotfix/` salen de `main`.
- Los cambios entran mediante PR, con merge por squash.
- Los commits usan la identidad configurada en git. No cambies el autor de los commits.

## Antes de terminar una tarea

1. Ejecuta las comprobaciones del proyecto: formateo, linter, tipos, pruebas y cobertura.
2. Revisa los guardarraíles aplicables y confirma que el cambio los cumple.
3. Actualiza la documentación afectada.
4. Resume al usuario qué has cambiado y qué comprobaciones has ejecutado. Si alguna no se ha podido ejecutar, dilo.
