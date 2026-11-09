# AGENTS.md

## 1. Contexto del proyecto

Orders Service es una API REST desarrollada con Java 21 y Spring Boot 4.1.2.
Utiliza PostgreSQL, Spring Data JPA, Flyway, Maven y Docker Compose.

El proyecto implementa una arquitectura hexagonal ligera, organizada en:
- domain
- application
- infrastructure
- web

El alcance actual es exclusivamente la Fase 1. No agregar funcionalidades
que no estén contempladas en esta fase.

## 2. Arquitectura

- El dominio no debe depender de Spring ni de infraestructura.
- Las reglas de negocio deben permanecer en el dominio o en la capa de
  aplicación, según su responsabilidad.
- Los controladores deben encargarse de HTTP y delegar la lógica.
- Los repositorios e integraciones externas pertenecen a infraestructura.
- Evitar dependencias entre capas que rompan la arquitectura.
- No introducir patrones o capas adicionales sin una necesidad justificada.

## 3. Reglas de negocio obligatorias

- Una orden debe contener al menos un producto.
- La cantidad de cada producto debe ser mayor que cero.
- El precio unitario no puede ser negativo.
- Utilizar BigDecimal para valores monetarios.
- Los estados permitidos son CREATED, PAID, SHIPPED y CANCELLED.
- SHIPPED y CANCELLED son estados terminales.
- No permitir transiciones inválidas entre estados.
- No modificar estas reglas sin un requisito explícito.

## 4. Base de datos

- PostgreSQL es la base de datos principal.
- Utilizar Flyway para gestionar cambios del esquema.
- No modificar el esquema mediante cambios manuales que deban ser
  reproducibles en otros entornos.
- Revisar integridad referencial, restricciones, índices y consultas.
- Evitar pérdida de datos en migraciones.
- No almacenar contraseñas, tokens ni secretos en el repositorio.

## 5. Seguridad

- Validar las entradas recibidas por la API.
- No exponer información interna, credenciales ni detalles sensibles
  en respuestas o registros.
- Revisar autorización y autenticación cuando se modifiquen endpoints.
- No confiar en identificadores o datos enviados por el cliente sin
  comprobar las reglas correspondientes.
- No debilitar controles de seguridad existentes.

## 6. Pruebas y calidad

- Utilizar JUnit 5 y Mockito para pruebas unitarias.
- Utilizar Testcontainers cuando corresponda probar integración con
  PostgreSQL.
- H2 puede utilizarse donde la configuración de pruebas lo permita.
- Cubrir casos exitosos, errores, límites y reglas de negocio.
- No eliminar ni debilitar pruebas para conseguir que la compilación pase.
- Ejecutar las pruebas Maven correspondientes antes de dar por terminado
  un cambio.
- No afirmar que una prueba pasó si no se ejecutó.

## 7. Responsabilidades de revisión

### Backend
Revisar lógica de negocio, diseño orientado a objetos, manejo de errores,
validaciones, contratos REST y separación de responsabilidades.

### Database
Revisar entidades, relaciones JPA, consultas, transacciones, rendimiento,
integridad de datos y migraciones Flyway.

### Security
Revisar autenticación, autorización, validación de entradas, exposición
de datos, secretos y posibles vulnerabilidades.

### QA
Revisar cobertura, calidad de pruebas, casos límite, regresiones y
consistencia entre implementación y comportamiento esperado.

Estas son perspectivas de revisión complementarias; no se debe afirmar
que existen cuatro agentes independientes si la herramienta no los ejecuta.

## 8. Reglas para cualquier agente de IA

- Leer este archivo antes de proponer cambios.
- Analizar el código existente antes de recomendar una solución.
- No inventar clases, métodos, endpoints ni dependencias.
- No cambiar archivos fuera del alcance solicitado.
- No introducir dependencias nuevas sin justificar su necesidad.
- No reescribir código que no esté relacionado con el problema.
- Priorizar errores funcionales, seguridad, integridad de datos y
  mantenibilidad sobre preferencias de estilo.
- Evitar comentarios repetitivos, triviales o sin impacto práctico.
- Explicar el impacto y la evidencia de cada hallazgo.
- Diferenciar errores confirmados de posibles riesgos.
- Proponer soluciones compatibles con Java 21 y las versiones actuales
  del proyecto.
- No aprobar, fusionar ni cerrar Pull Requests automáticamente.
- No ejecutar acciones destructivas ni modificar la historia Git.
- Respetar el flujo de trabajo: rama propia, commits y Pull Request
  hacia develop.
