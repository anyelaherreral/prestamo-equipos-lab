# AGENTS.md

> Contexto para el agente de IA. Es un documento vivo: cada vez que el agente se equivoque por falta de contexto, se agrega aquí la línea que faltaba. Debe reflejar el estado real del sistema.

## Propósito
Sistema de préstamo de equipos de laboratorio (portátiles, osciloscopios, kits de sensores) para estudiantes y docentes. Reemplaza el cuaderno donde hoy se pierden equipos. Proyecto de portafolio para practicar bases de datos, API REST y comunicación front-back, con decisiones defendibles.

## Forma de trabajo
1. La persona plantea una necesidad, interpretación o alternativa.
2. La IA cuestiona, compara y explora consecuencias, y explica antes de escribir código.
3. Se revisan alternativas y casos límite.
4. La persona toma la decisión final.
5. La decisión se refleja en el código y, si corresponde, en un ADR, en `ASSUMPTIONS.md` y en `BITACORA-IA.md`.

Reglas para el agente:
- Una sugerencia de la IA no se convierte sola en requisito.
- No reportar algo como implementado o probado sin evidencia de que se ejecutó.
- Todo código propuesto debe poder explicarse línea por línea antes de incorporarse.
- Nunca escribir credenciales en el código ni en Git; usar variables de entorno.

## Stack
- Java 17+, Spring Boot, Maven (wrapper `mvnw`)
- PostgreSQL
- Swagger / OpenAPI (springdoc)
- Front: HTML, CSS y JavaScript sencillo (Bootstrap o Tailwind por decidir en un ADR)
- Arquitectura: monolito en capas (controller, service, repository), sin colas ni microservicios

## Modelo conceptual (borrador, por confirmar)
- **Encargado:** id, nombre, usuario (único), password_hash, activo.
- **CategoriaEquipo:** id, nombre, plazo_dias
- **Equipo:** unidad física con código único (Tensiómetro-1, Tensiómetro-2); id, código, nombre, categoria_id; el modelado de "en mantenimiento" y "bloqueado por novedad" está pendiente de ADR
- **Solicitante:** id, nombre, documento, tipo (estudiante o docente)
- **Prestamo:** id, fecha_hora_prestamo, fecha_limite, equipo_id, solicitante_id, encargado_id (quien lo registró). Un préstamo = un equipo.
- **Devolucion:** id, fecha_hora, prestamo_id, novedad (nullable), encargado_id (quien la recibió)
- **Sancion (segundo incremento):** id, solicitante_id, devolucion_id, tipo_sancion_id, fecha_inicio, fecha_levantada (nullable), encargado_id que la levantó. "Vigente" = aún no levantada, se deriva.
- **TipoSancion (segundo incremento):** id, nombre, condicion_levantamiento (texto).

Estados "vencido", "disponible" y "sanción vigente" se derivan, no se guardan (ver Principios). Un equipo con préstamo activo no pasa a mantenimiento hasta devolverse (A-07).

## Reglas de negocio (no negociables)
1. Un solicitante con un préstamo vencido sin devolver no puede pedir otro equipo.
2. Un equipo en mantenimiento no aparece como disponible aunque esté físicamente en el almacén.
3. El plazo de préstamo depende de la categoría del equipo, no del solicitante.
4. Una devolución con novedad (daño, faltante) bloquea el equipo hasta que alguien lo libere.

## Consulta obligatoria
Listado de préstamos vencidos a la fecha, con días de atraso.

## Principios de desarrollo
- Priorizar el cumplimiento de las reglas del enunciado y mantener el alcance acotado.
- Las reglas de negocio viven en el servicio, nunca en el controlador ni en el front.
- No almacenar información derivada que pueda calcularse de forma confiable (por ejemplo, "vencido" y "días de atraso").
- Usar restricciones de la base de datos para invariantes estructurales (por ejemplo, un solo préstamo activo por equipo).
- La API se diseña desde el uso, no desde las tablas: DTOs, no entidades JPA.
-El encargado que registra una acción sale siempre de la sesión autenticada en el servidor, nunca de un parámetro que envíe el cliente.
- El encargado que registra una acción sale siempre de la sesión autenticada en el servidor, nunca de un parámetro que envíe el cliente.

## Fuera de alcance
Autenticación avanzada (roles, OAuth, recuperación de contraseña), renovación de préstamos, pagos reales, despliegue en producción e integraciones externas.

## Documentación
- `README.md`: qué es, cómo ejecutarlo desde cero.
- `docs/BRIEF.md`: contexto, requisitos y atributos de calidad.
- `ASSUMPTIONS.md`: lo que el enunciado no dice y se resolvió por cuenta propia.
- `BITACORA-IA.md`: una entrada por sesión de trabajo con IA.
- `docs/adr/`: decisiones de arquitectura y diseño.

## Estado del diseño
**2026-11-23.** Decididos: alcance, requisitos y atributos de calidad (`docs/BRIEF.md`), asunciones A-01 a A-15 (`ASSUMPTIONS.md`), stack provisional (Spring Boot, PostgreSQL, front HTML/JS) y arquitectura de monolito en capas. Pendientes: modelo de datos (incluye cómo se guardan "en mantenimiento" y "bloqueado por novedad"), los ADR, y los tipos de sanción (A-16). No hay código ni base de datos creados todavía; nada se ha implementado ni probado.

## Errores frecuentes del agente
- (fecha) El agente hizo X porque no sabía Y → regla agregada: ...
