# Spec: Sistema de Generación de Certificados

## 1. Objetivo y Contexto
El módulo de generación de certificados tiene como objetivo automatizar la emisión de certificados digitales para los participantes y disertantes de eventos académicos organizados dentro de la plataforma SGEA.

Este módulo se integra directamente con los sistemas de inscripción y acreditación ya definidos. Un usuario únicamente podrá obtener un certificado si cumple con las condiciones establecidas por las reglas de negocio.

La funcionalidad permitirá:
- Generar certificados de asistencia.
- Generar certificados de aprobación.
- Generar certificados para disertantes.
- Descargar certificados en formato PDF.
- Validar autenticidad mediante código único o QR.

El sistema deberá contemplar distintos tipos de eventos académicos:
- Cursos
- Congresos
- Jornadas
- Seminarios
- Charlas
- Talleres

Este módulo será utilizado principalmente por:
- Organizadores
- Participantes
- Disertantes

Dependencias:
- Sistema de inscripción.
- Sistema de acreditación.
- Sistema de roles.
- Gestión de eventos.

---

## 2. Historias de Usuario y Criterios de Aceptación

### HU-01: Generación automática de certificado de asistencia
**Como** participante acreditado,
**quiero** descargar mi certificado de asistencia,
**para** acreditar mi participación en el evento.

#### Criterios de Aceptación
- El sistema debe verificar que el usuario posea una inscripción válida.
- El usuario debe estar acreditado.
- El certificado debe generarse en formato PDF.
- El certificado debe contener:
  - Nombre completo.
  - Nombre del evento.
  - Fecha del evento.
  - Firma digital o institucional.
  - Código de validación.
- El archivo debe poder descargarse desde la plataforma.

---

### HU-02: Generación de certificado de disertante
**Como** organizador,
**quiero** emitir certificados especiales para disertantes,
**para** reconocer formalmente su participación académica.

#### Criterios de Aceptación
- Solo organizadores podrán generar certificados manuales.
- El certificado debe incluir el rol “Disertante”.
- Debe indicar el título de la ponencia o exposición.
- Debe quedar registrado quién emitió el certificado.

---

### HU-03: Validación de autenticidad
**Como** institución receptora,
**quiero** validar la autenticidad de un certificado,
**para** evitar certificados falsificados.

#### Criterios de Aceptación
- Cada certificado debe poseer un código único.
- El sistema debe ofrecer una URL pública de validación.
- El estado del certificado debe indicar:
  - Válido.
  - Revocado.
  - Expirado.

---

## 3. Requisitos Funcionales y Reglas de Negocio

### Requisitos Funcionales

#### RF-01
El sistema debe generar certificados digitales en formato PDF.

#### RF-02
El sistema debe almacenar un historial de certificados emitidos.

#### RF-03
El sistema debe permitir descargar certificados desde el perfil del usuario.

#### RF-04
El sistema debe generar automáticamente un código QR de validación.

#### RF-05
El sistema debe permitir a los organizadores revocar certificados emitidos.

#### RF-06
El sistema debe soportar múltiples plantillas de certificados.

#### RF-07
El sistema debe enviar notificaciones por correo cuando un certificado esté disponible.

---

### Reglas de Negocio

#### RN-01
Solo los usuarios acreditados podrán recibir certificados de asistencia.

#### RN-02
Los certificados de aprobación requerirán una calificación mínima configurada por el evento.

#### RN-03
Un certificado no puede emitirse dos veces para la misma inscripción y tipo.

#### RN-04
Solo usuarios con rol ORGANIZADOR podrán revocar certificados.

#### RN-05
Los certificados revocados deberán conservarse en la base de datos por motivos de auditoría.

#### RN-06
La validación pública no deberá exponer datos sensibles del usuario.

---

## 4. Restricciones Técnicas Específicas de este Módulo

- El backend deberá utilizar Django Rest Framework.
- Los certificados deberán generarse utilizando librerías compatibles con Python 3.11.
- El PDF deberá ser responsive para impresión A4.
- Los archivos PDF deberán almacenarse en un servicio de almacenamiento persistente.
- El código QR deberá redirigir a una ruta pública de validación.
- El endpoint de descarga deberá requerir autenticación JWT.
- Las respuestas API deberán respetar el contrato JSON definido en contracts.md.

---

## 5. Modelo de Datos de este Módulo

### Entity: Certificate

| Campo | Tipo | Descripción |
|---|---|---|
| id | UUID | Identificador único |
| registration_id | UUID (FK) | Relación con inscripción |
| certificate_type | Enum | ASSISTANCE / APPROVAL / SPEAKER |
| generated_at | DateTime | Fecha de generación |
| generated_by | UUID | Usuario que emitió |
| validation_code | String | Código único |
| pdf_url | String | Ruta del archivo |
| status | Enum | VALID / REVOKED |

---

### Entity: CertificateTemplate

| Campo | Tipo | Descripción |
|---|---|---|
| id | UUID | Identificador |
| name | String | Nombre de plantilla |
| event_type | String | Tipo de evento |
| background_image | String | Fondo del certificado |
| active | Boolean | Estado |

---

## 6. Plan de Tareas

1. Crear entidad Certificate.
2. Crear migraciones de base de datos.
3. Implementar lógica de validación de acreditación.
4. Implementar generación de PDFs.
5. Integrar generación de QR.
6. Implementar endpoint:
   - POST /api/certificates/generate
7. Implementar endpoint:
   - GET /api/certificates/{id}/download
8. Implementar endpoint público:
   - GET /api/certificates/validate/{code}
9. Diseñar interfaz “Mis Certificados”.
10. Agregar historial de certificados emitidos.
11. Implementar revocación de certificados.
12. Crear pruebas unitarias e integración.

---

## 7. Estrategia de Verificación

### Test Unitarios
- Validar generación correcta de código único.
- Verificar que no se generen certificados duplicados.
- Verificar reglas de acreditación.

### Test de Integración
- Simular descarga de certificado.
- Verificar acceso autenticado.
- Validar generación correcta del PDF.

### Test Manuales
- Escanear QR desde dispositivo móvil.
- Verificar visualización del certificado.
- Comprobar impresión en hoja A4.

### Casos Negativos
- Intentar generar certificado sin acreditación.
- Intentar descargar certificado revocado.
- Intentar acceder con usuario no autenticado.

---
---