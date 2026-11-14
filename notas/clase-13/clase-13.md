# Transaction Management

## Transacción

Secuencia de operaciones a ejecutar como una unidad única e indivisible de trabajo.

## Principios ACID

- **Atomicidad:** Si algo falla se revierte todo.
- **Consistencia:** La BBDD pasa de un estado válido a otro.
- **Aislamiento:** Transacciones concurrentes no se obstaculizan.
- **Durabilidad:** Lo confirmado sobrevive aún a fallos.

## Tipos

- **Local:** Se commitea a la base de datos.
- **Distribuida:** Varios servicios.

## Anotación `@Transactional`

Spring abre la transacción al entrar al método y la cierra al salir, commit (ejecuta los cambios) si todo sale bien o rollback (restaurar) si hay error.

## Modelo de transacciones

- **TransactionSynchronizationManager:** Recursos ligados al hilo.
  - Transacción actual.
    - Recursos transaccionales:
      - **Connection:** La misma (del DataSource).
      - **EntityManager:** Contexto de persistencia.
      - **Estado:** ej. activa.

## Novedades: Spring Framework 7 y Boot 4

- Starters modulares, ej. webmvc, data-jpa.
- Métodos protected y package desde 6.0 son transaccionales.
- Resiliencia en el core `@Retryable` y `@ConcurrencyLimit`.
- Hibernate 7 + Jakarta JPA `TransactionManager` por defecto en Spring Data JPA.

## Dependencias

- `spring-boot-starter-webmvc`
- `spring-boot-starter-data-jpa-h2`
- `spring-boot-starter-test`

## Características predeterminadas

- Crea DataSource y EntityManagerFactory.
- Registra un JpaTransactionManager.
- `@EnableTransactionManagement`.
- Expone un TransactionTemplate.

> Configura `open-in-view=false` a fin de no sobreextender el ciclo de la transacción.

## Arquitectura transaccional

1. **Intercepción de la petición.**
   - **Controller:** Al invocar un método `@Transactional` la llamada es interceptada por un proxy.
   - **Proxy CGLIB:** Spring lo crea dinámicamente.
   - **TransactionInterceptor:** Interceptor transaccional, se encarga de la gestión técnica antes y después de esta.
2. **Apertura de la transacción y conexión.**
   - **Begin:** El interceptor inicia formalmente la transacción.
   - **Abre la conexión:** Se solicita una conexión a la base de datos y se desactiva el auto-commit (autocommit off) para asegurar que múltiples operaciones ocurran bajo una misma unidad atómica.
3. **Ejecución de la lógica de negocio.**
   - **Ejecución de la lógica de negocio:** Una vez abierta la transacción, el control pasa al método dentro del componente de servicio.
   - **Debit/Credit:** Se ejecutan las operaciones de base de datos. Todas comparten estrictamente la misma conexión transaccional.
   - **return o excepción:** El método finaliza su ejecución devolviendo un resultado con éxito o lanzando una excepción.
4. **Cierre de la transacción.**
   - **COMMIT o ROLLBACK:**
     - Si el método termina correctamente, se ejecuta un COMMIT para guardar los cambios de forma permanente en la base de datos.
     - Si el método lanza una excepción no controlada se ejecuta un ROLLBACK para deshacer cualquier cambio y mantener la consistencia de los datos.

## Propagación

Define cómo se comportan las operaciones cuando un método transaccional llama a otro que también la requiere.

### Modos

| Nombre | Qué hace | Cuándo usarlo |
|---|---|---|
| REQUIRED | Se une o crea una nueva (default) | Lo normal, casi siempre |
| REQUIRES_NEW | Suspende la actual y crea otra | Auditoría que debe sobrevivir |
| NESTED | Savepoint dentro de la actual | Reintentar una parte (savepoints, JDBC) |
| SUPPORTS | Usa la tx si existe, sino sin tx | Lecturas que pueden correr sin transacción |
| NOT_SUPPORTED | Suspende la tx y corre sin ella | |
| MANDATORY | Exige tx, si no hay, excepción | Nunca debe correr solo |
| NEVER | Prohíbe tx, si hay, excepción | |

## Rollback y operaciones

- **Sí hace Rollback:**
  - RuntimeExceptions (unchecked).
  - Error (OutOfMemoryError).
  - Excepciones de rollbackFor.
  - setRollbackOnly manual.
  - Rollback-only de un método interno.
- **No hace rollback:**
  - Checked exception (por defecto).
  - Excepción capturada con try/catch.
  - Auto invocación (this.metodo()).
  - Método private o fuera del proxy.
  - Hilo nuevo (Async, parallel).

## Eventos

Responde a eventos transaccionales con el uso de la anotación `@TransactionalEventListener`.

| Nombre | Cuándo | Cuándo usarlo |
|---|---|---|
| BEFORE_COMMIT | Justo antes del commit | Validar dentro del tx |
| AFTER_COMMIT | Solo si hubo commit (default) | Email, bróker, invalidar caché |
| AFTER_ROLLBACK | Solo si hubo rollback | Compensar, alertar o registrar |

## Concurrencia, locking e isolation

La propiedad "versión" es añadida al registro a fin de evitar escrituras simultáneas en consultas concurrentes.

### Tipos de bloqueo

- **Optimista:** Baja probabilidad de conflictos, protege si la versión no coincide.
- **Pesimista:** Colisiones frecuentes, se bloquea el acceso mientras se edita.

### Niveles de aislamiento y anomalías

| Nivel de Aislamiento | Lectura sucia | Lectura no repetible | Lectura fantasma |
|---|---|---|---|
| READ_UNCOMMITTED | Posible | Posible | Posible |
| READ_COMMITTED | No | Posible | Posible |
| REPEATABLE_READ | No | No | Posible |
| SERIALIZABLE | No | No | No |

## Timeout

Define el tiempo máximo de vida para esa transacción específica.

## Repositorio oficial
[Proyecto visto en la clase](https://github.com/geezylucas/transaction-management-bootcamp-backend-java-spring-g2), por el instructor, [Gerardo Lucas](https://github.com/geezylucas).