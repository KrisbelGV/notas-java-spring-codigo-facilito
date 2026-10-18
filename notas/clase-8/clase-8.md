# Teoría de Arquitectura de Software con enfoque a Microservicios

## Arquitectura de software

Compuesto por el estilo, características, decisiones arquitectónicas, y componentes lógicos.

## Características

- **Escalabilidad:** Capacidad de un sistema de adaptarse a un nuevo nivel de demanda.
  - Vertical: Aumentar los recursos de un servidor existente.
  - Horizontal: Añadir más servidores que trabajen el paralelo.
- **Disponibilidad:** Porcentaje del tiempo en que un servicio está disponible u de solicitudes validas atendidas exitosamente.
- **Performance:** Cómo se comporta el sistema bajo distintas circunstancias de trabajo.
- **Fiabilidad:** El sistema cumple consistentemente con la función esperada.
- **Elasticidad:** Posibilidad de adquirir y liberar recursos según se necesiten.
- **Mantenibilidad:** Facilidad al aplicar cambios y modificaciones.
- **Seguridad:** Restricciones que aseguren los datos y la comunicación entre sistemas.

## Conceptos importantes

- **Latencia:** Tiempo de respuesta para una operación determinada.
- **Rendimiento:** Volumen de operaciones procesados con éxito en un periodo de tiempo. Suele medirse en TPS (Transacciones Por Segundo).

## Teorema CAP

Privilegio en propiedades del sistema.

- Consistencia.
- Disponibilidad.
- Tolerancia a partición.

## Estilos de arquitectura

| Estilo | Ventajas | Desventajas |
|---|---|---|
| Monolito no estructurado | | Difícil de escalar<br>Baja o nula tolerancia a fallos<br>Elasticidad baja o nula<br>Difícil de mantener |
| Arquitectura por capas. | Mayor simplicidad<br>Costo aceptable<br>Puede ser muy confiable<br>Fácil en cuanto a testing | Separación en base a características técnicas, no de dominio<br>Agilidad<br>Escalabilidad |
| Monolito modular | Simplicidad<br>Costo<br>Tiempo de lanzamiento<br>Testeabilidad<br>Separación en base a dominio | Escalabilidad<br>Elasticidad<br>Requiere gobernanza |
| Microkernel | Simplicidad<br>Costo<br>Testeabilidad<br>Evolucionabilidad | Diseño complejo<br>Gobernanza<br>Escalabilidad |
| Arquitectura basada en eventos | Abstracción<br>Interoperabilidad heterogénea<br>Integración entre sistemas<br>Orquestación a la medida | Simplicidad<br>Desplegabilidad<br>Agilidad<br>Costo |
| Microservicios | Agilidad<br>Desplegabilidad<br>Escalabilidad<br>Tolerancia a fallos<br>Evolucionabilidad<br>Partición de dominio | Simplicidad<br>Costo |

## Características esenciales de los microservicios

- Granularidad.
- Aislamiento de datos.
- API layer.
- Monitoring.
- Patrones de resilencia.
  - Circuit-breaker.
  - Bulkhead.
  - Sidecar.
  - Rate limiting, retry, timeout.

## Disponibilidad y "nueves"

El uptime es el porcentaje de tiempo que está un sistema activo y operativo.

| Porcentaje | Tiempo (downtime) |
|---|---|
| 99% | 3.65 días |
| 99.9% | 8.77 horas |
| 99.99% | 52.6 minutos |
| 99.999% | 5.26 minutos |
| 99.9999% | 31.5 segundos |
| 99.99999% | 3.16 segundos |

## Métricas y observabilidad

Indispensable para el escalado en la arquitectura.

## Dimensiones de escalado

- **Trafico:** Peticiones, transacciones, eventos, conexiones.
- **Datos:** Llecturas/escrituras por segundo, indexado, fragmentación (particionamiento), objetos BLOB.
- **Concurrencia:** Procesos en paralelo o compitiendo por recursos.
- **Geográfica:** Latencia, distribución, regionalidad.
- **Operativa:** Más servicios, despliegues, orquestación, observación.
- **Organizacional:** Múltiples equipos trabajando en el mismo ecosistema.