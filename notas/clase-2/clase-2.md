# Clase 2: Introducción a Spring Framework y Contenedor de Inversión de Control, Parte 1

## Historia
Propuesto por primera vez en alrededores del año 2000 por Rod Johnson junto al modelo de configuración basado en inyección de dependencias y se popularizó rápidamente debido a su simplicidad, flexibilidad y extensibilidad.

## ¿Qué es?
Un framework para el desarrollo de aplicaciones en Java, de código abierto y que simplifica la creación de software de gran escala. Proveé módulos para facilitar el desarrollo de diferentes aspectos de una aplicación permitiendonos elegir cuales utilizar.

## Contenedor de inyección de dependencias (DI)
Se encarga de manejar los objetos de nuestra aplicación, quién instancia e inyecta las dependencias entre sí. También conocido como IoC, siendo más bien una implementación del concepto.

## Inversión de control
Principio de diseño, consiste en la inversión del control del flujo del código hacia instancias específicas mediante un framework que gestiona su creación y entrega.

## Packages Base
El DI cuenta con org.springframework.beans y org.springframework.contex, y estos a su vez con BeanFactory y ApplicationContext como interfaces principales. Manejan la gestión de la configuración, los beans y la inyección de dependencias.

## Beans
Objetos que forman parte de la app instanciados, configurados y manejados por Spring.

## POJO (Plain Old Java Object)
Clase ordinaria e independiente.

## Configuración del DI
Indicada por medio de una clase. Esto puede realizarse a través de una llamada directa:
```java
@Configuration
public class ApplicationConfig{
  @Bean
  public Interfaz bean() {
    return new Clase(dependencia());
  }
}
```
U inyección por argumentos: 
```java
@Configuration
public class ApplicationConfig{
  @Bean
  public Interfaz bean(TipoDependencia dependencia){
    return new Clase(dependencia);
  }
}
```

## Creación y uso de ApplicationContext
> Abordado en la clase 3, mas perteneciente a la presente (orden del repositorio)

Representa el DI, se puede emplear en diversos ambientes (app standalone, web application, test enviroment, etc).
```java
ApplicationContext context = SpringApplication.run(ApplicationConfig.class); 
Interfaz bean = context.getBean("bean", Interfaz.class); 
bean.metodo(argumento1, argumento2… argumentoN);
```
