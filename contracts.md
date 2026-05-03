# Contratos Globales del Proyecto - SGEA

## 1. Restricciones Técnicas

### Backend (API)
- **Lenguaje**: Python >= 3.11
- **Framework**: Django Rest Framework (DRF)
- **Estructura de Respuesta**: Todas las APIs del sistema (incluyendo inscripciones y eventos) deben devolver este formato JSON:
  ```json
  {
    "success": boolean,
    "data": object | array | null,
    "error": string | null,
    "timestamp": "ISO-8601-String"
  }
