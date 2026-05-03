# Spec: Sistema de Inscripción de Participantes

## 1. Objetivo y Contexto
Este módulo gestiona el flujo de inscripción de usuarios a eventos académicos. Su propósito es controlar la demanda, validar la disponibilidad de cupos y asegurar que las inscripciones se realicen dentro de los plazos estipulados por los organizadores.

## 2. Historias de Usuario y Criterios de Aceptación
* **HU-01: Inscripción a Evento**
    * **Como** participante registrado, **quiero** inscribirme a un evento de mi interés **para** asegurar mi lugar y recibir información relacionada.
    * **Criterio de Aceptación:** El sistema debe verificar que el usuario no esté ya inscrito y que la fecha actual esté dentro del rango permitido.
* **HU-02: Control de Capacidad**
    * **Como** organizador, **quiero** que el sistema bloquee inscripciones automáticamente cuando se alcance el cupo máximo **para** evitar problemas de logística en el recinto.

## 3. Requisitos Funcionales y Reglas de Negocio
* **RF-01:** Permitir la inscripción autónoma desde la vista pública del evento.
* **RF-02:** Permitir al organizador inscribir manualmente a participantes (ej: por ventanilla).
* **RN-01:** No se permiten inscripciones si `current_date > registration_deadline`.
* **RN-02:** Si un evento tiene `max_capacity` definido, el sistema debe arrojar error al intentar inscribir al usuario `max_capacity + 1`.

## 4. Restricciones técnicas específicas
* Sincronización con el módulo `event-management` para obtener metadatos del evento.
* Uso de transacciones de base de datos para evitar "overbooking" en inscripciones simultáneas.

## 5. Modelo de datos
* **Table: registrations**
    * `id`: UUID (PK)
    * `event_id`: UUID (FK)
    * `user_id`: UUID (FK)
    * `status`: Enum (registered, waitlisted, cancelled)
    * `created_at`: Timestamp

## 6. Plan de Tareas
1. Definir endpoint `POST /api/events/{id}/register`.
2. Implementar lógica de validación de cupos y fechas.
3. Crear vista de "Mis Inscripciones" en el frontend.
4. Notificar vía mail la confirmación de la inscripción.

## 7. Estrategia de Verificación
* **Prueba de Carga:** Simular 50 usuarios inscribiéndose al mismo tiempo a un evento con 10 cupos.
* **Test de Regla de Negocio:** Intentar inscripción en un evento con fecha de cierre vencida.
