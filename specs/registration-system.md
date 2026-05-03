# Spec: Sistema de Inscripción de Participantes

## 1. Objetivo y Contexto
Este módulo gestiona el proceso por el cual un usuario registrado solicita participar en un evento específico. Es fundamental para controlar el flujo de asistentes, manejar los cupos máximos y registrar la intención de participación antes de la fecha límite.

## 2. Historias de Usuario y Criterios de Aceptación
* **HU-01: Inscripción Autónoma**
    * **Como** participante, **quiero** inscribirme a un evento disponible **para** asegurar mi lugar.
    * **Criterio de Aceptación:** El sistema debe validar que el evento tenga cupos disponibles y que la fecha actual sea previa a la fecha límite de inscripción.
* **HU-02: Cancelación de Inscripción**
    * **Como** participante, **quiero** poder darme de baja de un evento **para** liberar el cupo si no puedo asistir.
    * **Criterio de Aceptación:** El estado de la inscripción debe cambiar a 'CANCELLED' y el cupo del evento debe incrementarse.

## 3. Requisitos Funcionales y Reglas de Negocio
* **RF-01:** El sistema debe impedir que un usuario se inscriba dos veces al mismo evento.
* **RN-01:** Solo usuarios con sesión iniciada pueden inscribirse.
* **RN-02:** Si el evento alcanza su `max_capacity`, el sistema debe rechazar nuevas inscripciones.

## 4. Restricciones técnicas específicas
* Debe validar la existencia del `event_id` y `user_id` contra los contratos definidos.
* La actualización del cupo debe ser atómica para evitar errores de concurrencia.

## 5. Modelo de datos
* **Entity: Registration**
    * `id`: UUID
    * `user_id`: UUID (FK)
    * `event_id`: UUID (FK)
    * `registration_date`: DateTime
    * `status`: Enum (PENDING, CONFIRMED, CANCELLED)

## 6. Plan de Tareas
1. Crear tabla `registrations` en la base de datos.
2. Implementar endpoint `POST /api/registrations`.
3. Desarrollar la lógica de validación de cupos y fechas.
4. Crear vista de "Mis Inscripciones" en el frontend.

## 7. Estrategia de Verificación
* **Test:** Intentar inscribir un usuario a un evento que ya alcanzó su cupo máximo. Resultado esperado: Error 400 - "Cupo agotado".
