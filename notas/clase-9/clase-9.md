# Lineamientos de proyecto final

## Proyectos propuestos

- API REST adopción de mascotas.
- Proyecto libre (que cumpla con los criterios).

## ENDPOINTS

- Registro de mascotas: `POST /api/mascotas`
- Consulta de mascotas disponibles para adopción: `GET /api/mascotas`
- Actualización de datos de una mascota: `PUT /api/mascotas/{id}`
- Eliminación de una mascota: `DELETE /api/mascotas/{id}`
- Registro de usuarios: `POST /api/usuarios`
- Adopción de mascota: `POST/api/adopciones`

## Modelo de datos (entities)

### Mascota

- **ID (long):** Identificador único, incremental.
- **Nombre (String):** Nombre de la mascota.
- **ID Tipo Mascota (ID Tipo Mascota):** Tipo de mascota.
- **Edad (Integer):** Edad de la mascota en años.
- **Disponible (Boolean):** Estado de disponibilidad para adopción.

### Tipo mascota

- **ID (Long):** Identificador único, incremental.
- **Nombre (String):** Nombre del tipo de mascota.

### Usuario

- **ID (Long):** Identificador único, incremental.
- **Nombre (String):** Nombre del usuario.
- **Correo Electrónico (String):** Correo de contacto.
- **Teléfono (String):** Número de contacto.

### Adopción

- **ID (Long):** Identificador único, incremental.
- **Mascota (ID Mascota):** Referencia a la mascota adoptada (relación many-to-one).
- **ID usuario (ID Usuario):** Referencia al usuario adoptante (relación many-to-one).
- **Fecha Adopción (Date):** Fecha en la que sea realiza la adopción.

## Criterios de evaluación

1. Correcta implementación de los endpoints REST.
2. Conexión exitosa y funcional con la base de datos (H2).
3. Unit test para cada endpoint.
4. Uso adecuado de los patrones y principios de Spring Boot.
5. Endpoints protegidos con Spring Security (Basic auth).
6. Manejo correcto de flujos de exepción y códigos de respuesta.
7. Calidad del código (buena organización, nomenclatura adecuada, uso de comentarios cuando sea necesario).
8. Documentación OpenAPI API y ejemplos de request para usar el API.
9. Endpoints para monitoreo activos (Actuator).

## Puntos extra

- Validación de inputs.
- CRUD de Usuario.
- Hyperlink (HATEOAS).
- Funcional con alguna base de datos diferente a H2 (por ejemplo Supabase).

## Consideraciones

1. Diseñar correctamente las capas.
2. Evitar combinar lógica y flujo.
3. EL modelo de datos es sugerido, puede ser modificado o extendido.

## Como realizar la entrega

Llenar el formulario con:

- Título.
- Descripción.
- Integrantes de equipo.
- Link al repositorio público.
- Links adicionales.
- Documentación en PDF del proyecto.
- Video de YouTube o Drive.