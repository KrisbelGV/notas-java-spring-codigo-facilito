# Clase 4: Spring Boot

## Auto configuración
Configura automáticamente todo lo que sea posible, en base a defaults establecidos.

## Starters
POMs de Maven, agrupan dependencias y configuraciones comunes para diferentes casos de uso.

## Convención sobre configuración
Sigue el principio, asumiendo reglas lógicas predeterminadas.

## Servidores web embebidos
Incluye todo lo necesario para proveer un servidor web preconfigurado así como los mecanismos para desplegarlo.
```XML
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

## JARS ejecutables
Habilita su generación, igualmente para web apps.

## Actuator
Herramienta para monitoreo y management mediante endpoints, ofreciendo métricas, health checks, información del ambiente y ejecución.

## Herramientas de desarrollo
Set para mejorar la experiencia (restarts automáticos, live reloading, deshabilitación de caché, mejoramiento en debugging, etc).
```XML
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <optional>true</optional>
</dependency>
```

## Spring boot initializr
Generador de proyectos Spring Boot disponible en web e IDEs.