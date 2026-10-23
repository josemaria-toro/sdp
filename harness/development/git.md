# Guía de desarrollo: Git

Guía con la estrategia de ramas.

## Ramas permanentes

- `main`: contiene el código en producción. Solo recibe merges desde `pre-release` y desde ramas de `hotfix`.
- `pre-release`: contiene el código preparado para publicar. Recibe merges desde `develop`.
- `develop`: contiene la integración del trabajo en curso. Es la rama de la que salen las ramas de trabajo.

No se hacen commits directos sobre ninguna rama permanente. Todo cambio entra mediante un Pull Request (PR).

## Flujo de promoción

```
feature/fix/docs/refactor/chore → develop → pre-release → main
```

1. Las ramas de trabajo salen de `develop` y se integran en `develop` mediante PR.
2. Cuando `develop` está listo para publicar, se abre un PR de `develop` a `pre-release`.
3. Cuando `pre-release` está validado, se abre un PR de `pre-release` a `main`.

## Ramas de trabajo

- Cada tarea se desarrolla en una rama corta creada a partir de `develop`.
- Nombre de las ramas: `<tipo>/<descripción-corta>`, en minúsculas y separadas por guiones.
  - Tipos: `feature/`, `fix/`, `docs/`, `refactor/`, `chore/`.
  - Ejemplos: `feature/login-oauth`, `fix/crash-al-guardar`.
- Las ramas deben ser cortas: se integran en `develop` lo antes posible y se eliminan tras el merge.

## Ramas de hotfix

- Se usan solo para corregir incidencias urgentes en producción.
- Salen de `main` con el nombre `hotfix/<descripción-corta>`.
- Se integran en `main` mediante PR. Después, el mismo cambio se propaga a `pre-release` y a `develop` mediante PR para que no se pierda en la siguiente publicación.

## Pull Requests

- Cada PR debe tener una descripción clara del cambio y su motivo.
- Antes de hacer merge, el PR debe tener una revisión aprobada y los checks en verde.
- Todos los merges en ramas permanentes (`develop`, `pre-release`, `main`) se hacen por "squash" para mantener un historial limpio.

## Commits

- Mensajes en imperativo y en una sola línea descriptiva (por ejemplo, `Añade validación del formulario de registro`).
- Cada commit debe dejar el proyecto en un estado que compile y pase las pruebas.

## Versiones

- Las versiones se etiquetan en `main` con tags anotados siguiendo [SemVer](https://semver.org/lang/es/) (`vMAJOR.MINOR.PATCH`).
- Cada tag de `main` se crea tras el merge de `pre-release` o de un `hotfix`.
