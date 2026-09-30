# Casos de uso

## Diagrama general

_Incluir el código PlantUML en `diagramas/casos-de-uso.puml`._
_Visualizar en [plantuml.com](https://www.plantuml.com/plantuml/uml/)._

_Describir brevemente los actores identificados y las relaciones principales (include, extend)._

---

## CU-01 — [Nombre]

| Campo | Detalle |
|-------|---------|
| Identificador | CU-01 |
| Nombre | |
| Descripción | |
| Actores | Principal: / Secundario: |
| Precondiciones | |
| Postcondiciones | Éxito: / Fallo: |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
|---|----------------|--------------------|
| 1 | | |
| 2 | | |

### Excepciones

| # | Situación | Respuesta del sistema |
|---|-----------|-----------------------|
| E1 | | |

| Campo | Detalle |
|-------|---------|
| Rendimiento | |
| Frecuencia | |
| Importancia | |
| Urgencia | |

---

## CU-02 — [Nombre]

| Campo | Detalle |
|-------|---------|
| Identificador | CU-02 |
| Nombre | |
| Descripción | |
| Actores | Principal: / Secundario: |
| Precondiciones | |
| Postcondiciones | Éxito: / Fallo: |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
|---|----------------|--------------------|
| 1 | | |
| 2 | | |

### Excepciones

| # | Situación | Respuesta del sistema |
|---|-----------|-----------------------|
| E1 | | |

CU-01 Consultar Stock de Producto

Actor: Usuario Autorizado
Precondición: Usuario validado (UC7)
Flujo básico:

Usuario envía /stock <producto>.
n8n valida y procesa (UC8).
Se consulta Loyverse (UC9).
Bot responde cantidad disponible.

Excepciones:

E1: Producto no existe → "Producto no encontrado" (404 API).
E2: Loyverse timeout/error 500 → reintento x1, luego "Servicio no disponible".
E3: Comando sin producto → "Debe indicar un producto".
E4: Nombre ambiguo → lista coincidencias y pide precisar.

Rendimiento: ≤15s total (RNF-02); consulta Loyverse ≤5s (RNF-03).
Frecuencia: 40-60 consultas/día, picos 10/hora (07-09hs).

CU-02 Consultar Ventas por Período

Actor: Usuario Autorizado
Precondición: Usuario validado (UC7)
Flujo básico:

Usuario envía /ventas <día|semana|mes>.
n8n procesa (UC8) y consulta Loyverse (UC9).
Bot responde total y detalle de ventas.

Excepciones:

E1: Período sin ventas → "Sin ventas registradas en el período".
E2: Parámetro de período inválido → "Período no reconocido, use día/semana/mes".
E3: Loyverse no disponible → mensaje de error temporal.

Rendimiento: ≤15s (RNF-02); datos con demora máx. 5s (RNF-03).
Frecuencia: 15-20 consultas/día.

CU-03 Consultar Productos Más Vendidos

Actor: Usuario Autorizado
Precondición: Usuario validado (UC7)
Flujo básico:

Usuario envía /top <cantidad>.
n8n procesa (UC8) y consulta ventas en Loyverse (UC9).
Bot responde ranking de productos.

Excepciones:

E1: Cantidad solicitada excede catálogo disponible → se devuelve el máximo posible con aviso.
E2: Sin ventas históricas suficientes → "Datos insuficientes para generar ranking".
E3: Parámetro no numérico → "Ingrese un número válido".

Rendimiento: ≤15s (RNF-02).
Frecuencia: 5-10 consultas/día.

CU-04 Consultar Resumen del Negocio

Actor: Usuario Autorizado
Precondición: Usuario validado (UC7)
Flujo básico:

Usuario envía /resumen.
n8n procesa (UC8) y consulta Loyverse (UC9).
Bot responde cantidad de productos, ventas totales y movimientos recientes.

Excepciones:

E1: Falta uno de los indicadores (ej. sin movimientos recientes) → se omite esa sección con aviso "Sin movimientos recientes".
E2: Loyverse con datos parciales → resumen indica "Información incompleta al momento de la consulta".

Rendimiento: ≤15s (RNF-02).
Frecuencia: 10-15 consultas/día.

CU-05 Generar Reporte

Actor: Usuario Autorizado
Precondición: Usuario validado; existen datos en el período
Flujo básico:

Usuario envía /reporte <tipo> <período>.
n8n procesa (UC8) y consulta Loyverse (UC9).
Bot entrega reporte en el chat.

Excepciones:

E1: Sin movimientos → "Sin movimientos en el período solicitado".
E2: Rango de fechas inválido → "Rango de fechas inválido".
E3: >1000 registros → reporte parcial + "Reporte truncado, solicite un rango menor".
E4: Rate-limit de Loyverse → "Reporte en proceso, se enviará en breve".

Rendimiento: ≤15s hasta ~1000 registros (RNF-02); demora datos máx. 5s (RNF-03).
Frecuencia: 3-6 reportes/día, concentrados 20-23hs.

CU-06 Recibir Alerta de Stock Bajo

Actor: n8n (disparador automático)
Precondición: Producto con stock mínimo configurado
Flujo básico:

n8n consulta stock periódicamente (UC9).
Detecta stock ≤ mínimo.
Envía alerta por Telegram con nombre y cantidad.

Excepciones:

E1: Sin mínimo definido → no se genera alerta (log interno).
E2: Producto discontinuado/inactivo → no dispara alerta aunque stock sea 0.

Rendimiento: detección ≤5s desde cambio real (RNF-03); envío ≤15s desde detección.


CU-07 Validar Usuario Autorizado

Actor: Usuario Autorizado (incluido por UC1-UC6)
Precondición: Ninguna
Flujo básico:

Se recibe ID de Telegram del emisor.
Si coincide, continúa el flujo original.

Excepciones:

E1: ID no autorizado → bot responde "Acceso no autorizado" y corta el flujo (RF-08/RNF-05).


Rendimiento: validación ≤1s (subconjunto de los 15s totales).
Frecuencia: en cada interacción (100% de los mensajes recibidos).

CU-08 Procesar Solicitud vía n8n

Actor: n8n (incluido por UC1-UC6)
Precondición: Usuario validado (UC7)
Flujo básico:

n8n recibe el comando parseado.
Identifica el intent (stock, ventas, reporte, etc.).
Ejecuta el flujo correspondiente y devuelve resultado a Telegram.

Excepciones:

E1: Comando no reconocido → extiende a UC10 (notificar comando inválido).
E2: Nodo del flujo falla en ejecución → se responde "Error al procesar la solicitud, intente nuevamente".

Rendimiento: procesamiento interno ≤10s (dejando margen para respuesta Telegram dentro de 15s).
Frecuencia: equivalente al total de comandos recibidos, ~80-100/día.

CU-09 Obtener Datos de Loyverse

Actor: n8n (incluido por UC1, UC2, UC3, UC4, UC5, UC6)
Precondición: Credenciales de API válidas
Flujo básico:

n8n arma la petición a la API de Loyverse.
Loyverse responde con los datos solicitados.
n8n entrega los datos al flujo que lo invocó.

Excepciones:

E1: Token de API expirado → n8n intenta renovar token automáticamente; si falla, se informa error de integración.
E2: Timeout de red (>5s) → se marca dato como "no actualizado" y se informa al usuario si aplica.

Rendimiento: respuesta ≤5s (RNF-03).
Frecuencia: una llamada por cada CU que la incluye, ~100-130/día.

CU-10 Notificar Comando Inválido

Actor: Usuario Autorizado (extiende a UC8)
Precondición: El intent del comando no matchea ningún flujo válido
Flujo básico:

n8n no encuentra coincidencia de intent.
Bot responde "Comando no válido, use /ayuda para ver opciones".

Excepciones:

E1: Comando parcialmente similar a uno válido → se sugiere el comando más cercano (ej. /stok → "¿Quiso decir /stock?").

Rendimiento: respuesta ≤15s (RNF-02).
Frecuencia: estimado 5-10% del total de comandos recibidos/día.

CU-11 Monitorear Flujos n8n

Actor: Administrador del Sistema
Precondición: Acceso al panel de n8n
Flujo básico:

Administrador revisa ejecución y logs de los flujos.
Detecta fallos o cuellos de botella.
Ajusta o corrige el flujo correspondiente.

Excepciones:

E1: Flujo sin documentación (incumple RNF-08) → se marca como pendiente de estandarización antes de modificar.
E2: Cambio en un flujo rompe otro existente → se revierte y se aísla el flujo (RNF-07).

Rendimiento: sin restricción de tiempo real (tarea de mantenimiento, no transaccional).
Frecuencia: revisión estimada 1 vez/semana, o ante incidente reportado.

CU-12 Mantener Infraestructura del Sistema

Actor: Administrador del Sistema
Precondición: Ninguna
Flujo básico:

Administrador supervisa disponibilidad del sistema (RNF-01).
Verifica conexión con Loyverse, Telegram y n8n.
Aplica correcciones o actualizaciones según necesidad.

Excepciones:

E1: Caída fuera del horario 07-23hs → no genera incidente crítico según RNF-01, se resuelve antes de apertura.
E2: Integración con Loyverse caída durante horario operativo → se notifica al administrador de forma prioritaria.

Rendimiento: disponibilidad garantizada 07:00-23:00hs (RNF-01).
Frecuencia: monitoreo continuo/diario.


| Campo | Detalle |
|-------|---------|
| Rendimiento | |
| Frecuencia | |
| Importancia | |
| Urgencia | |
