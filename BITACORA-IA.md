# BITACORA-IA.md

> Una entrada corta por sesión de trabajo. Se escribe durante, no al final.

## Sesión 1: 2026-11-22 (planeación)

- **Herramienta:** Claude (chat). Respaldo: pendiente de definir (ver ADR de herramienta de IA).
- **Qué pedí:** ayuda para organizar un proyecto de portafolio a partir de la prueba técnica del Sistema A (préstamo de equipos de laboratorio) y revisar un repositorio de referencia, apuntes de contratos de API y diapositivas de arquitectura.
- **Qué me propuso el agente:** plantillas de AGENTS.md, ASSUMPTIONS.md, BITACORA-IA.md, README.md y ADR; un brief con requisitos funcionales y seis atributos de calidad con medidas; un plan por fases; stack Spring Boot, PostgreSQL y front HTML/JS; cuatro preguntas de asunciones.
- **Qué acepté y por qué:** el brief, el stack, el monolito en capas, vencido desde el día siguiente (A-02), sin renovaciones (A-04), liberación por el encargado (A-05).
- **Qué rechacé o cambié y por qué:** -El agente había dejado A-11 como "elegir el encargado al iniciar"; fui yo quien señaló que contradecía la táctica de autorizar al actor. La parte 403/404 de la táctica no se aplica porque todos los encargados ven los mismos datos (A-15).
  - El agente recomendó "un solo préstamo activo por solicitante". Se detectó que con eso la regla 1 queda implícita y sin lógica propia, así que se cambió a "varios préstamos activos" (A-03).
  - El agente propuso un solo rol de encargado. Yo decidí que puede haber varios y que debe quedar trazabilidad de quién registró cada movimiento (A-01), lo que agrega la entidad Encargado.
  - El agente redactó A-06 hablando de "equipo" sin distinguir unidad de tipo. Yo lo precisé: Equipo es cada unidad física y la categoría es el tipo (A-06).
  - Yo decidí agregar sanciones al solicitante por tardanza o novedad (A-08, A-09), pero acepté dejarlas para un segundo incremento para no retrasar las 4 reglas del enunciado.
- **Qué quedó sin verificar:** los números de los escenarios de calidad (QA-01 a QA-06) son propuestas sin validar; el modelo conceptual de AGENTS.md es un borrador; A-11 (identificar al encargado sin login), A-12 y A-13 siguen abiertas; no se ha creado todavía código ni base de datos del proyecto.
