# 📦 Módulo de Compras - SAP Business One API

Microservicio desarrollado con **Java 21**, **Spring Boot 3.4.5**, **PostgreSQL** y **Swagger UI**, diseñado para gestionar procesos de **compras empresariales** integrables con **SAP Business One**.

Este módulo sigue buenas prácticas de diseño de software usando patrones robustos: **Factory Method**, **Observer** y **State**, garantizando una arquitectura limpia, extensible y preparada para entornos productivos.

---

## 🚀 ¿Cómo ejecutar?

### 1. Requisitos

- Java 21
- PostgreSQL 14+
- Maven o Gradle

### 2. Configura la base de datos

Crea una base de datos PostgreSQL llamada `compras_db`.

```sql
CREATE DATABASE compras_db;
```

### 3. Configura las credenciales en `application.yml`

```yaml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/compras_db
    username: postgres
    password: tu_contraseña
```

Reemplaza `tu_contraseña` por la contraseña de tu entorno.

### 4. Ejecuta la aplicación

```bash
./mvnw spring-boot:run
```

### 5. Accede a Swagger

- UI interactiva: [http://localhost:8080/swagger-ui.html](http://localhost:8080/swagger-ui.html)
- OpenAPI JSON: [http://localhost:8080/v3/api-docs](http://localhost:8080/v3/api-docs)

---

## 🧠 Patrones de Diseño Implementados

### 🏭 Factory Method

**Objetivo**: Permitir la creación de diferentes tipos de compras sin acoplarse a sus implementaciones específicas.

- `CompraFactory`: interfaz base
- Implementaciones: `CompraDirecta`, `CompraOrden`, `CompraRecurrente`
- Se aplica en `CompraService` para instanciar según tipo definido

### 👁️ Observer

**Objetivo**: Notificar automáticamente a sistemas externos cuando una compra es creada (Inventario, Pagos, etc.).

- Observadores: `SistemaInventario`, `SistemaPago`
- `NotificadorCompra` mantiene la lista de suscriptores
- Se ejecuta automáticamente tras crear una compra

### 🔄 State

**Objetivo**: Manejar el ciclo de vida de una compra según su estado actual.

- Estados: `Pendiente`, `Procesada`, `Facturada`
- Cada clase define su comportamiento al ser procesada
- El patrón garantiza que una compra evolucione correctamente

---

## 📐 Arquitectura de Carpetas

```
src/
└── main/java/com/eqa/sbo/compras/
    ├── CompraApplication.java
    ├── controller/
    ├── service/
    │   ├── estados/
    ├── factory/
    │   ├── tipos/
    ├── observer/
    ├── entity/
    ├── repository/
    ├── dto/
    ├── enums/
    ├── exception/
    └── config/
```

### ✅ Justificación

- **Modular y limpia**: cada paquete es responsable de una única función
- **Alta cohesión y bajo acoplamiento**
- **Preparado para testing e integración**
- **Cumple con principios SOLID**

---

## ⚙️ Comentario Técnico

Este microservicio está diseñado para ser **extensible**, **mantenible** y **escalable**, integrándose de forma natural en ecosistemas SAP. A destacar:

- Uso de `record` para DTOs (concisos e inmutables)
- Separación clara entre lógica de negocio, controladores y persistencia
- Excepciones personalizadas para mayor control
- Swagger UI para exploración y pruebas automáticas
- Preparado para microservicios: puedes agregar Kafka, seguridad (JWT), auditoría o versionado sin romper diseño

---

## 🛠️ Endpoints Documentados

| Método     | Endpoint                    | Descripción                            |
|------------|-----------------------------|----------------------------------------|
| `POST`     | `/api/compras`              | Crear una nueva compra (Factory + Observer) |
| `GET`      | `/api/compras`              | Listar todas las compras               |
| `GET`      | `/api/compras/{id}`         | Obtener una compra por ID              |
| `PUT`      | `/api/compras/{id}`         | Actualizar proveedor o monto           |
| `DELETE`   | `/api/compras/{id}`         | Eliminar compra                        |
| `POST`     | `/api/compras/{id}/procesar`| Cambiar estado según lógica de negocio (State) |

Todos estos están documentados automáticamente en Swagger.

---

## 🧪 Manejo de errores

- `CompraNotFoundException`: cuando una compra no existe
- `@ControllerAdvice` global: captura errores y devuelve respuestas coherentes al cliente
- Errores genéricos son manejados con status 500

---

## 💡 Mejoras Futuras

- Autenticación y autorización con JWT
- Paginación y ordenamiento en listados
- Auditoría con Spring Events o Kafka
- Internacionalización (i18n)
- Validaciones avanzadas con Bean Validation

---

## 📫 Autor

Desarrollado por [@rudy.ruiz](mailto:rudy.ruiz@) como módulo base extensible para ecosistemas SAP Business One.
