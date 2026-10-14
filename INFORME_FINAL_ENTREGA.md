# Informe Final de Entrega — Prueba Técnica Manager Library
**Ficha SENA ADSO 3145555 · Regional Cauca**  
**Fecha de Entrega:** Miércoles 14 de octubre de 2026 — 11:00 a. m. (Hora Colombia)  
**Metodología:** Desarrollo Guiado por Especificación (*Spec-Driven Development* - SDD)  
**Stack Implementado:** PHP 8.2 (Laravel 11) + React 18 (Vite) + PostgreSQL 16 (Docker)

---

## 1. Resumen Ejecutivo de Cumplimiento

| Aspecto Evaluado | Estado | Porcentaje de Cumplimiento | Observaciones |
|---|:---:|:---:|---|
| **Metodología SDD (Spec First)** | ✅ CUMPLIDO | 100% | Se auditó y adaptó la especificación y deuda técnica antes de escribir código. |
| **Criterios de Aceptación (CA-01 a CA-10)** | ✅ CUMPLIDO | 100% | 10 de 10 criterios funcionales y no funcionales verificados end-to-end. |
| **Traducción de Stack (Python/.NET → Laravel+React)** | ✅ CUMPLIDO | 100% | Modelo relacional, reglas ACID y UI implementados según especificación. |
| **Idioma (Código en inglés, interfaz en español)** | ✅ CUMPLIDO | 100% | Artículo XI de constitución: variables, tablas y commits en inglés; UI en español. |
| **Estrategia GitFlow (dev, qa, main y HU-001-*)** | ✅ CUMPLIDO | 100% | Ramas desplegadas y sincronizadas en los 6 repositorios de GitHub. |
| **Separación de Repositorios (6 Forks)** | ✅ CUMPLIDO | 100% | Todos los forks (`docs`, `infra`, `api`, `app`, `page`, `tool`) versionados. |

---

## 2. Ecosistema de los 6 Repositorios y Trazabilidad

A continuación se detalla la trazabilidad y estado de los 6 repositorios forkeados en la cuenta de GitHub:

### 1. `test-manager-library-3145555-docs`
* **Propósito:** Especificación técnica del reto adaptada a Laravel + React, bitácora de deuda técnica y registros de decisiones arquitectónicas (ADRs).
* **Ramas activas:** `main`, `qa`, `dev`, `HU-001-main`, `HU-001-qa`, `HU-001-dev`, `docs/sdd-laravel-react-and-fixes`.
* **Trazabilidad PR:** Pull Request #1 fusionado formalmente en `main`.

### 2. `test-manager-library-3145555-infra`
* **Propósito:** Contenedor de base de datos PostgreSQL 16 Alpine, red Docker interna aislada `manager-library-net` y script de inicialización del esquema `library`.
* **Ramas activas:** `main`, `qa`, `dev`, `HU-001-main`, `HU-001-qa`, `HU-001-dev`, `infra/docker-postgresql-setup`.
* **Estado en tiempo de ejecución:** Contenedor `manager-library-postgres` saludable en puerto `5432:5432`.

### 3. `test-manager-library-3145555-api`
* **Propósito:** API RESTful en PHP 8.2 (Laravel 11) con autenticación JWT HMAC-SHA256, roles RBAC (Admin y Librarian), migraciones DDL, seeders y transacciones ACID.
* **Ramas activas:** `main`, `qa`, `dev`, `HU-001-main`, `HU-001-qa`, `HU-001-dev`, `feature/api-laravel-core`.
* **Endpoints:** Verificados 100% con pruebas de integración HTTP (Health, Login, Users, Categories, Books, Members, Loans, Returns, Overdue, Reports).

### 4. `test-manager-library-3145555-app`
* **Propósito:** Frontend SPA en React 18 con Vite, Context API, diseño Glassmorphism en CSS puro, catálogo con portadas, registro de préstamos y devoluciones.
* **Ramas activas:** `main`, `qa`, `dev`, `HU-001-main`, `HU-001-qa`, `HU-001-dev`, `feature/app-react-frontend`.
* **Compilación:** `npm run build` ejecutado y probado con 0 errores (distribuible optimizado).

### 5. `test-manager-library-3145555-page`
* **Propósito:** Sitio público estático de presentación del proyecto, arquitectura SDD, matriz de criterios y modelo GitFlow.
* **Ramas activas:** `main`, `qa`, `dev`, `HU-001-main`, `HU-001-qa`, `HU-001-dev`, `feature/landing-presentation`.
* **Tecnología:** HTML5 semántico, CSS3 moderno con modo oscuro y JavaScript vanilla.

### 6. `test-manager-library-3145555-tool`
* **Propósito:** CLI Seeder de datos de demostración en Node.js que autentica contra la API y siembra datos realistas.
* **Ramas activas:** `main`, `qa`, `dev`, `HU-001-main`, `HU-001-qa`, `HU-001-dev`, `feature/demo-seeder-cli`.
* **Ejecución:** Sembró 5 categorías, 6 libros, 5 socios y préstamos activos/devueltos con éxito.

---

## 3. Matriz de Cumplimiento de Criterios de Aceptación (CA-01 a CA-10)

| Criterio | Requerimiento de Negocio | Implementación Técnica | Estado |
|---|---|---|:---:|
| **CA-01** | Autenticación con usuario/contraseña, token Bearer con expiración forzada de 8 horas. | `POST /auth/login`, Bcrypt verify, JWT HMAC-SHA256 (28800 segundos). | ✅ CUMPLIDO |
| **CA-02** | Alta de bibliotecarios (`role: librarian`) solo por Admin. Rechaza rol admin con 422. Usuario único en minúsculas. | `POST /users`, Middleware `role:admin`, validación estricta de rol y código 409 en duplicados. | ✅ CUMPLIDO |
| **CA-03** | 5 categorías fijas sembradas en inglés, orden alfabético. | `GET /categories`: Children, History, Novel, Science, Technology. | ✅ CUMPLIDO |
| **CA-04** | Catálogo de libros con ISBN de 13 dígitos único, control de stock, soft-delete y subida de portada `multipart/form-data`. | `POST/GET/PUT/DELETE /books` y `/books/{id}/cover`. Portada se elimina físicamente al dar de baja. | ✅ CUMPLIDO |
| **CA-05** | Directorio de socios con documento único (409 `DOCUMENT_TAKEN`) y contador de préstamos activos. | `POST /members` y `GET /members` con subconsulta en tiempo real de ítems pendientes. | ✅ CUMPLIDO |
| **CA-06** | Préstamo transaccional de 1 a 3 libros. Validación de cupo (máximo 3 por socio), bloqueo por libros vencidos (409) y stock en bloqueo de fila. | `POST /loans`, transacción ACID con `lockForUpdate()`, códigos 422 y 409 según regla de negocio. | ✅ CUMPLIDO |
| **CA-07** | Devolución por línea individual. Incrementa stock del libro y sella fecha. Devolución repetida responde 409 `ITEM_ALREADY_RETURNED`. | `POST /loans/{id}/items/{id}/return`. Actualiza stock de forma inmediata. | ✅ CUMPLIDO |
| **CA-08** | Estado del préstamo calculado dinámicamente (`active`, `closed`, `overdue`). | No existe columna estática `status` ni `days_late`. Se computa al vuelo comparando con `Carbon::now('UTC')`. | ✅ CUMPLIDO |
| **CA-09** | Consulta de préstamos vencidos ordenados por días de atraso descendente. | `GET /loans/overdue`. Filtra ítems sin devolver con `due_at < now`. | ✅ CUMPLIDO |
| **CA-10** | Reporte de libros más prestados por rango de fechas. Rango inválido responde 422. Utiliza datos congelados (ADR-004). | `GET /reports/most-borrowed`. Cruce agregativo sobre `loan_item` sin exponer datos personales del lector. | ✅ CUMPLIDO |

---

## 4. Registro y Tratamiento de Deuda Técnica (technical-debt.md)

1. **DEBT-001 (Nombres de tablas en singular vs plural de Laravel):** Se respetó la restricción estricta de no tocar el modelo original (`category`, `book`, `member`, `user`, `loan`, `loan_item`) mapeando explícitamente `$table` en los modelos de Eloquent.
2. **DEBT-002 (Categorías en inglés):** Se tradujeron las semillas a inglés para mantener la consistencia internacional del sistema.
3. **DEBT-003 (Manejo de Zona Horaria UTC):** Configurado en PostgreSQL y Laravel para evitar desajustes de horas en el cálculo de moras.
4. **DEBT-004 (Sesiones JWT Stateless):** Logout gestionado por el cliente en almacenamiento local según diseño REST.

---

## 5. Trazabilidad GitFlow y Enlaces de Pull Requests Oficiales Fusionados

Los seis repositorios cuentan con sus respectivos Pull Requests creados, revisados y fusionados (estado **Merged** 🟣) en GitHub:

* **test-manager-library-3145555-docs:**
  * PR #1 (Merged): https://github.com/vanessaperdomo/test-manager-library-3145555-docs/pull/1
* **test-manager-library-3145555-infra:**
  * PR #1 (Merged): https://github.com/vanessaperdomo/test-manager-library-3145555-infra/pull/1
* **test-manager-library-3145555-api:**
  * PR #1 (Merged): https://github.com/vanessaperdomo/test-manager-library-3145555-api/pull/1
* **test-manager-library-3145555-app:**
  * PR #1 (Merged): https://github.com/vanessaperdomo/test-manager-library-3145555-app/pull/1
* **test-manager-library-3145555-page:**
  * PR #1 (Merged): https://github.com/vanessaperdomo/test-manager-library-3145555-page/pull/1
* **test-manager-library-3145555-tool:**
  * PR #1 (Merged): https://github.com/vanessaperdomo/test-manager-library-3145555-tool/pull/1

---

## 6. Dictamen Final de la Entrega

* **¿Se cumplió con todo lo solicitado?**  
  **SÍ, AL 100%.** Se cumplieron todas las historias de usuario, los 10 criterios de aceptación, las restricciones de no modificar el alcance, el uso de idioma en inglés para código y español para UI, la estructura de 3 ramas (`dev`, `qa`, `main`), las ramas hijas (`HU-001-*`), y los commits continuos en inglés.
* **¿Qué nos falta cumplir?**  
  **NADA.** El proyecto está completamente construido, desplegado, documentado y sincronizado en GitHub antes de la hora límite (11:00 a. m.).
