# Modelo Entidad-Relación

## Diagrama

_Incluir el código PlantUML en `diagramas/er.puml`._
_Visualizar en [plantuml.com](https://www.plantuml.com/plantuml/uml/)._

## Entidades

| Entidad | Descripción | Relaciones clave |
|---------|-------------|-------------------|
| Producto | Representa un producto del comercio, con su stock actual y el límite mínimo configurado para disparar alertas (RF-01, RF-04, RF-10).| 1 — N con Venta; 1 — N con Alerta |
| Venta | Representa una línea de venta: la venta de un producto en una fecha determinada (RF-02, RF-11, CU-02). | N — 1 con Producto |
| Alerta |  Representa una notificación automática generada cuando el stock de un producto alcanza el mínimo definido (RF-04, CU-06). | N — 1 con Producto |
| Usuario | Representa a una persona autorizada a interactuar con el sistema mediante su ID de Telegram (RF-08, RNF-05, CU-07). | Sin relaciones con otras entidades |
 
## Descripción de atributos principales

_Para cada entidad, describir brevemente los atributos más relevantes y su propósito._

### Producto

- `id_producto` (PK): identificador único del producto.
- `nombre`: nombre del producto, usado en consultas y alertas (RF-01).
- `codigo`: código identificador interno del producto; único.
- `categoria`: categoría a la que pertenece, usada en reportes (RF-11).
- `stock_actual`: cantidad disponible actualmente (RF-01, RNF-03).
- `stock_minimo`: umbral que dispara una Alerta al ser alcanzado (RF-04, CU-06).

### Venta

- `id_venta` (PK): identificador único de la línea de venta.
- `id_producto` (FK): producto vendido, referencia a Producto.
- `cantidad`: unidades vendidas en ese registro.
- `monto`: importe correspondiente a esa venta.
- `fecha`: momento en que se registró la venta, usado para agrupar por período (RF-02).

### Alerta

- `id_alerta` (PK): identificador único de la alerta.
- `id_producto` (FK): producto que disparó la alerta, referencia a Producto.
- `fecha_hora`: momento en que se generó la alerta.
- `stock_al_momento`: cantidad de stock registrada al momento de dispararse (CU-06).

### Usuario
- `id_usuario` (PK): identificador único del usuario en el sistema.
- `telegram_id`: identificador de Telegram usado para validar el acceso (RF-08, RNF-05, CU-07); único.
- `nombre`: nombre de referencia del usuario autorizado.

## Decisiones de diseño

### Decisión 1 — Almacenamiento local de Producto y Venta, actualizado por eventos de Loyverse

Producto y Venta se guardan localmente en el SAS y se mantienen actualizados mediante los webhooks de la API de Loyverse (`inventory_levels.update` para el stock y `receipts.update` para las ventas). Cada evento llega a un flujo de n8n, que actualiza la copia local en el momento en que ocurre el cambio. Las consultas del usuario (RF-01, RF-02, RF-03, RF-05) se responden desde esa copia, sin esperar a la API de Loyverse. Esto permite cumplir RNF-03 (datos con una demora máxima de 5 segundos respecto de Loyverse) y RNF-02 (respuesta en 15 segundos), y evita el riesgo de timeout descrito en CU-01/E2. Como respaldo ante un evento perdido, se agrega una sincronización completa de baja frecuencia que corrige diferencias; el cumplimiento de RNF-03 no depende de ella.

Se descartaron dos alternativas: consultar Loyverse en vivo ante cada comando, porque el cumplimiento de RNF-02 quedaría atado a la velocidad y disponibilidad de un servicio externo; y sincronizar solo de forma periódica, porque un intervalo de minutos no cumple los 5 segundos de RNF-03.


### Decisión 2 — Venta como línea individual, no como Transacción con detalle N-M

Se descartó modelar una entidad Transacción vinculada a Producto mediante una tabla intermedia (DetalleVenta), a pesar de que en un POS real una venta pueda incluir varios productos. Ningún RF ni CU del sistema requiere reconstruir el ticket completo de una venta: RF-02, RF-03 y RF-11 solo piden totales y rankings por producto y por período, que se resuelven modelando Venta como una línea individual (un producto, una cantidad, un momento). Agregar esa complejidad no estaría respaldado por ningún requisito.

### Decisión 3 — Usuario sin relación con las entidades comerciales

Se decidió no vincular Usuario con Producto, Venta o Alerta. La función de Usuario en el sistema es exclusivamente autorizar el acceso (RF-08, RNF-05, CU-07), una validación binaria que no distingue acciones entre los usuarios autorizados. Aunque el sistema permite realizar consultas y recibir reportes, estas son interacciones en tiempo real que no generan un dato persistente vinculado a un usuario específico: ningún requisito pide trazabilidad de qué usuario realizó cada consulta. Si en el futuro surgiera ese requisito, se podría incorporar una entidad adicional (por ejemplo, un registro de consultas) sin modificar el resto del modelo.