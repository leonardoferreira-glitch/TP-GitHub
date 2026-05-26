# Spec: Sistema de Encuestas de Satisfacción Post-Evento

## 1. Objetivo y Contexto
Este módulo permite recopilar opiniones, valoraciones y sugerencias de los participantes luego de finalizar un evento académico.

El objetivo principal es mejorar la calidad de futuros eventos mediante métricas y análisis de satisfacción.

La funcionalidad permitirá:
- Crear encuestas personalizadas.
- Responder encuestas desde cualquier dispositivo.
- Obtener estadísticas automáticas.
- Generar reportes de satisfacción.
- Identificar oportunidades de mejora.

El módulo será utilizado por:
- Organizadores.
- Participantes.
- Administradores.

Dependencias:
- Gestión de eventos.
- Sistema de inscripción.
- Sistema de roles.

---

## 2. Historias de Usuario y Criterios de Aceptación

### HU-01: Completar encuesta
**Como** participante,
**quiero** responder una encuesta al finalizar el evento,
**para** expresar mi nivel de satisfacción.

#### Criterios de Aceptación
- Solo participantes inscritos podrán responder.
- La encuesta deberá estar disponible luego de finalizar el evento.
- El sistema debe permitir preguntas:
  - Escala numérica.
  - Opción múltiple.
  - Texto libre.
- El sistema debe guardar respuestas automáticamente.

---

### HU 2: Inscripción Express a Eventos Académicos
**Como** Participante del evento  
**Quiero** Registrar mis datos en el formulario web  
**Para** Asegurar mi cupo y recibir el código QR de acreditación.

#### Criterios de Aceptación Originales:
* El sistema debe validar que el DNI no esté duplicado.
* El sistema debe restar 1 al cupo disponible.

#### [ENRIQUECIMIENTO TP4] Criterios de Aceptación de Seguridad (Controles OWASP):
1. **Mitigación de Inyección SQL (OWASP A03:2021):** El backend de Django Rest Framework deberá procesar la consulta utilizando exclusivamente el ORM parametrizado. Queda estrictamente prohibido el uso de strings crudos (`RawSQL`).
2. **Defensa de Concurrencia y Control de Flujo:** Para evitar la explotación de condiciones de carrera al agotar cupos, la verificación del remanente debe usar un bloqueo pesimista en base de datos (`select_for_update()`).
3. **Validación de Entradas Estricta (OWASP A04:2021):** El campo `Email` y `DNI` deben ser sanitizados tanto en el cliente (Next.js) como en los Serializers del servidor, rechazando caracteres especiales sospechosos (`<`, `>`, `'`, `--`) para prevenir vectores de ataque XSS e inyecciones.

### HU-03: Configuración de encuestas
**Como** organizador,
**quiero** configurar preguntas personalizadas,
**para** adaptar la encuesta al tipo de evento.

#### Criterios de Aceptación
- El organizador podrá agregar, editar y eliminar preguntas.
- El sistema deberá validar longitud máxima.
- No podrán modificarse encuestas ya respondidas.

---

## 3. Requisitos Funcionales y Reglas de Negocio

### Requisitos Funcionales

#### RF-01
El sistema debe permitir crear encuestas asociadas a eventos.

#### RF-02
El sistema debe permitir preguntas de distintos tipos.

#### RF-03
El sistema debe calcular estadísticas automáticas.

#### RF-04
El sistema debe generar reportes exportables.

#### RF-05
El sistema debe permitir respuestas anónimas opcionalmente.

#### RF-06
El sistema debe permitir habilitar o deshabilitar encuestas.

#### RF-07
El sistema debe enviar recordatorios automáticos.

---

### Reglas de Negocio

#### RN-01
Solo usuarios inscriptos podrán responder encuestas.

#### RN-02
Cada participante podrá responder una sola vez.

#### RN-03
Las encuestas estarán disponibles únicamente después de finalizar el evento.

#### RN-04
Los organizadores no podrán responder encuestas de sus propios eventos.

#### RN-05
Las respuestas anónimas no deberán almacenar datos personales.

#### RN-06
Las preguntas obligatorias deberán responderse antes del envío.

---

## 4. Restricciones Técnicas Específicas de este Módulo

- El frontend deberá desarrollarse con Next.js 14.
- Los gráficos deberán ser responsivos.
- La API deberá soportar paginación.
- El sistema deberá soportar múltiples encuestas concurrentes.
- Todas las fechas deberán manejar timezone UTC.
- Las respuestas deberán persistirse en PostgreSQL 16.
- Las APIs deberán respetar los contratos globales.

---

## 5. Modelo de Datos de este Módulo

### Entity: Survey

| Campo | Tipo | Descripción |
|---|---|---|
| id | UUID | Identificador |
| event_id | UUID (FK) | Evento asociado |
| title | String | Título |
| description | Text | Descripción |
| active | Boolean | Estado |
| created_at | DateTime | Fecha creación |

---

### Entity: SurveyQuestion

| Campo | Tipo | Descripción |
|---|---|---|
| id | UUID | Identificador |
| survey_id | UUID (FK) | Encuesta |
| question_text | Text | Pregunta |
| question_type | Enum | SCALE / MULTIPLE / TEXT |
| required | Boolean | Obligatoria |
| order | Integer | Orden |

---

### Entity: SurveyResponse

| Campo | Tipo | Descripción |
|---|---|---|
| id | UUID | Identificador |
| survey_id | UUID (FK) | Encuesta |
| user_id | UUID (FK) | Usuario |
| submitted_at | DateTime | Fecha respuesta |
| anonymous | Boolean | Respuesta anónima |

---

### Entity: SurveyAnswer

| Campo | Tipo | Descripción |
|---|---|---|
| id | UUID | Identificador |
| response_id | UUID (FK) | Respuesta |
| question_id | UUID (FK) | Pregunta |
| answer_value | Text | Respuesta |

---

## 6. Plan de Tareas

1. Crear tablas de encuestas y respuestas.
2. Implementar CRUD de encuestas.
3. Implementar CRUD de preguntas.
4. Crear endpoint:
   - POST /api/surveys
5. Crear endpoint:
   - POST /api/surveys/{id}/responses
6. Implementar validación de respuestas únicas.
7. Implementar dashboard estadístico.
8. Crear gráficos de satisfacción.
9. Implementar exportación PDF/CSV.
10. Implementar envío automático de recordatorios.
11. Diseñar interfaz responsive.
12. Crear pruebas unitarias e integración.

---

## 7. Estrategia de Verificación

### Test Unitarios
- Validar preguntas obligatorias.
- Validar unicidad de respuestas.
- Verificar cálculos estadísticos.

### Test de Integración
- Simular respuestas concurrentes.
- Verificar persistencia correcta.
- Validar endpoints REST.

### Test Manuales
- Completar encuesta desde móvil.
- Verificar gráficos estadísticos.
- Exportar reportes.

### Casos Negativos
- Intentar responder dos veces.
- Intentar responder encuesta inactiva.
- Intentar acceder sin autenticación.

---