# Tareas de Implementación — Manager Library (Laravel + React)

Cada tarea concluye cuando su **evidencia** existe, compila y es verificable.

| # | Tarea | Componente | Evidencia de Aceptación |
|---|---|---|---|
| **T-01** | **Auditoría de Spec y Registro de Deuda Técnica** | Docs | `technical-debt.md` creado, inconsistencias documentadas y spec adaptado a Laravel + React. |
| **T-02** | **Infraestructura Docker y Motor PostgreSQL** | Infra | `docker-compose.yml` levanta PostgreSQL 16 con schema `library` y healthcheck en verde. |
| **T-03** | **Migraciones DDL y Seeders de Categorías** | Backend API | 6 tablas creadas (`category`, `book`, `member`, `user`, `loan`, `loan_item`), FKs, restricciones `CHECK` y 5 categorías sembradas. |
| **T-04** | **Autenticación, Middleware de Roles y Salud** | Backend API | `GET /health` responde 200 `{"status":"ok"}`; `POST /auth/login` emite token; endpoints protegidos devuelven 401/403. |
| **T-05** | **Gestión de Usuarios y Alta de Bibliotecarios** | Backend API | `POST /users` crea usuario con rol `librarian` (rechaza `admin` con 422); contraseña hasheada y nunca expuesta. |
| **T-06** | **Catálogo de Libros y Portadas** | Backend API | CRUD de libros con paginación, filtros, baja lógica (`deleted_at`), subida de portada y validación de ISBN de 13 dígitos. |
| **T-07** | **Gestión de Socios** | Backend API | `POST /members` y `GET /members` con validación de documento único y conteo de préstamos activos. |
| **T-08** | **Módulo Transaccional de Préstamos** | Backend API | `POST /loans` valida límites (1 a 3 libros, máx 3 sin devolver, sin vencidos), descuenta stock y congela datos de libro en `loan_item`. |
| **T-09** | **Devolución de Préstamos y Vencidos** | Backend API | `POST /loans/{id}/items/{item_id}/return` restituye disponible; `GET /loans/overdue` lista libros en mora con días de retraso. |
| **T-10** | **Reporte de Libros Más Prestados** | Backend API | `GET /reports/most-borrowed` agrupa por libro y categoría congelada dentro del rango de fechas sin exponer datos personales. |
| **T-11** | **Frontend React: Autenticación y Layout** | Frontend App | Pantalla de Login, barra de navegación con rol, protección de rutas y persistencia de sesión. |
| **T-12** | **Frontend React: Catálogo de Libros** | Frontend App | Interfaz para listar libros con portada, paginador, buscador y modal de creación/edición para admin. |
| **T-13** | **Frontend React: Gestión de Préstamos y Devoluciones** | Frontend App | Flujo interactivo para seleccionar socio, libros disponibles y registrar préstamo; lista de préstamos y acción de devolución. |
| **T-14** | **Frontend React: Reporte y Alertas de Vencidos** | Frontend App | Vista del reporte con filtros de fechas y tabla interactiva de libros vencidos. |
| **T-15** | **Sitio Estático de Presentación (`page`) y Sembrador (`tool`)** | Page & Tool | Landing page de presentación del proyecto y script sembrador que consume la API. |
