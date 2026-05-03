# Spec: Sistema de Inscripción de Participantes

## 1. Objetivo y Contexto
Este módulo gestiona el proceso por el cual un usuario registrado solicita participar en un evento específico. Es fundamental para controlar el flujo de asistentes, manejar los cupos máximos y registrar la intención de participación antes del evento.

## 2. Historias de Usuario y Criterios de Aceptación
* **HU-01: Inscripción Autónoma**
    * **Como** participante, **quiero** inscribirme a un evento disponible **para** asegurar mi lugar.
    * **Criterio de Aceptación:** El sistema debe validar que el evento tenga cupos disponibles y que la fecha actual sea previa a la fecha límite de inscripción.
* **HU-02: Cancelación de Inscripción**
    * **Como** participante, **quiero** poder darme de baja de un evento **para** liberar el cupo si no puedo asistir.
    * **Criterio de Aceptación:** El estado de la inscripción debe cambiar a 'CANCELLED' y el cupo del evento debe incrementarse automáticamente.

## 3. Requisitos Funcionales y Reglas de Negocio
* **RF-01:** El sistema debe impedir que un usuario se inscriba dos veces al mismo evento.
* **RN-01:** Solo usuarios con rol 'PARTICIPANTE' o 'DISERTANTE' pueden inscribirse (los organizadores están implícitos).
* **RN-02:** Si el evento alcanza su `max_capacity`, el botón de inscripción debe deshabilitarse o mostrar un mensaje de "Cupo Lleno".

## 4. Restricciones técnicas específicas
* Debe consumir el contrato de `event-management` para verificar la existencia del evento.
* Las validaciones de cupo deben ser atómicas para evitar sobre-inscripción en accesos concurrentes.

## 5. Modelo de datos
* **Entity: Registration**
    * `id`: UUID
    * `user_id`: UUID (Relación con Auth)
    * `event_id`: UUID (Relación con Event)
    * `registration_date`: DateTime
    * `status`: Enum (PENDING, CONFIRMED, CANCELLED)

## 6. Plan de Tareas
1. Crear la tabla/colección de `registrations`.
2. Implementar lógica de validación de fechas y cupos.
3. Desarrollar el endpoint `POST /registrations`.
4. Crear la vista de "Mis Inscripciones" para el perfil del usuario.

## 7. Estrategia de Verificación
* **Prueba Unitaria:** Validar que un usuario no pueda inscribirse si la fecha actual es posterior a la `deadline` del evento.
* **Prueba de Integración:** Verificar que al realizar una inscripción exitosa, el contador de cupos del evento se actualice correctamente.
