# Plan de Implementación — Manager Library (Laravel + React)

## 1. Estrategia de Entrega por Fases y Commits Continuos

Para cumplir con la directriz del evaluador sobre **commits continuos y desarrollo guiado por especificación (SDD)**, la construcción sigue un orden secuencial verificable:

1. **Fase 1: Especificación y Deuda Técnica (Docs):**
   - Formalización de `technical-debt.md`.
   - Creación de `spec-laravel-react/` con arquitectura y tareas.
   - Primer commit y Pull Request en el repositorio de documentación.

2. **Fase 2: Infraestructura y Base de Datos (Infra + API):**
   - Configuración de contenedor PostgreSQL en `test-manager-library-3145555-infra`.
   - Definición del esquema `library` y migraciones en `test-manager-library-3145555-api`.
   - Seeding de las 5 categorías oficiales con nombres en inglés.

3. **Fase 3: Núcleo de la API Backend (Laravel):**
   - Endpoints de autenticación y salud (`/health`, `/auth/login`).
   - Módulos de Catálogo (`/books`, `/categories`) y Socios (`/members`).
   - Transaccionalidad de Préstamos y Devoluciones (`/loans`, `/loans/{id}/items/{item_id}/return`, `/loans/overdue`).
   - Módulo de Reportes (`/reports/most-borrowed`).

4. **Fase 4: Frontend React (App):**
   - Base del proyecto con Vite y diseño moderno responsive.
   - Cliente API con manejo de tokens Bearer.
   - Vistas de catálogo, préstamos, socios y reportes.

5. **Fase 5: Presentación y Sembrado (Page + Tool):**
   - Landing page pública en `test-manager-library-3145555-page`.
   - Script sembrador de demostración en `test-manager-library-3145555-tool`.
