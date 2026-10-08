# Clase 6: Web Applications, RESTful Services y diseño de APIs, Parte 2

## Comparativa de clientes HTTP

| Cliente | Modelo | API | Requiere | Estado |
|---|---|---|---|---|
| `RestTemplate` (deprecado) | Síncrono | Métodos sueltos | `spring-web` | Mantenimiento |
| `RestClient` | Síncrono | Fluida | `starter-restclient` | Recomendado en MVC |
| `WebClient` | Reactivo, no bloqueante | Fluida | WebFlux (Reactor) | Recomendado en WebFlux |

## Clientes declarativos con `@HttpExchange`

Spring escribe la implementación en base a nuestra descripción de la API remota.

```java
@HttpExchange("/prefijo-base-del-servicio")
public interface NombreCliente {

    @GetExchange("/subruta/{variable1}/{variable2}/…/{variableN}")
    TipoRetorno nombreDelMetodo(
        @PathVariable TipoDato variable1,
        @PathVariable TipoDato variable2…
        @PathVariable TipoDato variableN
    );
}
```

### Registro (Spring 7 y Boot 4)

Sin implementación manual, solo grupos + properties.

```java
@Configuration
@ImportHttpServices(
    group = "nombre-del-grupo",
    types = NombreCliente.class
)
public class NombreDeLaConfiguracion {}
```

```properties
spring.http.serviceclient.nombre-del-grupo.base-url=https://dominio-del-api.com
spring.http.serviceclient.nombre-del-grupo.read-timeout=tiempo
```

## RFC 9457 (Problem Details for HTTP APIs)

Formato estándar de errores: `Content-Type application/problem+json`. Soportado por Spring con `ProblemDetail`.

### Anatomía de un `ProblemDetail`

Permite extender propiedades propias sobre las cinco estándar.

```http
HTTP/1.1 CODIGO_ESTADO MensajeEstado
Content-Type: application/problem+json

{
  "type": "https://dominio-api.com/errores/tipo-de-error",
  "title": "Título Breve Del Error",
  "status": CODIGO_ESTADO_NUMERICO,
  "detail": "Explicación puntual de lo que ocurrió con los datos recibidos",
  "instance": "/ruta/del/endpoint/o/recurso",
  "propiedadExtendida": "valorOObjeto",
}
```

### Excepciones de dominio y el handler

El handler global "traduce" el HTTP, extiende sobre `ResponseEntityExceptionHandler`. El resto de errores de Spring MVC son generados por este con `ProblemDetail` igualmente.

```java
@RestControllerAdvice
public class GlobalExceptionHandler extends ResponseEntityExceptionHandler {

    @ExceptionHandler(ExcepcionEspecíficaException.class)
    public ProblemDetail manejarExcepcionEspecífica(
        ExcepcionEspecíficaException ex,
        HttpServletRequest request
    ) {
        ProblemDetail problemDetail = ProblemDetail.forStatusAndDetail(
            HttpStatus.CODIGO_ESTADO,
            ex.getMessage()
        );

        problemDetail.setType(URI.create("https://dominio-api.com/errores/tipo-de-error"));
        problemDetail.setTitle("Título Breve Del Error");
        problemDetail.setInstance(URI.create(request.getRequestURI()));

        problemDetail.setProperty("propiedadExtendida", ex.getDato());

        return problemDetail;
    }
}
```

## Estrategias de versionado

| Tipo | Ejemplo | Propiedad |
|---|---|---|
| Path | `GET /v{version}/{recurso}` | `spring.mvc.apiversion.use.path-segment` |
| Query param | `GET /{recurso}?{param}={version}` | `spring.mvc.apiversion.use.header` |
| Header | `{Nombre-Header}: {version}` | `spring.mvc.apiversion.use.query-parameter` |
| Media type | `Accept: application/json;{param}={version}` | `spring.mvc.apiversion.use.media-type-parameter` |