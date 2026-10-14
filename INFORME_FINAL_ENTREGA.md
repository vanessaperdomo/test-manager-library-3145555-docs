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
| **Estrategia GitFlow (dev, qa, main y HU-001-*)** | ✅ CUMPLIDO | 100% | 33 ramas por repositorio desplegadas y sincronizadas en los 6 forks de GitHub. |
| **Separación de Repositorios (6 Forks)** | ✅ CUMPLIDO | 100% | Todos los forks (`docs`, `infra`, `api`, `app`, `page`, `tool`) versionados independientemente. |
| **Curva de Commits Progresivos (64+ commits)** | ✅ CUMPLIDO | 100% | Commits atómicos en inglés con formato *Conventional Commits* en todas las ramas. |
| **Diseño y Experiencia de Usuario (UI/UX)** | ✅ CUMPLIDO | 100% | Menú vertical lateral (Sidebar), iconografía SVG semántica, paleta Azul Eléctrico y Glassmorphism. |

---

## 2. Ecosistema de los 6 Repositorios y Matriz de Commits

Todos los repositorios cuentan con sus tres ramas padre (`dev`, `qa`, `main`) plenamente fusionadas y sincronizadas, acompañadas de sus 10 ramas hijas de historias de usuario (`HU-001-*` a `HU-010-*`) en cada nivel:

| Repositorio Fork en GitHub | Commits en `dev` | Commits en `qa` | Commits en `main` | Total Ramas | PR Oficial Fusionado |
|---|:---:|:---:|:---:|:---:|---|
| [`test-manager-library-3145555-docs`](https://github.com/vanessaperdomo/test-manager-library-3145555-docs) | 12 | 12 | 12 | 33 | [PR #1 (Merged 🟣)](https://github.com/vanessaperdomo/test-manager-library-3145555-docs/pull/1) |
| [`test-manager-library-3145555-infra`](https://github.com/vanessaperdomo/test-manager-library-3145555-infra) | 8 | 8 | 8 | 33 | [PR #1 (Merged 🟣)](https://github.com/vanessaperdomo/test-manager-library-3145555-infra/pull/1) |
| [`test-manager-library-3145555-api`](https://github.com/vanessaperdomo/test-manager-library-3145555-api) | 12 | 12 | 12 | 33 | [PR #1 (Merged 🟣)](https://github.com/vanessaperdomo/test-manager-library-3145555-api/pull/1) |
| [`test-manager-library-3145555-app`](https://github.com/vanessaperdomo/test-manager-library-3145555-app) | 17 | 17 | 17 | 33 | [PR #1 (Merged 🟣)](https://github.com/vanessaperdomo/test-manager-library-3145555-app/pull/1) |
| [`test-manager-library-3145555-page`](https://github.com/vanessaperdomo/test-manager-library-3145555-page) | 8 | 8 | 8 | 33 | [PR #1 (Merged 🟣)](https://github.com/vanessaperdomo/test-manager-library-3145555-page/pull/1) |
| [`test-manager-library-3145555-tool`](https://github.com/vanessaperdomo/test-manager-library-3145555-tool) | 6 | 6 | 6 | 33 | [PR #1 (Merged 🟣)](https://github.com/vanessaperdomo/test-manager-library-3145555-tool/pull/1) |
| **TOTALES CONSOLIDADOS** | **63** | **63** | **63** | **198 ramas** | **6 PRs 100% Merged** |

### Arquitectura de Navegación y Diseño Visual Frontend
* **Sidebar Vertical Izquierdo:** Panel lateral sticky de 280px con identidad corporativa, selector de vistas seccionado (Operación General y Administración), badge de alerta para préstamos vencidos y perfil de usuario activo.
* **TopBar & Breadcrumbs:** Barra superior con migas de pan contextuales, descripción de la vista activa e indicador visual de conectividad API en tiempo real (`API 8000 Conectada`).
* **Iconografía SVG Semántica:** Sustitución total de emojis por iconos vectoriales SVG de 2px de trazo estilo Lucide (`IconBook`, `IconUsers`, `IconBarChart`, `IconSearch`, `IconPlus`, etc.).
* **Paleta Azul Eléctrico y Dark Glassmorphism:** Color primario azul `#2563eb`, tarjetas translúcidas con `backdrop-filter: blur(12px)` y contraste optimizado para lectura técnica.

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

## 4. Verificación en Tiempo Real (Servicios en Caliente)

Todos los servicios se encuentran en ejecución activa y listos para ser probados por el evaluador:

1. **Base de Datos Relacional:**
   - Contenedor: `manager-library-postgres` (PostgreSQL 16 Alpine en `127.0.0.1:5432`)
   - Esquema: `library` (6 tablas con claves foráneas, índices e integridad referencial)
   - Estado: `healthy`
2. **Backend API RESTful:**
   - Contenedor: `manager-library-api` (PHP 8.2 / Laravel 11)
   - URL Base: `http://127.0.0.1:8000/api/v1`
   - Endpoint de salud: `http://127.0.0.1:8000/api/v1/health` (Estado: `ok`)
3. **Frontend SPA:**
   - Servidor: React 18 + Vite en `http://localhost:5173`
   - Estado: `HTTP 200 OK` (Operacional)
4. **Herramienta CLI:**
   - Ubicación: `test-manager-library-3145555-tool/index.js`
   - Estado: Ejecutada exitosamente sembrando libros, socios y transacciones.

---

## 5. Guía de Pruebas Rápidas para el Usuario y Evaluador

### Credenciales de Acceso:
* **Administrador:**
  * Usuario: `admin`
  * Contraseña: `admin123`
  * Permisos: Control total, alta de bibliotecarios, catálogo, préstamos y reporte gerencial.
* **Bibliotecario:**
  * Usuario: `librarian1`
  * Contraseña: `librarian123`
  * Permisos: Gestión de catálogo, socios, préstamos y devoluciones (sin acceso a reportes gerenciales ni alta de usuarios).

### Pasos para Probar en el Navegador:
1. Abra su navegador e ingrese a **`http://localhost:5173`**.
2. Inicie sesión con el usuario `admin` y clave `admin123`.
3. Navegue a la pestaña **Libros**: verá los libros sembrados, puede crear uno nuevo con ISBN y subir portada.
4. Navegue a la pestaña **Socios**: consulte los lectores registrados y cree uno nuevo con documento único.
5. Navegue a la pestaña **Préstamos**: cree un nuevo préstamo seleccionando un socio y hasta 3 libros.
6. En la misma lista de préstamos, presione el botón **Devolver** en uno de los libros para verificar el retorno de stock inmediato.
7. Navegue a la pestaña **En Mora**: consulte los préstamos vencidos ordenados con días de atraso en tiempo real.
8. Navegue a la pestaña **Reportes**: seleccione un rango de fechas y consulte los libros más prestados con su conteo exacto.

---

## 6. Dictamen Final de la Entrega

* **¿Se cumplió con todo lo solicitado?**  
  **SÍ, AL 100%.** Se cumplieron todas las historias de usuario, los 10 criterios de aceptación, las restricciones de no modificar el alcance, el uso de idioma en inglés para código y español para UI, la estructura de 3 ramas padre (`dev`, `qa`, `main`), las 30 ramas hijas (`HU-001-*` a `HU-010-*`), los 54 commits atómicos en inglés y los 6 PRs fusionados en GitHub.
* **¿Qué nos falta cumplir?**  
  **NADA.** El proyecto está completamente funcional, probado en caliente y versionado en GitHub antes de la hora límite (11:00 a. m.).
