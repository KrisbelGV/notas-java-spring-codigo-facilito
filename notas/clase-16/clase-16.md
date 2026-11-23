# Un vistazo a Spring Cloud, parte II

> **Aviso importante:** Esta clase emplea servicios en la nube. Sigue con atención los pasos del instructor, ya que un despliegue incorrecto podría generar cargos.

## Mapa de la primera clase (repaso)

- **Cloud native:** Contenedores + 12 factores.
- **Spring Cloud:** Una pieza por problema.
- **CI/CD:** La línea de ensamblaje.
- **FaaS:** Una función por evento.

## La aplicación

1. Cliente.
2. **pokedex-api:** Spring Boot en Cloud Run.
   - **PokéApi:** API pública.
   - **pokemon-of-the-day:** Cloud Run function.

## Consumo de API pública con Rest Client

- Builder inyectado.
- URL por configuración.
- 404 -> excepción propia.
- Un record por respuesta.

## Resiliencia sin framework

- URL vacía -> Optional.empty()
- Llamada a la función -> id
- Error / timeout -> Optional.empty()

## Test sin internet

- **Unit:** Servicio con Mockito.
- **Slice web:** @WebMvcTest + @MockitoBean.
- **Cliente HTTP:** MockRestServiceServer.

## Dockerfile multi-stage

- **Etapa build:** JDK + Maven + código + pesada.
- **Imagen final:** JRE + app.jar + ligera.
- **PORT lo inyecta Cloud Run:** `server.port=${PORT:8080}`

## Probar el contenedor en local

- **Puerto por entorno:** `-e PORT=9090` cambia el puerto sin tocar el código.
- **Health check:** `/actuator/health`: lo usa la plataforma.
- **Misma imagen en todos lados:** laptop = staging = producción.

## Primer despliegue a mano

- **Sube el código:** `--source` empaqueta la carpeta.
- **Cloud Build:** Usa el Dockerfile.
- **Artifact Registry:** Guarda la imagen.
- **Cloud Run:** Crea la revisión 1.

## Repositorio oficial

[Enlace al proyecto](https://github.com/geezylucas/spring-cloud-gcp-bootcamp-backend-java-spring-g2), por el instructor, [Gerardo Lucas](https://github.com/geezylucas).