# Guía de arquitectura: backend

Guía de arquitectura para los proyectos de backend.

## Principio general

- Los proyectos de backend siguen Domain-Driven Design (DDD).
- El código se organiza en capas. Una capa solo depende de las capas indicadas en la regla de dependencias.
- Las dependencias apuntan hacia el dominio. El dominio no depende de ninguna otra capa ni de ningún framework.

### Regla de dependencias

```
interfaces → application → domain ← infrastructure
```

- `domain` no importa nada de `application`, `infrastructure` ni `interfaces`, ni de FastAPI, SQLAlchemy ni otros frameworks.
- `application` importa de `domain`, pero no de `infrastructure` ni de `interfaces`.
- `infrastructure` implementa los puertos definidos en `domain` y puede importar de `domain` y `application`.
- `interfaces` expone los casos de uso de `application` al exterior (HTTP, CLI, runner).

## Estructura de carpetas

Cada proyecto de backend sigue esta estructura dentro de `src/<nombre_paquete>/`:

```
src/<nombre_paquete>/
├── domain/
│   ├── entities/
│   ├── value_objects/
│   ├── repositories/
│   ├── services/
│   └── exceptions.py
├── application/
│   ├── use_cases/
│   └── dtos/
├── infrastructure/
│   ├── persistence/
│   │   ├── models.py
│   │   ├── mappers.py
│   │   └── repositories/
│   └── config/
└── interfaces/
    ├── api/
    │   ├── routers/
    │   └── schemas/
    └── cli/
```

Si el dominio se extrae a un paquete independiente, como `packages/domain/`, su estructura interna es la de `domain/` y las demás capas lo importan como dependencia.

## Descripción de las capas

- **`domain/`**: el núcleo del negocio. No depende de ninguna tecnología.
  - `entities/`: entidades con identidad y reglas de negocio. Ejemplo: `project.py` con la clase `Project`.
  - `value_objects/`: objetos inmutables definidos por sus valores. Ejemplo: una versión SemVer.
  - `repositories/`: puertos, es decir, interfaces (`typing.Protocol`) que definen cómo se persisten las entidades. Ejemplo: `project_repository.py` con `ProjectRepository`.
  - `services/`: lógica de dominio que no pertenece a una sola entidad.
  - `exceptions.py`: excepciones propias del dominio, como `ProjectNotFoundError`.
- **`application/`**: orquesta los casos de uso. Coordina entidades y puertos, pero no contiene reglas de negocio.
  - `use_cases/`: un caso de uso por fichero, con un único método público de ejecución. Ejemplo: `create_project.py`.
  - `dtos/`: objetos de entrada y salida de los casos de uso, independientes de HTTP y de la base de datos.
- **`infrastructure/`**: detalles técnicos. Implementa los puertos del dominio.
  - `persistence/models.py`: modelos ORM de SQLAlchemy, como `ProjectModel`. No salen de esta capa.
  - `persistence/mappers.py`: conversión entre modelos ORM y entidades de dominio.
  - `persistence/repositories/`: implementaciones de los puertos, como `SqlAlchemyProjectRepository`.
  - `config/`: lectura de configuración desde variables de entorno.
- **`interfaces/`**: adaptadores de entrada, sin lógica de negocio.
  - `api/routers/`: rutas de FastAPI que llaman a los casos de uso.
  - `api/schemas/`: modelos de entrada y salida de la API. Se exponen en `camelCase`.
  - `cli/`: comandos de línea de comandos, si el proyecto los necesita.

## Reglas de nombrado

- Las entidades de dominio no llevan sufijo: `Project`.
- Los modelos ORM llevan el sufijo `Model`: `ProjectModel`.
- Los puertos de repositorio se nombran por la entidad: `ProjectRepository`.
- Las implementaciones indican la tecnología: `SqlAlchemyProjectRepository`.
- Los casos de uso se nombran con un verbo: `CreateProject`, `GetProject`.

## Creación de entidades del dominio

Cada vez que se crea una entidad del dominio, se crean también los siguientes elementos en el mismo cambio:

1. **Contrato del repositorio** (`domain/repositories/`): puerto `typing.Protocol` con las operaciones de persistencia de la entidad. Ejemplo: `ProjectRepository`.
2. **Modelo de persistencia** (`infrastructure/persistence/models.py`): clase ORM de SQLAlchemy que representa la tabla. Ejemplo: `ProjectModel`. Los tamaños de las columnas coinciden con las longitudes máximas definidas en la entidad.
3. **Repositorio de persistencia** (`infrastructure/persistence/repositories/`): implementación del contrato con SQLAlchemy. Ejemplo: `SqlAlchemyProjectRepository`. Recibe la sesión por inyección y devuelve entidades de dominio, nunca modelos ORM.
4. **Mapper** (`infrastructure/persistence/mappers.py`): conversión bidireccional entre el modelo ORM y la entidad de dominio.

- Una entidad no se considera completa hasta que tenga estos cuatro elementos y sus pruebas.
- Las pruebas de la entidad son unitarias. Las del repositorio y el mapper son de integración contra una base de datos real.

## Pruebas por capa

- Las pruebas de `domain` y `application` son unitarias y no necesitan base de datos ni red.
- Las pruebas de `infrastructure` son de integración y usan una base de datos real.
- Las pruebas de `interfaces` comprueban los contratos HTTP o de CLI.
- La estructura de `tests/` refleja la de `src/`.
