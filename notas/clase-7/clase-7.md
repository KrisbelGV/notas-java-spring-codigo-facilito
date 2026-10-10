# Clase 7: Web Applications, RESTful Services y diseño de APIs, Parte 3

## Bean Validation

Especificación de Java llamada Jakarta Validation, define sus reglas mediante anotaciones como contrato luego ejecutadas por la implementación oficial (Hibernate Validator).

## Validadores comunes

Se añaden sobre los campos del DTO. Requiere usar `@Valid` en el controller.

| Validador | Restricción |
|---|---|
| `@NotNull` | No admite null |
| `@NotBlank` | No null, ni "", ni " " |
| `@Size` | Longitud del String o colección |
| `@Min` / `@Max` | Rangos numericos |
| `@Email` - `@Pattern` | Formato de texto |
| `@Future` - `@Past` | Fechas |

## Buenas prácticas

1. Recursos en plural.
2. Status codes correctos.
3. Errores consistentes.
4. DTOs ≠ modelo.
5. Validar formatos y dominios.
6. Versionar cambios incompatibles.
7. Paginar colecciones.
8. Documentar el contrato.

## OpenAPI

Especificación estándar (JSON/YAML) que describe endpoints, parámetros, esquemas y respuestas.

## Swagger UI

Interfaz web que lee la especificación y permite probar la API.

## springdoc-openapi

Genera la especificación mirando Spring MVC en runtime.

**Code-first:** El código genera el contrato.

**Contract-first:** Se escribe el contrato y se genera el código.

## Anotaciones de SpringDoc

| Anotación | Propósito |
|---|---|
| `@OpenAPIDefinition` | Título, versión, contacto |
| `@Tag` | agrupa endpoints |
| `@Operation` | resumen y descripción |
| `@ApiResponse` | documenta los errores |
| `@Schema(example)` | precarga "Try it out" |
| `@Hidden` | oculta un endpoint |

## JdbcClient

Interfaz para trabajar con BBDD relacionales.

## Hilo

Unidad de ejecución más pequeña dentro de un programa.

### Tipos

- **Plataforma:** Uno hilo del SO por request, caros en memoria y cambio de contexto.
- **Virtuales:** Gestionados por la JVM, se pueden crear por millones.

### Activación

`application.properties`

```properties
spring.threads.virtual.enabled=true
```

## AOT (Ahead-Of-Time)

Reduce el tiempo de arranque al precompilar y cargar las clases antes de su ejecución.

## Native Images

Compilación ejecutable para el SO. Requiere GraalVM, una JDK.

## Spring Modulith

Plantilla de Spring para un monolito separado en módulos.