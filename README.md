# Préstamo de equipos de laboratorio

Sistema para registrar préstamos y devoluciones de equipos de un laboratorio universitario, hacer cumplir sus reglas (morosidad, mantenimiento, plazo por categoría, novedades) y consultar los préstamos vencidos con sus días de atraso.

> **Estado: en diseño.** Aún no hay código. Las instrucciones de ejecución se completarán cuando exista el sistema.

## Qué hace
Registra equipos (cada unidad física con código único), solicitantes y encargados. Los encargados inician sesión y registran préstamos y devoluciones; el sistema rechaza los préstamos que rompen las reglas y deja trazado qué encargado hizo cada movimiento.

## Qué quedó fuera
Autenticación avanzada (roles, OAuth, recuperación de contraseña), renovación de préstamos, pagos reales, despliegue en producción e integraciones externas. Las sanciones al solicitante entran en un segundo incremento.

## Documentación
- `docs/BRIEF.md`: contexto, requisitos y atributos de calidad.
- `ASSUMPTIONS.md`: lo que el enunciado no dice y cómo se resolvió.
- `docs/adr/`: decisiones de arquitectura.
- `AGENTS.md` y `BITACORA-IA.md`: contexto para la IA y registro de su uso.

## Requisitos (previstos)
- Java 17 o superior
- PostgreSQL 16
- Git
- Maven no hace falta instalarlo: se usa el wrapper `mvnw` del proyecto.

## Cómo ejecutarlo desde cero
(Pendiente: se completa y se verifica en otra máquina cuando exista el código.)
