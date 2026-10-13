# Historias de usuario

_Presentar al menos una historia de usuario representativa por módulo._
_Cada historia debe incluir formato clásico, criterios de aceptación y validación INVEST._

---

## HU-01 — Consulta de Stock

| Campo | Detalle |
|-------|---------|
| Historia | Como dueño o encargado del comercio, quiero consultar el stock disponible de los productos mediante Telegram, para conocer la disponibilidad de mercadería en tiempo real. |
| Módulo | Consulta de información General |
| Requisitos relacionados | RF-01, RNF-02 |

### Criterios de aceptación

1. El usuario envía un comando de consulta de stock por Telegram y el bot responde con la cantidad disponible del producto solicitado.
2. Si el producto consultado no existe en el sistema, el bot informa que no fue encontrado.
3. El sistema responde a la consulta en un plazo máximo de 15 segundos, según lo definido en RNF-02.

### Validación INVEST

| Criterio | ¿Se cumple? | Observación |
|----------|-------------|-------------|
| Independiente | Si | No depende de que otra HU esté implementada, solo requiere que RF-01 exista.|
| Negociable | Si | El formato del mensaje de respuesta puede acordarse con el equipo sin afectar el objetivo. |
| Valiosa | Si | Evita que el usuario deba abrir Loyverse directamente para saber el stock, ahorrando tiempo operativo. |
| Estimable | Si |  El alcance (una consulta, una respuesta) es acotado y estimable en horas de desarrollo. |
| Pequeña | Si | Se puede implementar y probar en una sola iteración, sin dividirse en tareas menores. |
| Verificable | Si | Se prueba enviando el comando de stock y comprobando que el bot responde con la cantidad real cargada en Loyverse en un máximo de 15 segundos (RNF-02). |

---

## HU-02 — Recepción de Alertas de Stock Bajo

| Campo | Detalle |
|-------|---------|
| Historia | Como dueño o encargado del comercio, quiero recibir alertas automáticas cuando un producto tenga poco stock, para evitar faltantes de mercadería. |
| Módulo | Consulta de Información General |
| Requisitos relacionados | RF-04 |

### Criterios de aceptación

1. El sistema detecta cuando el stock de un producto alcanza el límite mínimo definido para ese producto.
2. Al detectarse, el sistema envía automáticamente una alerta al usuario por Telegram, sin que este deba solicitarla.
3. La alerta indica el nombre del producto y la cantidad de stock restante al momento de dispararse.

### Validación INVEST

| Criterio | ¿Se cumple? | Observación |
|----------|-------------|-------------|
| Independiente |Si | Se dispara sola por verificación de stock, no depende de que el usuario haya hecho una consulta previa. |
| Negociable | Si | El valor del límite mínimo por producto puede ajustarse sin cambiar el objetivo de la historia. |
| Valiosa | Si | Previene una pérdida de venta concreta: evita que el comercio se quede sin stock sin darse cuenta. |
| Estimable | Si | El alcance (detección de umbral + envío de mensaje) está bien delimitado para estimar su desarrollo. |
| Pequeña | Si | Es un flujo automatizado acotado, implementable en una sola iteración. |
| Verificable | Si | Se prueba bajando el stock de un producto de prueba por debajo del mínimo definido y comprobando que la alerta llega por Telegram con el nombre y cantidad correctos (RF-04). |

---

## HU-03 — Recepción de Respuestas Automáticas

| Campo | Detalle |
|-------|---------|
| Historia | Como dueño o encargado del comercio, quiero recibir respuestas automáticas a mis consultas realizadas por Telegram, para obtener información de manera rápida y sencilla. |
| Módulo | Comunicación e Integración |
| Requisitos relacionados | RF-06, RF-07, RNF-02 |

### Criterios de aceptación

1. Ante cualquier consulta válida enviada por Telegram, el bot responde sin intervención manual de un operador.
2. El flujo de n8n procesa la solicitud (RF-07) y genera la respuesta correspondiente dentro del tiempo máximo definido en RNF-02.
3. Si la solicitud no es reconocida, el bot informa al usuario que el comando no es válido, en vez de no responder.

### Validación INVEST

| Criterio | ¿Se cumple? | Observación |
|----------|-------------|-------------|
| Independiente | Si |  Puede desarrollarse y probarse aunque otras HU (como reportes) todavía no estén listas. |
| Negociable | Si | El texto exacto de los mensajes de respuesta puede redefinirse sin alterar el objetivo. |
| Valiosa | Si | Es la base de toda interacción del sistema: sin esto, ninguna otra consulta funciona. |
| Estimable | Si | El flujo en n8n (recibir → procesar → responder) es concreto y estimable. |
| Pequeña | Si | Puede implementarse como una unidad funcional independiente del resto del sistema. |
| Verificable | Si | Se prueba enviando 3 comandos distintos (uno válido conocido, uno inválido, uno vacío) y verificando que cada uno recibe respuesta en menos de 15 segundos (RNF-02). |

---

## HU-04 — Generación de Reporte de Ventas

| Campo | Detalle |
|-------|---------|
| Historia | Como dueño o encargado del comercio, quiero generar un reporte de ventas para un período determinado, para analizar el desempeño comercial. |
| Módulo | Registro y Reportes |
| Requisitos relacionados | RF-11, RF-12, RNF-02, RNF-03 |

### Criterios de aceptación

1. El usuario envía `/reporte ventas <período>` y el bot entrega en el chat el reporte con el total de ventas, la cantidad de registros y el detalle por producto.
2. El reporte se genera con los datos de ventas del sistema, con una demora máxima de 5 segundos respecto de Loyverse (RNF-03).
3. El bot entrega el reporte en un máximo de 15 segundos para hasta 1000 registros (RNF-02).
4. Si el período indicado es inválido, el bot responde "Rango de fechas inválido" y no genera el reporte.
5. Si no hay ventas en el período, el bot responde "Sin movimientos en el período solicitado".
6. Si el período supera los 1000 registros, el bot entrega un reporte parcial e informa "Reporte truncado, solicite un rango menor".

### Validación INVEST

| Criterio | ¿Se cumple? | Observación |
|----------|-------------|-------------|
| Independiente | Sí | Solo requiere que las ventas estén almacenadas (RF-11); no depende de otras historias. |
| Negociable | Sí | El orden y el formato del detalle por producto se pueden acordar sin cambiar el objetivo. |
| Valiosa | Sí | Le permite al dueño ver las ventas de un período sin entrar a Loyverse. |
| Estimable | Sí | El alcance es un solo tipo de reporte, con tres casos de error ya definidos. |
| Pequeña | Sí | Cubre un único tipo de reporte con un solo comando; los reportes de stock y de indicadores quedan como historias aparte. |
| Verificable | Sí | Se prueba con un período de ventas conocidas, comparando el total y la cantidad con Loyverse, midiendo que la respuesta llegue en 15 segundos o menos (RNF-02) y forzando cada uno de los tres errores. |