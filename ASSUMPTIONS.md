# ASSUMPTIONS.md

> Todo lo que el enunciado no dice y tuve que resolver por mi cuenta. Cada asunción lleva su razón y lo que pasa si me equivoco.
> Las razones son un primer borrador: reescríbelas con tus palabras antes de hacer commit.

| ID | Pregunta que el enunciado no responde | Asunción tomada | Por qué | Impacto si me equivoco | Estado |
|---|---|---|---|---|---|
| A-01 | ¿Quién opera el sistema? | Los encargados del laboratorio, que pueden ser varios. Se guarda qué encargado registró cada préstamo y cada devolución, con fecha y hora. El solicitante no entra al sistema. | Con varios encargados hace falta trazabilidad de quién hizo qué y cuándo. Los turnos no se modelan: se derivan de lo que cada uno registró. | Sin la entidad Encargado no habría auditoría de quién entregó cada equipo. | Decidida |
| A-02 | ¿Cuándo está vencido un préstamo? | Desde el día siguiente a la fecha límite. Si el límite es el 10, el 10 aún es válido y el 11 lleva 1 día de atraso. | Evita manejar horas y hace simple el cálculo de días de atraso. | La consulta de vencidos cambiaría en un día. | Decidida |
| A-03 | ¿Cuántos préstamos activos puede tener un solicitante? | Varios a la vez. | Así la regla 1 conserva su sentido propio: se rechaza solo si alguno de sus préstamos está vencido. Con "uno solo" la regla quedaría implícita. | Un tope por solicitante sería una regla nueva que el enunciado no pide. | Decidida |
| A-04 | ¿Se puede renovar un préstamo? | No. Está fuera del alcance. | El enunciado no lo pide y agregaría estados y casos límite. | Habría que agregar la operación y su regla. | Decidida |
| A-05 | ¿Quién libera un equipo bloqueado por novedad? | Cualquier encargado del laboratorio; queda registrado cuál lo hizo. | No hay otros roles (A-01) y la trazabilidad conserva la responsabilidad. | Si fuera un rol aparte, habría que modelar permisos. | Decidida |
| A-06 | ¿Un equipo puede estar en dos préstamos activos a la vez? | No. **Equipo** es cada unidad física con código único (Tensiómetro-1, Tensiómetro-2). La **categoría** es el tipo y define el plazo. Una unidad tiene como máximo un préstamo activo; dos unidades de la misma categoría sí pueden estar prestadas a la vez. | Una unidad física no puede estar en dos manos. Se garantiza con una restricción de la BD. | Sin la restricción, dos encargados podrían prestar la misma unidad. | Decidida |
| A-07 | ¿Se puede pasar a mantenimiento un equipo que está prestado? | No. Solo se envía a mantenimiento después de devolverlo. Si falla durante el préstamo, se registra como novedad en la devolución. | Un equipo prestado se entregó en buen estado; el mantenimiento se decide con el equipo en el laboratorio. | Habría que definir qué pasa con el préstamo activo. | Decidida |
| A-08 | Al devolver tarde, ¿qué ocurre? | Se registra la devolución y el préstamo deja de estar vencido. Se genera una **sanción** al solicitante (la "multa", cuyo contenido fija el laboratorio, fuera del sistema). Mientras la sanción esté vigente, no puede pedir equipos. | Es una regla del laboratorio, no del enunciado; se implementa en el **segundo incremento**. | Si se omite, la tardanza solo se ve en el historial. | Decidida, incremento 2 |
| A-09 | ¿Una devolución con novedad bloquea también al solicitante? | Sí, con una sanción mientras no la cumpla, además del bloqueo del equipo (regla 4). | Misma razón que A-08. | Igual que A-08. | Decidida, incremento 2 |
| A-10 | ¿Un préstamo incluye uno o varios equipos? | Un solo equipo por préstamo, con su propia fecha límite y su propia devolución. | Modelo simple; evita devoluciones parciales y plazos distintos dentro de un mismo préstamo. | Habría que agregar una tabla intermedia. | Decidida |
| A-11 | Sin login, ¿cómo sabe el sistema qué encargado opera? | Pendiente: se resuelve en un ADR (por ejemplo, elegir el encargado al iniciar). Es identificación, no autenticación segura. | | | Abierta |
| A-12 | ¿La sanción lleva un monto de dinero? | No. Solo se registra que existe, su motivo, cuándo se impuso y quién y cuándo la levantó. El estado "vigente" se deriva (aún no levantada). | Los pagos reales están fuera de alcance. | | Propuesta, incremento 2 |
| A-13 | Si la devolución es tardía y además con novedad, ¿una sanción o dos? | Pendiente. | | | Abierta, incremento 2 |

## Cosas que no entiendo del código y quiero revisar
- (Anótalas aquí en vez de esconderlas.)
