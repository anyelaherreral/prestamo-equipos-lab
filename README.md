# Préstamo de equipos de laboratorio

Sistema para registrar préstamos y devoluciones de equipos de laboratorio, con reglas de negocio y una consulta de préstamos vencidos.

## Qué hace
(2–3 frases.)

## Qué quedó fuera
(Alcance que decidiste no cubrir y por qué.)

## Requisitos
- Java 17+
- Maven 3.9+ (o Gradle)
- PostgreSQL 14+
- Git

## Cómo ejecutarlo desde cero
1. Clonar el repo:
   ```bash
   git clone <url> && cd <carpeta>
   ```
2. Crear la base de datos:
   ```sql
   CREATE DATABASE prestamos;
   ```
3. Configurar credenciales en `src/main/resources/application.properties` (o variables de entorno).
4. Arrancar:
   ```bash
   ./mvnw spring-boot:run
   ```
5. Abrir Swagger: http://localhost:8080/swagger-ui.html

## Flujo de ejemplo
(Pasos para probar un préstamo completo de principio a fin.)

## Estructura del repo
- `src/` código
- `docs/adr/` decisiones de arquitectura
- `AGENTS.md`, `ASSUMPTIONS.md`, `BITACORA-IA.md`

## Pruebas
```bash
./mvnw test
```
