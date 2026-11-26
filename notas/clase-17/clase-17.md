# Mensajería con Spring Boot

## Síncrono vs asíncrono

### Síncrono

Un servicio espera la respuesta. Si el segundo cae, el primero también falla.

### Asíncrono

En su lugar, deja un mensaje y sigue. Este es leído a su propio ritmo.

## ¿Por qué usar mensajería?

- **Desacoplamiento:** El productor no conoce a los consumidores.
- **Resiliencia:** Si un consumidor cae, el mensaje espera.
- **Escalabilidad:** Agrega consumidores sin tocar al productor.
- **Absorbe picos:** Un buzón amortigua el tráfico (load leveling).

## Tres modelos de mensajería

- **Cola:** Cada mensaje lo recibe un consumidor y se elimina al leerse.
- **Publicación / suscripción:** Cada suscriptor recibe una copia. Todos reciben todo.
- **Log de eventos:** Los eventos se guardan y se pueden releer, se conservan.

## Garantías de entrega

- **A lo mucho una vez:** Puede perderse, nunca se duplica.
- **Al menos una vez:** No se pierde, puede duplicarse.
- **Exactamente una vez:** Efecto único (costoso, con límites).

> Regla práctica: Diseña para "al menos una vez" y haz tus consumidores IDEMPOTENTES.

## Patrones de mensajería esenciales

- **Competing consumers:** Varios trabajadores comparten una cola.
- **Pub/Sub:** Un evento, muchos interesados.
- **Dead Letter:** Mensajes imposibles van a revisión.
- **Retry con backoff:** Reintenta esperando cada vez más.
- **Consumidor idempotente:** Repetir no cambia el resultado.
- **Transactional Outbox:** Guarda el evento junto a tus datos.

## Apache Kafka: un log distribuido y duradero

### Flujo

1. **Productores:** Escriben eventos al final.
2. **Topic**
   - **P0:** 0, 1, 2, 3, 4, 5, 6
   - **P1:** 0, 1, 2, 3, 4, 5, 6
   - **P2:** 0, 1, 2, 3, 4, 5, 6
3. **Consumidores:** Leen a su ritmo con un offset.

### Características

- Duradero.
- Ordenado por partición.
- Reproducible (replay).
- Escala por particiones.

## Topic, partición y offset

- **Topic:** Nombre lógico del flujo de eventos.
- **Partición:** Un log ordenado; permite paralelismo.
- **Offset:** Posición de un evento en su partición.
- **Clave (key):** Hash(key) -> partición: mismo cliente, mismo orden.

> El orden se garantiza dentro de una misma partición, pero no entre distintas.

## Pub/Sub sobre un commit log

### Flujo

1. **Productor** (ej. pedidos).
2. **Topic** (ej. orders.created).
   - 0, 1, 2, 3, 4
   - 0, 1, 2, 3, 4
   - 0, 1, 2, 3, 4
3. **Grupos.**
   - Ej. **Notifications:** Avisar al cliente, offset 4.
   - Ej. **Analytics:** Contar ventas, offset 2.
   - Ej. **Audit:** Lee TODO el historial, offset 0.

### Características

- **Publicación/suscripción:** Un evento llega a TODOS los grupos interesados.
- **Un grupo nuevo puede leer desde el principio:** El log no se borra al leerse.
- **Dentro de un grupo, las particiones se reparten.**

## Consumer groups: paralelismo

1. Cada partición, un solo consumidor del grupo.
2. Más consumidores -> rebalance.
3. Máximo útil = nº de particiones.

## Durabilidad: réplicas, retención y replay

- **Replicación:** Cada partición tiene líder y réplicas en otros brokers.
- **Retención:** Se guarda por tiempo o tamaño, no hasta que alguien lo lea.
- **Compactación:** Conserva el último valor por clave (changelog).
- **Replay:** Reinicia el offset y vuelve a procesar el historial.

## Arquitectura: brokers y KRaft

Un líder por partición atiende lecturas y escrituras, mientras las réplicas seguidoras toman el relevo si el líder cae.

> Esperar la confirmación de las réplicas (evita duplicados al reintentar): Producer: `acks=all` + `enable.idempotence`

## Más allá del bróker: el ecosistema

- **Kafka Connect:** Integra bases de datos y sistemas sin código.
- **Kafka Streams:** Procesa y agrega flujos en tu app Java.
- **Schema Registry:** Contratos y evolución de los mensajes.
- **Share groups:** Consumo tipo cola con ack por mensaje (preview).

## RabbitMQ: exchanges, colas y routing

1. **Productor.**
2. **Exchange** (decide el destino).
3. **Bindings / reglas de enrutamiento** (criterio que usa el exchange para decidir la cola).
   - Ej. `destino = email` -> `Emails.send`
   - Ej. `tipo = auditoria` -> `Audit.log`
   - Ej. `estado = fallido` -> `Emails.dead`
4. **Colas** (destino final donde se almacenan los mensajes).
   - Ej. `Emails.send`
   - Ej. `Audit.log`
   - Ej. `Emails.dead`

## Kafka vs RabbitMQ

| | Apache Kafka - log | RabbitMQ - cola |
|---|---|---|
| Modelo | Topic con particiones (log) | Exchange -> colas (y streams) |
| Mensaje leído | Se conserva; offset por grupo | Se borra al hacer ack |
| Replay | Sí, nativo | Solo en streams |
| Routing | En el productor (key) | En el broker (exchanges) |
| Orden | Por partición | Por cola, con matices |
| Escala | Particiones y grupos | Colas, cluster, super streams |
| Ideal para | Event streaming, analítica | Tareas, RPC, routing flexible |

## Cuál elegir

### RabbitMQ

- Tareas en segundo plano con reintentos y prioridades.
- Routing flexible (direct, topic, fanout).
- Patrón petición-respuesta o TTL por mensaje.
- Pocos mensajes, baja latencia, colas aisladas.

### Kafka

- Historial reproducible de eventos.
- Muchos consumidores independientes del mismo flujo.
- Volumen muy alto (telemetría, clics, IoT).
- Procesamiento de streams y analítica en tiempo real.

## Spring y la mensajería: el mapa

- **Spring for Apache Kafka.**
  - `spring-boot-starter-kafka`
  - `KafkaTemplate`
  - `@KafkaListener`
- **Spring AMQP (RabbitMQ).**
  - `spring-boot-starter-amqp`
  - `RabbitTemplate`
  - `@RabbitListener`
- **Spring Cloud Stream.**
  - Abstracción por funciones: el binder se elige por configuración.
- **Spring Integration - JMS - Pulsar**
  - Patrones EIP, estándar JMS y Apache Pulsar.

## Errores: reintentos y Dead Letter

1. **Mensaje:** Llega al consumidor.
2. **Falla:** Lanza una excepción.
3. **Reintentos:** 1s - 1s (backoff).
4. **Dead Letter:** created.DLT.

## Doble escritura y Outbox

Usar Transactional Outbox en vez de guardar y luego publicar.

1. La BD lleva el registro.
2. Se añade a la tabla outbox (misma transacción).
3. Un proceso lo lee y publica al bróker.