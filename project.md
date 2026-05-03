
# Proyecto: Sistema de Gestión de Eventos Académicos (SGEA)

## 1. Descripción General
Este proyecto consiste en una plataforma web integral para la organización, gestión y participación en eventos de carácter académico (congresos, jornadas, cursos y charlas). El sistema permite a los organizadores gestionar la logística de los eventos y a los participantes inscribirse y acreditar su asistencia de forma digital.

## 2. Metodología de Desarrollo: SDD (Spec-Driven Development)
El desarrollo de este software se rige bajo la metodología **SDD**, como requiere la cátedra. Esto significa que:
- **Las Specs son la Fuente de Verdad**: Antes de escribir código, se definen archivos `.md` en la carpeta `specs/` que detallan el comportamiento esperado del sistema.
- **Contratos Estrictos**: Los archivos `project.md` y `contracts.md` definen las reglas globales (base de datos, stack técnico, convenciones) que todos los módulos deben respetar.
- **Desarrollo asistido por IA**: Se utilizan agentes de IA para interpretar las specs y generar implementaciones coherentes con los contratos.

## 3. Stack Tecnológico
- **Backend**: Django 5.x + Django Rest Framework (DRF)
- **Frontend**: Next.js 14 (App Router) + Tailwind CSS
- **Base de Datos**: PostgreSQL 16
- **Infraestructura**: Docker & Docker Compose

## 4. Objetivos del Producto
1. Facilitar la publicación y visibilidad de eventos académicos.
2. Automatizar el proceso de inscripción y control estricto de cupos.
3. Agilizar la acreditación (check-in) de asistentes el día del evento.
4. Proveer una base sólida para la futura generación de certificados.
