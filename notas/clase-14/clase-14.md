# Testing de Aplicaciones Spring

## Tiempo de detección de un bug

- **Al escribir:** Segundos.
- **En el test:** Minutos.
- **En CI/QA:** Horas.
- **En producción:** Días.

> Ilustrativo, no son cifras medidas.

## Orden de tests (por costo)

1. **Unitarios:** Muchos, milisegundos, sin Spring.
2. **Integración:** Algunos, contexto Spring.
3. **E2E/UI:** Pocos, lentos, frágiles.

## Unit test

Prueba una clase o método; Mocks y objetos simples, sin Spring.

## Integration test

Prueba varias piezas juntas; contexto Spring, BD, HTTP.

## Características de un buen test

### AAA

- **Arrange:** Prepara datos y dobles.
  > Un doble es un objeto o componente que sustituye a dependencias reales.
- **Act:** Ejecuta una acción.
- **Assert:** Verifica el resultado.

### FIRST

- **Fast:** Rápido.
- **Independent:** Sin orden.
- **Repeatable:** Igual siempre.
- **Self-validating:** Pasa o falla.
- **Timely:** Junto al código.

## Tipos de test doubles

- **Dummy:** Relleno, se pasa pero no se usa.
- **Stub:** Respuesta fija.
- **Mock:** Verifica llamadas.
- **Spy:** Objeto real vigilado.
- **Fake:** Implementación ligera real.

Primero lo simple: usa el doble más sencillo que haga pasar el test.

## Componentes activos en un test de Spring Boot

- **Test (JUnit 6).**
- **TestContext (framework de Spring).**
- **ApplicationContext (todos los beans).**
- **Base de datos (H2/Postgres).**

### Según qué se va a levantar

- **Mock:** Sin servidor real, usa MockMvc.
- **RANDOM_PORT:** Tomcat en puerto libre. RestTestClient.
- **None:** Sin entorno web. Solo servicios y JPA.

## Dependencias

- `spring-boot-starter-test`
- `spring-boot-starter-webmvc-test`
- `spring-boot-starter-data-jpa-test`
- `spring-boot-resttestclient`
- `spring-boot-testcontainers`

## Caché de contexto

Test con la misma configuración reutiliza la caché, haciéndolos más rápidos que otros.

## Slice testing

Carga solo la capa a probar.

- **@WebMvcTest:** Controllers, @ControllerAdvice, JSON, filtros. El servicio se simula con @MockitoBean.
- **@DataJpaTest:** Entidades, repositorios y BD embebida. Transacción con rollback por test.

## Testing en APIs Rest

Verifica:

- Status.
- Cuerpo JSON.
- Errores.
- Efecto en la BD.

Usa:

- **MockMvcTester (slice web):** Para simular la petición sin servidor.
- **RestTestClient (integración):** Servidor, petición y contrato con la BD real.

## Perfiles y configuración

Para aislar el entorno:

- **@ActiveProfiles:** Activa application-test.yml.
- **@TestConfiguration:** Agrega o reemplaza beans.
- **@DynamicPropertySource:** Puertos de contenedores.

## Testcontainers

Un contenedor puede contener muchos test. Utiliza Bean + @ServiceConnection, H2 para la mayoría, la BBDD real para lo crítico.

## TDD

- **Rojo:** Escribe un test que falle.
- **Verde:** Código mínimo para pasar.
- **Refactor:** Limpia sin romper nada.