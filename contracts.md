# TechStore - Contratos del Proyecto

## 1. Restricciones Tecnológicas Obligatorias

### Backend
- **Framework Obligatorio**: Django + Django Rest Framework (DRF)
- **Python Version**: >= 3.10
- **Base de Datos**: PostgreSQL >= 14
- **Package Manager**: pip + requirements.txt

### Frontend
- **Framework Obligatorio**: Next.js 14+
- **Node Version**: >= 18.x
- **Package Manager**: npm o pnpm
- **Styling Framework**: Tailwind CSS

### DevOps
- **Versionado**: Git (GitHub)
- **CI/CD**: GitHub Actions
- **Containerización**: Docker
- **Ambiente Local**: Docker Compose

## 2. Restricciones de Datos y Seguridad

### Datos Sensibles - PROHIBIDO
- ❌ NO almacenar contraseñas en texto plano (siempre hasheadas con bcrypt)
- ❌ NO guardar tokens JWT completos en BD
- ❌ NO guardar datos de tarjeta de crédito (PCI compliance)
- ❌ NO exponer secrets en repositorio (usar .env)

### Datos Sensibles - OBLIGATORIO
- ✅ Usar variables de entorno para configuración sensible
- ✅ Encriptar campos sensibles en BD (ejemplo: últimos dígitos de tarjeta)
- ✅ Implementar rate limiting en endpoints de autenticación
- ✅ Auditar accesos a datos sensibles

## 3. Restricciones Funcionales

### Autenticación
- Los tokens JWT deben expirar en máximo 1 hora
- Los refresh tokens deben expirar en máximo 7 días
- Máximo 5 intentos de login fallidos antes de bloqueo temporal
- Las sesiones deben ser revocables

### Carrito y Órdenes
- El carrito no puede contener más de 100 items
- No se permite crear órdenes sin al menos 1 producto
- El stock no puede ser negativo
- Los precios son inmutables una vez creada la orden

### Pagos
- Solo se aceptan métodos de pago validados (Stripe, MercadoPago)
- Las transacciones deben registrarse con timestamp
- Implementar idempotencia en endpoints de pago (evitar duplicados)
- Guardar log de intentos fallidos de pago

### Inventario
- Alerta de bajo stock cuando cantidad <= 10 unidades
- No se permite actualizar stock a valores negativos
- Todo movimiento de inventario debe registrarse en auditoría

## 4. Estándares de Código y Estructura

### Backend (Django)
```
techstore/
├── apps/
│   ├── users/
│   │   ├── models.py
│   │   ├── views.py
│   │   ├── serializers.py
│   │   ├── urls.py
│   │   └── tests.py
│   ├── products/
│   ├── orders/
│   └── payments/
├── core/
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── requirements.txt
├── manage.py
└── .env.example
```

### Frontend (Next.js)
```
frontend/
├── app/
│   ├── layout.tsx
│   ├── page.tsx
│   ├── auth/
│   ├── products/
│   ├── cart/
│   └── orders/
├── components/
│   ├── common/
│   ├── forms/
│   └── layouts/
├── lib/
│   ├── api.ts
│   ├── auth.ts
│   └── utils.ts
├── types/
├── styles/
└── package.json
```

### Convenciones de Nombres
- **Models/Entidades**: CamelCase (User, Product, Order)
- **Funciones**: snake_case (get_user_by_id, calculate_total)
- **Constantes**: UPPER_SNAKE_CASE (MAX_RETRY_ATTEMPTS)
- **Archivos**: snake_case (user_serializer.py, UserForm.tsx)
- **Rutas API**: kebab-case (/api/users, /api/product-categories)

## 5. Patrones y Arquitectura

### APIs REST
- **GET** - Obtener recurso(s), idempotente
- **POST** - Crear nuevo recurso
- **PUT** - Actualizar recurso completo
- **PATCH** - Actualizar parcialmente
- **DELETE** - Eliminar recurso

### Respuestas API
```json
{
  "success": true,
  "data": { /* datos */ },
  "error": null,
  "timestamp": "2024-01-15T10:30:00Z"
}
```

### Códigos HTTP Esperados
- 200: OK
- 201: Created
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 409: Conflict
- 500: Internal Server Error

## 6. Testing y Calidad

### Cobertura Mínima
- Backend: >= 80% cobertura
- Frontend: >= 60% cobertura
- Todos los tests deben pasar antes de merge

### Herramientas
- **Backend**: pytest + pytest-django
- **Frontend**: Jest + React Testing Library
- **Linting**: flake8 + black (backend), ESLint (frontend)
- **Type Checking**: mypy (backend), TypeScript (frontend)

## 7. Documentación

### Obligatorio
- README.md en raíz (instrucciones de setup)
- Documentación de API (Swagger/OpenAPI)
- Docs/ARCHITECTURE.md (decisiones técnicas)
- Cada spec debe tener su propio archivo .md

### Opcional pero Recomendado
- Diagramas de flujo (Mermaid)
- Diagramas de BD (ER)
- Guía de contribución (CONTRIBUTING.md)

## 8. Versionado y Commits

### Rama Principal
- `main`: código en producción (protegida, requiere PR)
- `develop`: rama de integración
- `feature/nombre`: ramas de features
- `bugfix/nombre`: ramas de bugfixes

### Commits
- Usar convención Conventional Commits
- Ejemplos:
  - `feat: add user authentication`
  - `fix: resolve cart calculation bug`
  - `docs: update API documentation`
  - `test: add tests for order creation`

## 9. Definiciones de Completitud

Una feature está completa cuando:
- ✅ Spec revisada y aprobada
- ✅ Código implementado según spec
- ✅ Tests escritos y pasando
- ✅ Documentación actualizada
- ✅ Code review aprobado
- ✅ Deployable a staging

## 10. Roles y Responsabilidades

### Integrante 1
- **Responsable de**: Backend + Base de Datos + Autenticación
- **Specs a desarrollar**: User Management, Orders & Payments, Inventory

### Integrante 2
- **Responsable de**: Frontend + Integración API
- **Specs a desarrollar**: Product Catalog, Shopping Cart, Shipping & Logistics

### Ambos
- Code reviews cruzados
- Testing conjunto
- Documentación compartida
