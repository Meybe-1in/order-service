# Orders Service

Servicio REST para la gestión de órdenes de compra, desarrollado con Java 21 y Spring Boot.

El proyecto busca implementar una API que permita crear, consultar y gestionar órdenes, aplicando reglas de negocio, validaciones y una arquitectura hexagonal ligera.

## Objetivo

Construir un servicio backend que permita:

- Crear órdenes con uno o varios productos.
- Consultar una orden por su identificador.
- Consultar órdenes mediante paginación y filtros por estado.
- Gestionar las transiciones de estado de una orden.
- Calcular subtotales, descuentos y totales.

## Tecnologías

- Java 21
- Spring Boot
- Maven
- Spring Data JPA
- PostgreSQL
- Flyway
- Docker Compose
- JUnit 5
- Mockito
- Testcontainers
- Swagger / OpenAPI

*Nota: Algunas tecnologías corresponden a la arquitectura y las etapas previstas; se irán configurando durante el desarrollo.*

## Arquitectura

Se utilizará una arquitectura hexagonal ligera, separando las responsabilidades en cuatro capas:

```text
com.order.service
├── domain
│   ├── model
│   ├── exception
│   ├── pricing
│   └── port
│       └── out
├── application
│   ├── port
│   │   └── in
│   ├── service
│   └── dto
├── infrastructure
│   ├── persistence
│   └── config
└── web
    ├── request
    └── response
```

El dominio contendrá las reglas de negocio y no dependerá de frameworks externos.

## Reglas de negocio principales

- Una orden debe contener al menos un producto.
- La cantidad de cada producto debe ser mayor que cero.
- El precio unitario no puede ser negativo.
- Los cálculos monetarios utilizarán `BigDecimal`.
- El total de una orden no puede ser negativo.
- Las órdenes enviadas o canceladas no podrán cambiar nuevamente de estado.

### Estados de una orden

| Estado actual | Estado permitido |
|---|---|
| CREATED | PAID |
| CREATED | CANCELLED |
| PAID | SHIPPED |
| PAID | CANCELLED |
| SHIPPED | Ninguno |
| CANCELLED | Ninguno |

## Requisitos previos

- JDK 21
- Maven
- Git
- Docker Desktop (para la etapa de infraestructura)

## Ejecución

La configuración de ejecución local se documentará cuando se complete la conexión con PostgreSQL, Flyway y Docker Compose.

## Pruebas

El proyecto contempla:

- Pruebas unitarias del dominio.
- Pruebas de los servicios de aplicación.
- Pruebas de integración de persistencia.
- Pruebas de integración de la API.

Para ejecutar las verificaciones Maven:

```bash
mvn clean verify
```

## Estado del proyecto

Actualmente se encuentra en la etapa de configuración inicial. La implementación de dominio, persistencia, endpoints y demás funcionalidades se realizará progresivamente.

## Alcance de la Fase 1

Esta primera fase se concentra en la gestión de órdenes. No incluye Kafka, Outbox, autenticación, servicios cloud ni funcionalidades de inteligencia artificial.
