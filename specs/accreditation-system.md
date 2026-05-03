# Spec: Sistema de Acreditación de Participantes

## 1. Objetivo y Contexto
Este módulo permite registrar la asistencia efectiva de los participantes el día del evento. Es el paso previo necesario para la posterior generación de certificados de asistencia.

## 2. Historias de Usuario y Criterios de Aceptación
* **HU-01: Acreditación por Organizador**
    * **Como** organizador, **quiero** marcar a un participante como "Presente" **para** llevar el control de asistencia.
    * **Criterio de Aceptación:** El sistema debe permitir buscar al usuario por DNI o Email y cambiar su estado de inscripción a "ACCREDITED".
* **HU-02: Consulta de Asistencia**
    * **Como** participante, **quiero** ver si ya fui acreditado **para** confirmar que recibiré mi certificado.

## 3. Requisitos Funcionales y Reglas de Negocio
* **RF-01:** Interfaz de búsqueda rápida para el personal de recepción del evento.
* **RN-01:** Solo los usuarios con rol 'ORGANIZADOR' pueden realizar acreditaciones.
* **RN-02:** Un usuario solo puede ser acreditado si tiene una inscripción previa en estado 'CONFIRMED'.

## 4. Restricciones técnicas específicas
* La interfaz debe estar optimizada para dispositivos móviles (uso de tablets en la entrada del evento).
* Opcional: Generación de un código QR por inscripción para escaneo rápido.

## 5. Modelo de datos
* **Extend: Registration** (Se agrega campo al modelo anterior o se crea tabla de asistencia)
    * `is_accredited`: Boolean
    * `accreditation_timestamp`: DateTime
    * `accredited_by`: UUID (ID del organizador que realizó la acción)

## 6. Plan de Tareas
1. Agregar campos de acreditación al modelo de datos.
2. Implementar buscador de participantes por nombre/DNI dentro de un evento.
3. Crear endpoint `PATCH /registrations/{id}/accredit`.
4. Diseñar la vista de lista de asistencia para el organizador.

## 7. Estrategia de Verificación
* **Simulación:** Intentar acreditar a un usuario que no está inscrito en el evento y verificar que el sistema lance un error descriptivo.
* **Manual:** Verificar que el `timestamp` de acreditación coincida con la hora de la acción.
