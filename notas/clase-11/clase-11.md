# Persistencia de Datos, Spring Data JPA y JdbcTemplate, parte 2

## NamedParameterJdbcTemplate

JdbcTemplate con parámetros con nombre en lugar de los marcadores de posición (?).

## ORM (Object-Relational Mapping)

Realiza el mapping entre objetos de java y tablas de la BBDD.

## Capas de JPA

- **Spring Data JPA:** Escribe la implementación de los repositorios a partir de una interfaz.
- **JPA (Jakarta Persistence):** Especificación, define anotaciones y como se guardan los objetos.
- **Hibernate:** Implementación de JPA, genera el SQL, lo ejecuta y convierte las filas en objetos.
- **JDBC + HikariCP:** La base de todo (usado por Hibernate).

## Entidad

Tabla de la BBDD representada como una clase de Java.

| Java | Base de datos |
|---|---|
| Clase con `@Entity` | Tabla |
| Objeto | Fila |
| Atributo | Columna |
| Atributo con `@Id` | Clave primaria |

## Anotaciones

| Anotación | Función |
|---|---|
| `@Entity` | Marca la clase como entidad |
| `@Table(name = "nombre")` | Nombre de la tabla, el de la clase por default |
| `@Id` | Clave primaria |
| `@GeneratedValue(IDENTITY)` | Id generado, con su AUTO_INCREMENT |
| `@Column(name, nullable, length)` | Nombre de la columna y sus restricciones (opcionales) |
| `@Enumerated(EnumType.STRING)` | Guarda un ENUM como texto |

## Métodos de JpaRepository

| Método | Función |
|---|---|
| `findAll()` | Trae todos |
| `findById()` | Busca uno; devuelve Optional |
| `save()` | Inserta o actualiza |
| `existsById()` | ¿Existe? true o false |
| `deleteById()` | Borra uno |
| `count()` | Cuenta cuántos hay |

## Configuración

| Propiedad | Valor | Función |
|---|---|---|
| `spring.jpa.show-sql` | `true` | Muestra en el log el SQL |
| `hibernate.format_sql` | `true` | Añade saltos de línea |
| `spring.jpa.hibernate.ddl-auto` | `none` | Hibernate no toca la estructura |
| `spring.jpa.open-in-view` | `false` | Libera la conexión al terminar el servicio |

## Consultas derivadas

Métodos del repositorio generados por Spring en base a su nombre.

### Palabras clave

| Palabra | SQL |
|---|---|
| `And` / `Or` | `... AND ...` |
| `Not` | `columna <> ?` |
| `Between` | `columna BETWEEN ? AND ?` |
| `After` / `Before` | `columna > ?` |
| `Containing` | `LIKE '%?%'` |
| `IgnoreCase` | `UPPER(...) = UPPER(?)` |
| `OrderBy` | `ORDER BY columna DESC` |

## JPQL

Lenguaje de consulta orientado a objetos; entidades y atributos. Útil para consultas complejas. También es posible usar SQL puro especificándolo en su anotación.

## Paginación

Subdivide la respuesta en fragmentos.

| Parámetro | Función |
|---|---|
| `page` | Número de página |
| `size` | Cuántos elementos por página |
| `sort` | Orden y en base a qué |

### Pageable

Arma los parámetros del request permitiendo además el seteo por default.