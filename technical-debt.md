# Registro de Deuda Técnica — Manager Library

Este documento registra formalmente las inconsistencias, ambigüedades y compromisos de diseño identificados durante la fase de análisis de especificación (**SDD**), conforme a la regla de oro:
> *«No se puede cambiar el alcance del producto (nombres de atributos, nombres de entidades ni endpoints). Si se encuentran discrepancias o nombres en español, se trasladan al inglés o se registra formalmente la deuda técnica sin alterar el modelo.»*

---

## 1. Matriz de Deuda Técnica e Inconsistencias

| ID | Hallazgo / Inconsistencia | Capas afectadas | Regla de Alcance | Decisión / Mitigación |
|---|---|---|---|---|
| **DT-01** | **Discrepancia Singular / Plural entre Tablas y Endpoints** | Base de Datos vs API HTTP vs Eloquent | Innegociable: no cambiar entidades ni endpoints | En `data-model.md` las tablas son singulares (`category`, `book`, `member`, `loan`, `loan_item`, `user`), mientras que en `api-contract.md` las rutas son plurales (`/categories`, `/books`, `/members`, `/loans`, `/users`). En Laravel (Eloquent), la convención por defecto es plural. **Solución:** Se respeta estrictamente el modelo físico singular en PostgreSQL (`library.book`, etc.) forzando `$table = 'library.book'` en los modelos de Laravel sin alterar rutas ni esquema. |
| **DT-02** | **Nombres en Español en Datos Semilla (Categorías)** | Base de datos (`category.name`) y Respuestas API | Los términos de dominio en código y base de datos deben ser en inglés | En `data-model.md` §9 se proponen 5 categorías en español (`Novela`, `Ciencia`, `Tecnología`, `Historia`, `Infantil`). **Solución:** Se registran y persisten con su valor técnico estandarizado en inglés (`Novel`, `Science`, `Technology`, `History`, `Children`). El frontend React manejará la capa de presentación/traducción hacia el usuario final. |
| **DT-03** | **Atributo `status` Calculado vs Persistido en Préstamos** | Modelo de Datos (`loan`) vs API (`GET /loans`) | No agregar columnas no autorizadas al alcance | `status` (`open`, `overdue`, `closed`) no existe como columna en la tabla `loan`, sino que se calcula en tiempo de ejecución a partir de `loan_item.returned_at` y `due_at`. **Deuda Técnica:** Filtrar y paginar por `status` en grandes volúmenes requiere expresiones condicionales SQL complejas. Se mantiene calculado mediante subconsultas SQL optimizadas en el repositorio/query builder de Laravel para no alterar el esquema físico. |
| **DT-04** | **Restricciones de Motor Diferidas a T-18** | Modelo Físico (`book`, `user`) | Integridad referencial y de dominio | Las restricciones `ck_book_total_positive` (`total_copies >= 1`) y `ck_user_role` (`role IN ('admin', 'librarian')`) están documentadas como «pendientes T-18 (hoy solo dominio)». **Mitigación:** Se implementan tanto en las validaciones de Laravel (Form Requests / Domain Rules) como directamente en la migración DDL de PostgreSQL para blindar la concurrencia. |
| **DT-05** | **Normalización y Sensibilidad a Mayúsculas en Usuarios** | Autenticación (`POST /auth/login`, `POST /users`) | Contrato API | El contrato exige que `"Ana "` se guarde como `ana`. Se implementa mutador / normalizador en la capa de entrada del servicio de usuario antes de persistir o comparar hashes de contraseñas. |
| **DT-06** | **Congelación Histórica de Datos en Líneas de Préstamo** | Inmutabilidad de Auditoría | ADR-004 | Las columnas `loan_item.book_title`, `loan_item.isbn` y `loan_item.category_name` son copias congeladas en el instante del préstamo sin foreign key a `category`. Deben poblarse atómicamente dentro de la misma transacción que descuenta `available_copies`. |

---

## 2. Mapa de Traducción de Datos Semilla

Para dar cumplimiento a la directriz de mantener código, base de datos y atributos en inglés:

| ID Semilla (UUID) | Nombre Original (Español) | Nombre Estandarizado (Inglés) | Presentación UI |
|---|---|---|---|
| `11111111-1111-4111-8111-111111111111` | Novela | `Novel` | Novela |
| `22222222-2222-4222-8222-222222222222` | Ciencia | `Science` | Ciencia |
| `33333333-3333-4333-8333-333333333333` | Tecnología | `Technology` | Tecnología |
| `44444444-4444-4444-8444-444444444444` | Historia | `History` | Historia |
| `55555555-5555-4555-8555-555555555555` | Infantil | `Children` | Infantil |

---

## 3. Conformidad con los Pilares de Calidad

1. **Robustez y Concurrencia:** La restricción `0 <= available_copies <= total_copies` se define a nivel motor en PostgreSQL para que, ante dos solicitudes simultáneas que intenten prestar el último ejemplar, el motor aborte la segunda con error de restricción (capturado como `409 BOOK_NOT_AVAILABLE`).
2. **Seguridad y Privacidad:** La contraseña nunca se expone en respuestas ni logs. El endpoint de reportes nunca expone datos personales de socios ni bibliotecarios.
3. **Inmutabilidad:** Préstamos y devoluciones no admiten eliminación física (`DELETE`), y los libros utilizan baja lógica (`deleted_at`).
