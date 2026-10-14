# Constitución — Manager Library (PHP Laravel + React)

Principios innegociables del proyecto adaptados al stack tecnológico del reto: **PHP con Laravel en el Backend** y **React en el Frontend**. Todo cambio que viole estos artículos se rechaza de inmediato.

---

## Artículo I — El dominio de negocio no depende del Framework

En el backend (`app/Domain/`), las entidades, reglas de negocio y objetos de valor no dependen de controladores HTTP, vistas ni librerías de terceros externas:
- Las reglas de negocio (ej. validación de 1 a 3 libros por préstamo, cálculo de fecha de vencimiento a 14 días, cálculo del estado `open`/`overdue`/`closed`) residen en clases de servicio y dominio puro.
- Eloquent se utiliza como adaptador de persistencia (ActiveRecord/Repository), asegurando que las invariantes de negocio se comprueben antes de disparar eventos de base de datos.

## Artículo II — Contrato HTTP Uniforme y Estricto

1. **Respuestas de error:** Todo error HTTP sin excepción devuelve la estructura canónica:
   ```json
   {
     "code": "CODIGO_DE_NEGOCIO",
     "message": "Mensaje legible en español.",
     "details": []
   }
   ```
2. **Sin fuga de información técnica:** Respuestas en producción jamás exponen stack traces, consultas SQL, contraseñas o rutas del servidor.
3. **Mapeo de códigos HTTP:**
   - 401: No autenticado o credenciales inválidas (`INVALID_CREDENTIALS`).
   - 403: Rol no autorizado (`FORBIDDEN_ROLE`).
   - 404: Recurso no encontrado o dado de baja (`NOT_FOUND`).
   - 409: Conflicto de estado (`BOOK_NOT_AVAILABLE`, `MEMBER_HAS_OVERDUE`, `ITEM_ALREADY_RETURNED`, etc.).
   - 422: Validación de entrada (`VALIDATION_ERROR`, `MEMBER_LIMIT_EXCEEDED`, `DUPLICATE_BOOK_IN_LOAN`).

## Artículo III — Seguridad y Aislamiento de Secretos

- La clave nunca se almacena en texto plano: solo se almacena `password_hash` generado mediante algoritmos seguros (Bcrypt/Argon2).
- La clave nunca se retorna en ninguna consulta, recurso o serialización JSON.
- Autenticación mediante tokens Bearer (Sanctum / JWT) con expiración máxima de 8 horas.
- El usuario `admin` inicial se aprovisiona mediante variables de entorno en el arranque; nunca se permite crear usuarios con rol `admin` a través de la API pública o privada.

## Artículo IV — Inmutabilidad y Auditoría de Datos

- Los registros de `loan` y `loan_item` son históricos inmutables: **no se borran**.
- Los libros cuentan con baja lógica mediante `deleted_at`. Un libro con préstamos activos no puede ser eliminado.
- Los datos de libro en `loan_item` (`book_title`, `isbn`, `category_name`) quedan **congelados** al momento del préstamo para garantizar la inmutabilidad de los reportes históricos (ADR-004).

## Artículo V — Frontend React Limpio y Reactivo

- El frontend React desacopla las llamadas HTTP mediante servicios de API dedicados (`services/api.js`).
- Manejo centralizado del estado de autenticación (Context / Hook de sesión) persistiendo el token seguro.
- Retroalimentación visual inmediata con manejo explícito de estados de carga (`loading`), estados vacíos (`empty`) y alertas de error según el `code` de la API.
