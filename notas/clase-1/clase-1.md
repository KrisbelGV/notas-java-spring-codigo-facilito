# Clase 1: Inauguración e Introducción: ¿Qué ha cambiado desde la primera edición de este Bootcamp?

## NOVEDADES EN SPRING FRAMEWORK 7

### CORE PLATFORM
* Evolución significativa
* Requiere Java 17 como mínimo
* Actualización nivel base:
    * Jakarta EE 11
    * Servlet 6.1
    * JPA 3.2, Bean
    * Validation 3.1
    * WebSocket 2.2
* Deprecated:
    * Jackson 2.0
    * RestTemplate
    * Jackson 2.x
* Removido:
    * Undertow
    * spring-jcl
    * javax.annotation
    * javax.inject

### FEATURES
* Anotaciones de JSpecify por defecto
* Nuevo mecanismo para API versioning
* Nuevas características nativas de resiliencia
* Interfaz de HTTP Client mejorada
* Introduce Programmatic Bean Registration
* Nuevo JmsClient

## NOVEDADES EN SPRING BOOT 4
* Gran refactor en busca de modularidad
* Mejoras en el soporte a GraalVM Native Images
* Soporte a migración:
    * Introduce spring-boot-starter-classic y spring-
boot-starter-classic-test como ayuda para migración gradual de 3.x a 4.x
    * Incluye spring-boot-properties-migrator para
detectar configuraciones a migrar en tus archivos
properties
* Testing:
    * @SpringBootTest ya no incluye por defecto:
        * MockMvc
        * WebClient
        * TestRestTemplate
    * Actuator: liveness y readiness probes habilitadas
por defecto