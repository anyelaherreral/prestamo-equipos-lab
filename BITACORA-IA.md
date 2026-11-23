# BITACORA-IA.md

> Una entrada corta por sesión de trabajo. Se escribe durante, no al final.

## Sesión 1: 2026-11-22 (planeación)

- **Herramienta:** Claude (chat). Respaldo: pendiente de definir (ver ADR de herramienta de IA).
- **Qué pedí:** ayuda para organizar un proyecto de portafolio a partir de la prueba técnica del Sistema A (préstamo de equipos de laboratorio) , apuntes de contratos de API.
- **Qué me propuso el agente:** plantillas de AGENTS.md, ASSUMPTIONS.md, BITACORA-IA.md, README.md y ADR; un brief con requisitos funcionales y seis atributos de calidad con medidas; un plan por fases y cuatro preguntas de asunciones.
- **Qué acepté y por qué:** el brief, el stack, el monolito en capas, vencido desde el día siguiente (A-02), sin renovaciones (A-04), liberación por el encargado (A-05).
- **Qué rechacé o cambié y por qué:**
  - El agente recomendó "un solo préstamo activo por solicitante". Se detectó que con eso la regla 1 queda implícita y sin lógica propia, así que se cambió a "varios préstamos activos" (A-03).
  - El agente propuso un solo rol de encargado. Yo decidí que puede haber varios y que debe quedar trazabilidad de quién registró cada movimiento (A-01), lo que agrega la entidad Encargado.
  - El agente redactó A-06 hablando de "equipo" sin distinguir unidad de tipo. Yo lo precisé: Equipo es cada unidad física y la categoría es el tipo (A-06).
  - Yo decidí agregar sanciones al solicitante por tardanza o novedad (A-08, A-09), pero acepté dejarlas para un segundo incremento para no retrasar las 4 reglas del enunciado.
- **Qué quedó sin verificar:** los números de los escenarios de calidad son propuestas sin validar; el modelo conceptual de AGENTS.md es un borrador; no se ha creado código ni base de datos.

## Sesión 2: 2026-11-23 (asunciones A-11 a A-15 y revisión del repositorio)

- **Herramienta:** Claude (chat).
- **Qué pedí:** revisar mis observaciones sobre A-11 y A-13 a la luz de las diapositivas de tácticas.
- **Qué me propuso el agente:** verificó la cita de las diapositivas (autorizar al actor), señaló que elegir el encargado en un desplegable haría falsificable la trazabilidad, propuso un catálogo de tipos de sanción y, al revisar el repo, listó errores de formato y de consistencia.
- **Qué acepté y por qué:** Acepté el uso de un login básico con identidad desde la sesión del servidor; dos sanciones separadas con tipos en un catálogo; corregir las inconsistencias señaladas.
- **Qué rechacé o cambié y por qué:** el agente había dejado A-11 como "elegir el encargado al iniciar"; fui yo quien señaló que contradecía la táctica de autorizar al actor. La parte del tipo de error 403/404 de esa táctica no se aplica porque todos los encargados ven los mismos datos (A-15).
- **Qué quedó sin verificar:** los tipos concretos de sanción (A-16); cómo se modelan "en mantenimiento" y "bloqueado por novedad"; ninguna regla está implementada ni probada.
