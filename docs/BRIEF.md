# Brief del proyecto: Préstamo de equipos de laboratorio

> Estado: borrador v1 (2026-11-23). Los números de los escenarios de calidad son propuestas para validar; cada cambio se registra en `ASSUMPTIONS.md` o en un ADR.

## 1. Contexto y problema
El laboratorio de la facultad presta equipos (portátiles, osciloscopios, kits de sensores) a estudiantes y docentes. Hoy el control se lleva en un cuaderno y se pierden equipos: no se sabe quién tiene qué, qué está vencido ni qué equipo está dañado o en mantenimiento.

**Objetivo:** un sistema que registre préstamos y devoluciones, haga cumplir las reglas del laboratorio y responda en cualquier momento "¿qué préstamos están vencidos y cuántos días de atraso llevan?".

## 2. Actores
| Actor | Qué hace con el sistema |
|---|---|
| Encargados del laboratorio (varios) | Registran equipos, préstamos, devoluciones y novedades; liberan equipos bloqueados; consultan vencidos. Cada acción queda ligada al encargado que la hizo (A-01). |
| Solicitante (estudiante o docente) | Es la persona que recibe el equipo. No opera el sistema (A-01). |

## 3. Alcance
**Dentro:** persistencia de datos, interfaz para operar el sistema, autenticación básica por encargado (usuario y contraseña con hash), las 4 reglas de negocio y una consulta de préstamos vencidos.
**Fuera:** autenticación avanzada (roles, recuperación de contraseña, OAuth), pagos reales, despliegue en producción, integraciones externas, renovación de préstamos.
**Segundo incremento (después de las 4 reglas del enunciado):** sanciones al solicitante por devolución tardía o con novedad (A-08, A-09) y su catálogo de tipos de sanción (A-14).

## 4. Requisitos funcionales
| ID | Requisito |
|---|---|
| RF-01 | El sistema debe permitir gestionar categorías de equipo, cada una con su plazo de préstamo en días. |
| RF-02 | El sistema debe permitir gestionar equipos (código único, categoría, y su condición: en mantenimiento o bloqueado por novedad). "Disponible" no se guarda: se deriva. |
| RF-03 | El sistema debe permitir gestionar solicitantes. |
| RF-04 | El sistema debe permitir registrar un préstamo de un equipo a un solicitante, calculando la fecha límite según la categoría. |
| RF-05 | El sistema debe permitir registrar la devolución de un préstamo, con o sin novedad (daño, faltante). |
| RF-06 | El sistema debe permitir marcar un equipo en mantenimiento y sacarlo de mantenimiento. |
| RF-07 | El sistema debe permitir liberar un equipo bloqueado por novedad. |
| RF-08 | El sistema debe permitir consultar equipos disponibles. |
| RF-09 | El sistema debe permitir listar préstamos vencidos a una fecha, con días de atraso. |
| RF-10 | El sistema debe permitir gestionar encargados con usuario y contraseña, iniciar sesión y registrar qué encargado hizo cada préstamo, devolución y liberación, con fecha y hora. La identidad sale de la sesión del servidor. |

## 5. Reglas de negocio
| ID | Regla |
|---|---|
| RN-01 | Un solicitante con un préstamo vencido sin devolver no puede pedir otro equipo. |
| RN-02 | Un equipo en mantenimiento no aparece como disponible, aunque esté físicamente en el almacén. |
| RN-03 | El plazo de préstamo depende de la categoría del equipo, no del solicitante. |
| RN-04 | Una devolución con novedad bloquea el equipo hasta que alguien lo libere. |

## 6. Atributos de calidad (escenarios medibles)
Formato: fuente → estímulo → artefacto → entorno → respuesta → **medida**.

| ID | Atributo | Escenario | Medida (propuesta) |
|---|---|---|---|
| QA-01 | Integridad / consistencia | Dos encargados intentan prestar el mismo equipo a la vez, o un solicitante moroso pide otro equipo; el sistema rechaza con un error claro. | 100 % de los intentos inválidos rechazados; nunca dos préstamos activos del mismo equipo. |
| QA-02 | Modificabilidad | El laboratorio cambia el plazo de una categoría. | 0 archivos de código modificados (es un dato, no una constante). |
| QA-03 | Desempeño | El encargado consulta los préstamos vencidos con 10.000 préstamos registrados. | p95 < 500 ms en ejecución local. |
| QA-04 | Testeabilidad | Un desarrollador verifica las 4 reglas de negocio. | Se prueban sin levantar HTTP ni la interfaz; 1 prueba por regla como mínimo. |
| QA-05 | Trazabilidad | Se pierde un equipo y se necesita saber quién lo tuvo y qué encargado lo entregó y lo recibió. | Historial completo del equipo reconstruible desde la BD, con solicitante, encargado y fecha de cada movimiento. |
| QA-06 | Seguridad | Un encargado intenta registrar un préstamo a nombre de otro enviando otro `encargado_id`, o una persona sin sesión intenta operar. | El servidor ignora el dato del cliente y usa la sesión; sin sesión responde 401. 0 registros con un encargado distinto al de la sesión; 0 credenciales en el repo; contraseñas solo con hash. |

## 7. Stack provisional (cada elección se justifica en un ADR)
- Backend: Java, Spring Boot, Maven.
- Base de datos: PostgreSQL.
- Contrato: OpenAPI / Swagger (springdoc).
- Front: HTML, CSS y JavaScript sencillo; Bootstrap o Tailwind por decidir.
- Arquitectura: monolito en capas (controller, service, repository), sin colas ni microservicios.

## 8. Preguntas abiertas (pasan a `ASSUMPTIONS.md`)
- ¿Cómo se modelan "en mantenimiento" y "bloqueado por novedad": columnas, tabla de eventos o derivado? (ADR del modelo de datos)
- ¿Qué tipos concretos de sanción existen y cuáles genera el sistema solo? (A-16, incremento 2)
- ¿Bootstrap, Tailwind o CSS propio para el front? (ADR del front)
- ¿Qué herramienta de IA sirve de respaldo? (ADR de herramienta de IA)

Las preguntas anteriores (vencido, préstamos por solicitante, renovación, quién libera, mantenimiento con préstamo activo) ya están resueltas en `ASSUMPTIONS.md`.

## 9. Criterio de éxito
Un flujo completo funcionando de principio a fin (registrar equipo → prestar → devolver con novedad → liberar → consultar vencidos), con las 4 reglas probadas y un README que permita ejecutarlo desde cero en otra máquina.
