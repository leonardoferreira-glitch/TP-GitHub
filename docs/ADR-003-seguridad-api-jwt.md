# ADR-003: Implementación de Autenticación mediante JWT
**Estado:** Aceptado
**Fecha:** 2026-06-16
**Decisores:** Leonardo Ferreira
**Contexto:** Necesitamos un sistema de autenticación stateless para nuestra arquitectura de microservicios o API backend (Django).
**Decisión:** Utilizar JSON Web Tokens (JWT) para la gestión de sesiones de usuarios.
**Alternativas consideradas:** Sesiones tradicionales con cookies (descartado por falta de escalabilidad horizontal).
**Consecuencias:** Mejora la escalabilidad, pero requiere manejo seguro del almacenamiento del token en el cliente (localStorage/HttpOnly cookies).
**Métrica de éxito:** Reducción en la latencia de validación de usuarios en cada request.
