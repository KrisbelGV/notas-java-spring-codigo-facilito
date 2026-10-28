# Persistencia de Datos, Spring Data JPA y JdbcTemplate, parte 1

## ¿Qué es JDBC (Java Database Connectivity)?

API estándar de Java para hablar con bases de datos relacionales.

## Componentes

1. **Connection:** Conexión abierta con la base.
2. **PreparedStatement:** Sentencia SQL con parámetros.
3. **ResultSet:** Resultado de un SELECT a recorrer.
4. **SQLException:** Excepción de operaciones fallidas.
5. **DataSource:** Origen de conexiones

## Inyección SQL

Tipo de ataque que altera la consulta contra una base de datos, se evita empleando PreparedStatement, el cual trata el valor como un dato, en lugar de concatenarlo.

## JdbcTemplate

Patrón de Spring que simplifica el acceso al gestionando recursos, ejecutar consultas, traducir excepciones, entre otros.

## Excepciones comunes

| Excepción | Significado |
|---|---|
| `DuplicateKeyException` | Clave repetida |
| `DataIntegrityViolationException` | NOT NULL; FK, largo |
| `EmptyResultDataAccessException` | No hay filas |
| `CannotGetJdbcConnectionException` | La base no responde |

## HikariCP

Pool de conexiones defautl de Spring Boot, las inicia, mantiene y gestiona.

### Configuración

Parte de: `spring.datasource.hikari.*`

| Propiedad | Controla | Default |
|---|---|---|
| `maximum-pool-size` | Máximo de conexiones | 10 |
| `connection-timeout` | Espera si no hay libres | 30s |
| `pool-name` | Nombre en el log | HikariPool-1 |

## Datos persistentes

`application.properties`

```properties
spring.datasource.url=jdbc:h2:file:./data/db
```