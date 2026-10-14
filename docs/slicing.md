# Ejercicio: partir una épica en slices verticales

## La épica

> Como dueño o encargado del comercio, quiero generar reportes del negocio desde Telegram,
> para analizar el desempeño comercial sin entrar a Loyverse.

_Así como está, es una épica gorda: no se puede estimar, no se puede terminar en una
iteración, y esconde decisiones que nadie tomó todavía._

---

## Parte A — Historias verticales

_Entre 5 y 8 historias VERTICALES. Vertical significa que cada historia, sola, entrega algo
usable de punta a punta ("diseñar la pantalla de envío" no es vertical; "enviar dinero a un
contacto de la agenda con saldo suficiente" sí lo es)._

### Historia 1 — Reporte de ventas de un período

| Campo | Detalle |
|-------|---------|
| Historia | Como dueño o encargado del comercio, quiero pedir por Telegram el reporte de ventas de un período, para saber cuánto vendí y qué productos se vendieron. |
| Requisitos relacionados | RF-12, RF-11, RNF-02 (corresponde a HU-04, sin el caso de más de 1000 registros, que pasa a la Historia 2) |

**Criterios de aceptación**

1. Con `/reporte ventas <período>`, el bot entrega el total de ventas, la cantidad de registros y el detalle por producto.
2. Si el período es inválido, el bot responde "Rango de fechas inválido" y no genera el reporte.
3. Si no hay ventas en el período, el bot responde "Sin movimientos en el período solicitado".
4. El bot entrega el reporte en un máximo de 15 segundos (RNF-02).

---

### Historia 2 — Reporte de ventas de gran volumen

| Campo | Detalle |
|-------|---------|
| Historia | Como dueño o encargado del comercio, quiero recibir un reporte parcial cuando el período tiene demasiados registros, para recibir igual información útil y saber que tengo que acotar el rango. |
| Requisitos relacionados | RF-12, RNF-02 |
| Depende de | Historia 1 (agrega el caso del período con más de 1000 registros) |

**Criterios de aceptación**

1. Si el período supera los 1000 registros, el bot entrega un reporte parcial de hasta 1000 registros.
2. El bot informa "Reporte truncado, solicite un rango menor".
3. El bot entrega el reporte parcial en un máximo de 15 segundos (RNF-02).

---

### Historia 3 — Reporte de ventas por categoría

| Campo | Detalle |
|-------|---------|
| Historia | Como dueño o encargado del comercio, quiero filtrar el reporte de ventas por categoría de producto, para comparar el rendimiento de cada rubro. |
| Requisitos relacionados | RF-12, RF-10, RF-11 |
| Depende de | Historia 1 (agrega el filtro por categoría al reporte base) |

**Criterios de aceptación**

1. Con `/reporte ventas <período> <categoría>`, el bot entrega solo las ventas de los productos de esa categoría, con total y detalle.
2. Si la categoría no existe, el bot responde "Categoría no encontrada" y no genera el reporte.
3. El bot entrega el reporte en un máximo de 15 segundos (RNF-02).

---

### Historia 4 — Reporte de stock actual

| Campo | Detalle |
|-------|---------|
| Historia | Como dueño o encargado del comercio, quiero pedir el reporte de stock actual, para saber qué productos tengo que reponer. |
| Requisitos relacionados | RF-12, RF-04, RNF-02 |

**Criterios de aceptación**

1. Con `/reporte stock`, el bot entrega la lista de productos con su cantidad disponible.
2. Los productos con stock igual o menor al mínimo configurado aparecen marcados en la lista.
3. El bot entrega el reporte en un máximo de 15 segundos (RNF-02).

---

### Historia 5 — Reporte de indicadores del período

| Campo | Detalle |
|-------|---------|
| Historia | Como dueño o encargado del comercio, quiero ver indicadores resumidos de un período (total vendido, cantidad de ventas y producto más vendido), para evaluar el desempeño del negocio de un vistazo. |
| Requisitos relacionados | RF-12, RF-03, RNF-02 |

**Criterios de aceptación**

1. Con `/reporte indicadores <período>`, el bot entrega el total vendido, la cantidad de ventas y el producto más vendido.
2. Si no hay ventas en el período, el bot responde "Sin movimientos en el período solicitado".
3. El bot entrega los indicadores en un máximo de 15 segundos (RNF-02).

---

## Parte B — Los caminos que no salen bien

_Elijan UNA de las historias de la Parte A. Las últimas tres preguntas son las importantes:
para cada una, indiquen qué debería hacer el sistema y quién tendría que decidirlo._

**Historia elegida:** Historia 1 — Reporte de ventas de un período (HU-04)

| Pregunta | Qué hace el sistema | Quién decide (analista / negocio / técnica) |
|----------|----------------------|-----------------------------------------------|
| ¿Qué pasa si el período indicado es inválido? | El bot responde "Rango de fechas inválido" y no genera el reporte (CU-05, E2). | Analista |
| ¿Qué pasa si no hay ventas en el período? | El bot responde "Sin movimientos en el período solicitado" (CU-05, E1). | Analista |
| ¿Qué pasa si la sincronización con Loyverse está caída y los datos almacenados quedaron desactualizados? | El bot entrega el reporte con los datos almacenados y agrega el aviso "Información posiblemente desactualizada". Además, se notifica al administrador la caída de la integración (CU-09, E2). | Negocio (entregar con aviso o no entregar) y técnica (cómo se detecta la falla) |
| ¿Qué pasa si el usuario envía el mismo comando dos veces? | Como el reporte solo lee datos, repetir el comando no duplica ni daña nada. Si llega mientras el primero se está procesando, el bot responde "Su reporte está en proceso" y no genera otro. | Analista (comportamiento esperado) y técnica (cómo detectar el pedido repetido) |
| ¿Qué pasa si el período supera los 1000 registros? | El bot entrega un reporte parcial de hasta 1000 registros y avisa "Reporte truncado, solicite un rango menor" (CU-05, E3). Esta rama es la Historia 2. | Negocio (el valor del límite) y técnica (que la respuesta entre en 15 segundos, RNF-02) |

---

## Parte C — Defensa

_Se hace oral, en el plenario. No se documenta en este archivo._
