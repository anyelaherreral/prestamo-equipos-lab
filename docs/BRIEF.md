# Brief del proyecto: Préstamo de equipos de laboratorio

> Estado: borrador v0. Los números de los escenarios de calidad son propuestas para validar; cada cambio se registra en `ASSUMPTIONS.md` o en un ADR.

## 1. Contexto y problema
El laboratorio de la facultad presta equipos (portátiles, osciloscopios, kits de sensores) a estudiantes y docentes. Hoy el control se lleva en un cuaderno y se pierden equipos: no se sabe quién tiene qué, qué está vencido ni qué equipo está dañado o en mantenimiento.

**Objetivo:** un sistema que registre préstamos y devoluciones, haga cumplir las reglas del laboratorio y responda en cualquier momento "¿qué préstamos están vencidos y cuántos días de atraso llevan?".

## 2. Actores
| Actor | Qué hace con el sistema |
|---|---|
| Encargados del laboratorio (varios) | Registran equipos, préstamos, devoluciones y novedades; liberan equipos bloqueados; consultan vencidos. Cada acción queda ligada al encargado que la hizo (A-01). |
| Solicitante (estudiante o docente) | Es la persona que recibe el equipo. No opera el sistema (A-01). |

## 3. Alcance
**Dentro:** persistencia de datos, interfaz para operar el sistema, las 4 reglas de negocio y una consulta de préstamos vencidos.
**Fuera:** autenticación avanzada, pagos reales, despliegue en producción, integraciones externas, renovación de préstamos.
**Segundo incremento (después de las 4 reglas del enunciado):** sanciones al solicitante por devolución tardía o con novedad (A-08, A-09).

## 4. Requisitos funcionales
| ID | Requisito |
|---|---|
| RF-01 | El sistema debe permitir gestionar categorías de equipo, cada una con su plazo de préstamo en días. |
| RF-02 | El sistema debe permitir gestionar equipos (categoría, estado: disponible / en mantenimiento / bloqueado por novedad). |
| RF-03 | El sistema debe permitir gestionar solicitantes. |
| RF-04 | El sistema debe permitir registrar un préstamo de un equipo a un solicitante, calculando la fecha límite según la categoría. |
| RF-05 | El sistema debe permitir registrar la devolución de un préstamo, con o sin novedad (daño, faltante). |
| RF-06 | El sistema debe permitir marcar un equipo en mantenimiento y sacarlo de mantenimiento. |
| RF-07 | El sistema debe permitir liberar un equipo bloqueado por novedad. |
| RF-08 | El sistema debe permitir consultar equipos disponibles. |
| RF-09 | El sistema debe permitir listar préstamos vencidos a una fecha, con días de atraso. |
| RF-10 | El sistema debe permitir gestionar encargados y registrar qué encargado hizo cada préstamo, devolución y liberación, con fecha y hora. |

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
| QA-04| Trazabilidad | Se pierde un equipo y se necesita saber quién lo tuvo y qué encargado lo entregó y lo recibió. | Historial completo del equipo reconstruible desde la BD, con solicitante, encargado y fecha de cada movimiento. |


## 7. Stack provisional (cada elección se justifica en un ADR)
- Backend: Java, Spring Boot, Maven.
- Base de datos: PostgreSQL.
- Contrato: OpenAPI / Swagger (springdoc).
- Front: HTML, CSS y JavaScript sencillo; Bootstrap o Tailwind por decidir.
- Arquitectura: monolito en capas (controller, service, repository), sin colas ni microservicios.

## 8. Preguntas abiertas (pasan a `ASSUMPTIONS.md`)
- ¿Qué significa "vencido": pasada la fecha límite o al día siguiente?
- ¿Un solicitante puede tener varios préstamos activos?
- ¿Se permite renovar un préstamo?
- ¿Quién puede liberar un equipo bloqueado?
- ¿Un equipo en mantenimiento puede estar prestado?

## 9. Criterio de éxito
Un flujo completo funcionando de principio a fin (registrar equipo → prestar → devolver con novedad → liberar → consultar vencidos), con las 4 reglas probadas y un README que permita ejecutarlo desde cero en otra máquina.
