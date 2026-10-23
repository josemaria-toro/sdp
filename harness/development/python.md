# Guía de desarrollo: Python

Guía para el desarrollo de aplicaciones basadas en Python.

## Versión y entorno

- Usar Python 3.13 o superior.
- Cada proyecto gestiona sus dependencias con `pyproject.toml`.
- Crear un entorno virtual por proyecto (`.venv`). Nunca instalar dependencias en el intérprete global.
- El fichero de bloqueo de dependencias (`uv.lock` o equivalente) se versiona en Git.

## Estructura del proyecto

- El código fuente vive en `src/<nombre_paquete>/`.
- Las pruebas viven en `tests/` y reflejan la estructura de `src/`.
- Los scripts de ejecución y utilidades van en `scripts/`, nunca mezclados con el paquete.
- No se incluyen en Git artefactos generados (`__pycache__`, `.venv`, cachés de herramientas, `dist/`).

## Idioma

- El código fuente se escribe siempre en inglés: módulos, paquetes, clases, métodos, funciones, propiedades, variables y constantes.
- Los docstrings se escriben en español.
- Los comentarios se escriben en español.
- Los mensajes de log y las excepciones se escriben en inglés, ya que forman parte de la interfaz técnica del código.

## Estilo de código

- Seguir PEP 8. El formateo se hace con `black` y la comprobación de estilo con `ruff` (linter).
- Los nombres de módulos, funciones y variables usan `snake_case`. Las clases usan `PascalCase`. Las constantes usan `UPPER_SNAKE_CASE`.
- Las funciones públicas deben tener anotaciones de tipo completas.
- Evitar `from módulo import *`.
- No usar valores mutables como argumentos por defecto.
- Los métodos y propiedades de una clase se ordenan alfabéticamente. Primero los públicos y después el resto.

## Tipado

- Todo el código nuevo debe pasar `mypy` en modo estricto (`strict = true`).
- Usar tipos modernos: `list[str]`, `dict[str, int]`, `X | None`. No usar `typing.List`, `typing.Dict` ni `Optional`.

## Serialización JSON

- Las propiedades de los datos serializados en JSON se exponen siempre en `camelCase`. Nunca en `snake_case`.
- Los atributos internos del código Python siguen usando `snake_case` según PEP 8. La conversión a `camelCase` se hace solo en la capa de serialización (por ejemplo, con alias de `pydantic`).

## Manejo de errores y logging

- Capturar excepciones concretas. Evitar `except Exception` sin relanzar o registrar el error.
- Usar el módulo `logging`. No usar `print` para información de ejecución.
- No registrar secretos, tokens ni datos personales.

## Pruebas

- Usar `pytest` como framework de pruebas.
- Cada módulo de lógica debe tener pruebas unitarias. Las pruebas de integración se separan con un marcador (`@pytest.mark.integration`).
- Las pruebas deben ser deterministas: no dependen del orden de ejecución, del reloj del sistema ni de la red, salvo que estén marcadas como integración.
- Cobertura mínima obligatoria: 80 % de todo el código del proyecto (ver [guardarraíles](../guardrails/python.md)).

## Documentación en el código

- Docstrings en formato Google para módulos, clases y funciones públicas.
- Los comentarios explican el *porqué*, no el *qué*.

## Herramientas de calidad

- Configurar `pre-commit` con `black`, `ruff` y `mypy` para ejecutar las comprobaciones antes de cada commit.
- La integración continua debe ejecutar `black --check`, `ruff`, `mypy` y `pytest`. Un fallo en cualquiera bloquea el merge.

## Dependencias

- Añadir solo las dependencias necesarias y mantenerlas actualizadas.
- Fijar versiones mínimas en `pyproject.toml` y dejar la resolución exacta al fichero de bloqueo.
- Antes de añadir una dependencia, comprobar que tiene licencia compatible con GPL v3.
