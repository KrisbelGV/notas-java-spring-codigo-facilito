# Un vistazo a Spring Cloud, parte I

## Cloud native

Diseño de la aplicación orientado al correcto empleo de sus funcionalidades.

- **Contenedores:** Empaquetan app + dependencias.
- **Microservicios:** Piezas pequeñas e independientes.
- **APIs y config declarativa:** Todo se pide por API/archivo.
- **CI/CD y automatización:** Cambios pequeños y frecuentes.

## Monolito vs servicios

Los monolitos requieren un solo despliegue, simple, pero escala todo junto; mientras, los microservicios despliegan de forma independiente, escala y cambia por pieza, pero requiere más red y conexiones.

## Factores a tener en cuenta

- **Config en el entorno:** Lo que cambia va en variables, no en el código.
- **Procesos sin estado:** Si la instancia termina, no se pierde nada.
- **Port binding:** La app escucha el puerto que le dan (PORT).
- **Logs a stdout:** La plataforma recoge la salida estándar y guarda.
- **Desechables:** Arrancan rápido y se apagan limpio.
- **Paridad dev/prod:** Entornos lo más parecidos posible.

## Flujo del contenedor

1. **Código:** La app Spring.
2. **Dockerfile:** Instrucciones de la imagen.
3. **Imagen:** Inmutable, con su versión.
4. **Registry:** Artifact Registry.
5. **Contenedor:** Corriendo en Cloud Run.

## Modelos de nube

Qué y quién lo gestiona.

| | Máquina virtual | Kubernetes | Contenedores serverless | Funciones (FaaS) |
|---|---|---|---|---|
| Código | Nosotros | Nosotros | Nosotros | Nosotros |
| Runtime/framework | Nosotros | Nosotros | Nosotros | Compartido |
| Contenedor | Nosotros | Nosotros | Nosotros | Google |
| Orquestación | Nosotros | Compartido | Google | Google |
| SO y servidores | Nosotros | Google | Google | Google |

## Google Cloud

- **Artifact Registry:** Guardado y versionado de imágenes de contenedor.
- **Cloud Build:** Construye la imagen desde tu código.
- **Cloud Run:** Ejecuta el contenedor, escala a cero. HTTPS incluido.
- **Cloud Run functions:** Ejecuta una función por evento o petición (FaaS).
- **IAM + Workload Identity:** Permisos mínimos y acceso desde GitHub sin llaves.
- **Cloud Logging/Monitoring:** Logs, métricas y trazas de los servicios.

## Componentes

- **Service Discovery:** Ubicar otro servicio.
- **Spring Cloud Config:** Configuración.
- **Spring Cloud Gateway:** Puerta de entrada.
- **Circuit Breaker:** Resiliencia.
- **Micrometer Tracing:** Seguimiento de la petición.
- **Spring Cloud Stream:** Comunicación.

## Config server

Sirve la config. En la nube: variables de entorno + Secret Manager / ConfigMaps hacen el mismo trabajo.

## Service Discovery

Los servicios se registran con un nombre a fin de identificarlos, no importa si su dirección cambia o no. Kubernetes y Cloud Run ya lo incluyen.

## API Gateway

Spring Cloud Gateway se encarga de:

- Autenticación.
- Límite de peticiones.
- CORS.
- Logging y métricas.

Cuenta con variantes como WebMVC y Server WebFlux.

## Balanceador de carga

Situado del lado del cliente, reparte el tráfico entre las instancias disponibles.

## Estados del Circuit Breaker

1. **Cerrado:** Pasan las llamadas.
2. **Abierto (tras un fallo):** Falla rápida + fallback.
3. **Semiabierto (pasado un tiempo de espera):** Prueba unas llamadas.
4. Se repite el ciclo.

## Resiliencia

- **Timeout:** Limita la espera.
- **Retry:** Reintenta con espera creciente.
- **Bulkhead:** Compartimientos aislados.
- **Rate limiter:** Limita las peticiones.
- **Fallback:** Plan B si todo falla.

## Tracing

Cada paquete mantiene un identificador al pasar por cada servicio.

## Mensajería

El bróker o tópico retiene el evento y lo dispone para que cada servicio lo lea a su propio ritmo.

## Otros proyectos de Spring Cloud

- **Function:** Lógica como función.
- **Task:** Tareas cortas y procesos batch.
- **Contract:** Contratos entre servicios.
- **Vault:** Secretos con HashiCorp Vault.
- **Kubernetes:** Discovery y config desde K8s.
- **Bus:** Propaga cambios de config.
- **Consul-Zookeeper:** Discovery y config alternativos.
- **OpenFeign:** Clientes HTTP declarativos.

## Spring Cloud Oakwood

- Spring Cloud 2025.1.x
- Spring Boot 4.0.x/4.1.x
- Spring Framework 7
- Java 17+

`spring-cloud-dependencies`

## Spring Cloud vs Plataforma

| Problema | Spring Cloud | Plataforma (Cloud Run/K8s) |
|---|---|---|
| Descubrir servicios | Eureka - Consul | DNS/URL del servicio |
| Configuración y secretos | Config Server - Vault | Variables, Secret Manager |
| Punto de entrada | Spring Cloud Gateway | Load Balancer / API Gateway |
| Resiliencia | Circuit Breaker (Resilience4j) | Código o service mesh |
| Trazas y métricas | Micrometer Tracing | Cloud Trace / Monitoring |

## CI/CD

1. **CI: Integración Continua:** Integrar y validar cada cambio.
   - **Commit:** Git Push.
   - **Build:** Compila.
   - **Test:** Unit + Slices.
2. **CD: Entrega/Despliegue Continuo.**
   - **Imagen:** Docker + SHA.
   - **Deploy:** Staging -> prod.

## Workflow de Actions

- **Evento:** Push, pull_request, manual.
- **Workflow:** Archivo .yml en .github/workflows.
- **Job:** Corre en un runner.
- **Steps:** uses:acción - run:comando.

## Estrategias de despliegue

- **Rolling:** Reemplaza una instancia a la vez.
- **Blue/Green:** Dos entornos; cambias el interruptor.
- **Canary:** Un porcentaje pequeño prueba la versión nueva.

## Cloud Run en una imagen

- **Escala a cero:** Sin tráfico, sin instancias, sin costo.
- **Revisiones y tráfico:** Cada deploy es una revisión, reparte porcentualmente.
- **Pago por uso:** Por solicitud o tiempo de CPU y memoria.
- **HTTPS y URL incluidos:** Sin configurar certificados.
- **Nuestro contenedor, nuestro PORT:** Cualquier lenguaje o framework.

## FaaS

A una función, un evento.

**Ventajas:**

- Sin servidores.
- Escala a cero.
- Pago por ejecución.

**Desventajas:**

- Arranque en frío.
- Ejecuciones cortas.

## Flujo de GitHub a Google Cloud sin llaves

1. **GitHub Actions:** El job pide un token OIDC.
2. **Workload Identity Pool:** Valida el repositorio.
3. **Service account:** Permisos mínimos.
4. **Cloud Run:** Recibe el deploy.

## Buenas prácticas y trampas

- **Topes de gasto:** Presupuestos y número máximo de instancias desde el primer deploy.
- **Cero llaves:** WIF o secretos gestionados; nunca credenciales en Git.
- **Imágenes inmutables:** Etiqueta con el SHA del commit, no con latest.
- **Pipelines rápidos:** Cache de dependencias; falla pronto y claro.
- **Observa desde el día 1:** Logs a stdout, health checks y métricas.
- **Limpieza inteligente:** Servicios, imágenes y proyectos de práctica.

## Vistazo a la clase 2 (no confirmado)

### Flujo

1. git push
2. GitHub Actions: test + imagen
3. Artifact Registry: imagen SHA
4. Cloud Run: pokedex-api
   - PokéApi: API pública
   - Cloud Run function: pokemon-of-the-day

### Requerimientos

- Cuenta de GitHub.
- JDK 25.
- Maven.
- Docker.
- Gcloud CLI.
- Cuenta de GCP (opcional).

Demo + guía (no necesita cuenta de Google Cloud).

### Tiempo estimado

Repaso de 15 minutos e inmediatamente la práctica.