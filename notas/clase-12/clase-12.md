# Spring Security para Servicios REST

## OWASP (Open Worldwide Application Security Project)

Organización sin fines de lucro dedicada a mejorar la seguridad del software.

### Top 10 riesgos de seguridad

1. ✅ Autorización a nivel de Objeto (BOLA).
2. ✅ Autorización rota.
3. ✅ Autorización a nivel de propiedad (BOPLA).
4. Consumo irrestricto de recursos.
5. ✅ Autorización a nivel de función (BFLA).
6. Flujo de negocio sensibles sin control.
7. Server-Side Request Forgery (SSRF).
8. ⚠️ Mala configuración de seguridad.
9. Inventario de APIs mal gestionado.
10. Consumo inseguro de APIs.

> ✅ = Fallos que Spring ya maneja · ⚠️ = Fallos que debemos cuidar

## Autenticación

Verifica la identidad de un usuario, dispositivo, sistema u aplicación.

## Autorización

Determina qué acciones, datos o recursos puede utilizar un cliente autenticado.

## Modelo de autenticación

- **SecurityContextHolder:** Dispone la referencia para los métodos (mismo hilo).
- **SecurityContext:** Contiene la información de seguridad de la sesión.
- **Authentication:** Solicitud o información del usuario autenticado.
- **Principal:** Quién es.
- **Credentials:** Contraseña o token.
  > Por seguridad se limpia tras completarse.
- **Authorities:** Colección de permisos o roles.

## Novedades de Spring Security

### Características

- Multi-factor Authentication.
- DSL solo con lambdas.
- PathPatternRequestMatcher.
- CSRF para SPAs.

### Dependencias / ecosistema

- Spring Authorization Server y Kerberos.
- Jackson 3 y OAuth 2.1.

## Dependencias

- `spring-boot-starter-security`
- `spring-boot-starter-oauth2-resource-server`
- `spring-boot-starter-security-test`

## Características predeterminadas

- Protege TODAS las rutas.
- Crea el usuario "user" con contraseña aleatoria.
- Activa form login y HTTP Basic.
- Habilita CSRF y headers de seguridad.
- Usa sesión (cookie JSESSIONID).

## Arquitectura de seguridad

1. **Servlet Container (Tomcat):** Recibe la petición HTTP cruda.
2. **DelegatingFilterProxy:** Puente Servlet <-> Spring.
3. **FilterChainProxy (springSecurityFilterChain):** Bean que administra la cadena de filtros.
4. **SecurityFilterChain:** Secuencia de filtros.
   - **SecurityContextHolderFilter:** Inicializa el contexto.
   - **HeaderWriterFilter:** Añade cabeceras.
   - **CorsFilter/CsrfFilter:** Políticas de intercambio de recursos y protección contra ataques.
   - **Basic/Bearer Auth Filter:** Extrae y valida credenciales de autenticación.
   - **Exception Translation Filter:** Captura excepciones de seguridad traduciéndolas a HTTP.
   - **Authorization Filter:** Valida permisos y roles.
5. **DispatcherServlet:** Enruta a nuestro `@RestController`.

## Múltiples cadenas de seguridad

Es posible configurarlas siguiendo el orden de reglas acorde a la primera coincidencia respecto a su ruta.

> Definir una configuración propia sobrescribirá el default de Spring, incluyendo la denegación no autenticada.

## Buenas prácticas en gestión de contraseñas

- Ajusta el work factor -1 segundo por validación.
- Nunca SHA-256 o MD5 simples.
- Usa DelegatingPasswordEncoder.
- Cambia credenciales largas por tokens de vida corta.
- No registrar contraseñas en los logs.
- Se aconseja priorizar la validación por Tokens y el uso de OAuth2.

## Seguridad a nivel de método

Se activa con la anotación `@EnableMethodSecurity` e indica con `@PreAuthorize` (validación previa) y `@PostAuthorize` (posterior a ejecutar).

## Sesión (Cookie, JSESSIONID)

Se guarda el estado en el servidor dispensando una cookie para el navegador, se usa en apps web.

## Token (Authorization, Bearer)

El estado viaja con este, empleado en APIs REST, SPAs y microservicios.

## Partes de JWT (Json Web Token)

- **Header:** Indica el algoritmo de la firma.
- **Payload:** Identifica el emisor, usuario, API, tiempo de expiración y permisos.
  > Nunca guardar secretos aquí, su información puede ser leída por cualquiera.
- **Firma:** A calcular en base a la clave privada que suministra y se verifica con la llave pública.

## Flujo de autorización con OAuth2

**Actores:**

- Cliente: SPA/App.
- Servidor de autorización.
- API.

**Paso a paso:**

1. **Petición de autorización:** El cliente realiza una petición GET /authorize al Servidor de Autorización incluyendo el code_challenge (generado mediante PKCE).
2. **Autenticación y consentimiento:** El Servidor de Autorización interactúa con el usuario para que inicie sesión (Login) y otorgue su consentimiento.
3. **Entrega del código:** Una vez el usuario se autentica correctamente, el Servidor de Autorización devuelve al cliente un authorization code (código de autorización).
4. **Intercambio por Token:** El cliente envía una petición POST /token de vuelta al Servidor de Autorización.
5. **Entrega del Token:** El Servidor de Autorización valida el code_verifier y responde entregando el access_token (formato JWT).
   > El code_verifier corresponde al secreto original generado en code_challenge del paso uno, demostrando que es el mismo cliente que inició el flujo.
6. **Petición al recurso:** El cliente ya autenticado realiza peticiones a la API incluyendo el token en las cabeceras de autorización.
7. **Validación del Token:** El Resource Server recibe la petición y valida la firma del token.

## Opciones para emisión de tokens

- **Proveedor externo:** La API solo valida. Opción recomendada en producción.
- **Spring Authorization Server:** Novedad en Spring Security 7, útil al necesitar IdPs propios.
- **JwtEncoder:** Solo demo, no se utiliza más que para aprender y pruebas.

## CORS

Controla qué orígenes pueden leer las respuestas, aplica al navegador.

## CSRF

Ataque donde el navegador envía las cookies solo.

## Métodos de autenticación modernos

- **One-Time Token:** Login sin contraseña con un enlace o código de un solo uso.
- **Passkeys/WebAuthn:** Credenciales resistentes al phishing sin contraseña.
- **MFA integrado:** Exige varios factores, se registra cada uso y cuándo.