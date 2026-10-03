# Clase 5: Web Applications, RESTful Services y diseño de APIs

## HTTP
Protocolo de comunicación entre cliente y servidor empleado para enviar y recibir información a través de internet.

# Encabezados
Request: Petición.
Response: Respuesta.
Content-Type/Accept: Formato de envío/respuesta.

## REST
Estilo de arquitectura para diseñar APIs comunicadas mediante HTTP.

## URI (Uniform Resource Identifier)
Dirección que identifica un recurso.

## Métodos HTTP
Determina el tipo de operación.
* GET: Obtiene el recurso.
* POST: Enviar data.
* PUT: Actualizar data.
* PATCH: Modificación parcial de la data.
* DELETE: Eliminar data.

## Clasificación
Seguro: No altera la data en el servidor.
Idempotente: Deja el recurso en el mismo estado tras la primera ejecución.

| Método | Seguro | Idempotente |
| --- | --- | --- |
| GET | Sí | Sí |
| POST | No | No |
| PUT | No | Sí |
| PATCH | No | Depende |
| DELETE | No | Sí |

## Códigos de estado
| Código | Frase de motivo | Ejemplo |
| --- | --- | --- |
| 200 | OK | GET y PUT |
| 201 | Created | POST + Location |
| 204 | No Content | DELETE |
| 400 | Bad Request | JSON inválido |
| 404 | Not Found | Recurso inexistente |
| 409 | Conflict | Duplicado o desactualizado |

## Flujo del request
En Spring Web MVC solo escribimos el paso 4.

1 . Cliente -> 2. Tomcat embebido -> 3. Dispatcher Servlet -> .4 @RestController -> 5. HTTPMessageConverter -> 6. Response JSON

## Servlet
Especificación estándar, define un contrato para una clase Java que recibe y escribe peticiones y respuestas HTTP.

## Tomcat
Contenedor de servlets, también llamado servidor web Java. Es integrado dentro de la app como una librería (embebido). Realiza tareas de bajo nivel como:
* Conexión.
* Traducción.
* HttpServletRequest.
* Hilos.
* Ruteo.
* Respuesta.
* HttpServletResponse.

## DispatcherServlet
Registra un servlet para el servidor, actúa como una única puerta de entrada que luego Spring distribuye.

## @RestController
Consiste en la suma de un @Controller y @ResponseBody “sobrescribiendo” la respuesta de la primera anotación (vista) con el formato de la segunda (JSON, por ejemplo).

## Ejecución y despliegue
1.	IDE: ```Run sobre la clase main```
2.	Maven: ```mvn spring-boot:run```
3.	Fat JAR: ```java -jar target/turnos-api-0.1.0.jar```

## Mapeo
| Anotación | Función | Ejemplo |
| --- | --- | --- |
| @PathVariable | Identifica el recurso | GET /api/turnos/{id} |
| @RequestParam | Filtra, ordena, pagina | GET /api/turnos?estado=PENDIENTE |
| @RequestBody | JSON enviado por el cliente | POST /api/turnos { ... } |
| @RequestHeader | Metadatos del request | Accept-Language: es |

## ResponseEntity
Envoltorio de la respuesta, contiene atajos de código HTTP.
