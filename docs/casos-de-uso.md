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

## CU-01 — Consultar Stock de Producto

| Campo | Detalle |
| --- | --- |
| Identificador | CU-01 |
| Nombre | Consultar Stock de Producto |
| Descripción | Permite al usuario consultar la cantidad disponible y el estado de inventario de un producto específico mediante un comando en Telegram. |
| Actores | Principal: Usuario Autorizado (Dueño o Encargado) / Secundario: Loyverse API |
| Precondiciones | Usuario autenticado en el sistema (CU-07). |
| Postcondiciones | Éxito: Se entrega al chat de Telegram del usuario la información de stock actualizada sin modificar los datos almacenados en Loyverse. / Fallo: Se notifica el motivo del error sin alterar el estado del sistema. |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
| --- | --- | --- |
| 1 | El usuario envía el comando `/stock <producto>` por Telegram. | El sistema valida la autorización del usuario (CU-07) e interpreta el comando recibido (CU-08). |
| 2 | El sistema procesa la solicitud. | El sistema realiza la consulta a la API de Loyverse para obtener el stock del producto solicitado (CU-09). |
| 3 | Loyverse retorna la información de inventario. | El bot de Telegram responde al usuario detallando el nombre del producto, la categoría y la cantidad disponible. |

### Excepciones

| # | Situación | Respuesta del sistema |
| --- | --- | --- |
| E1 | Producto no encontrado en Loyverse | El bot responde: "Producto no encontrado. Verifique la descripción o código." |
| E2 | Timeout o error 500/503 en la API de Loyverse | El sistema reintenta 1 vez; si persiste la falla, responde: "Servicio de consulta no disponible temporalmente. Intente más tarde." |
| E3 | Comando ingresado sin especificar producto | El bot responde: "Debe indicar un producto. Ejemplo: /stock Coca Cola 500ml." |
| E4 | Búsqueda devuelve múltiples coincidencias | El bot lista hasta 5 coincidencias solicitando al usuario seleccionar o precisar la búsqueda. |

| Campo | Detalle |
| --- | --- |
| Rendimiento | Tiempo total ≤ 15s (RNF-03); consulta a API Loyverse ≤ 5s |
| Frecuencia | 40 a 60 consultas/día (picos de 10 consultas/hora entre las 07:00 y 09:00 hs) |
| Importancia | Alta |
| Urgencia | Alta |

---

## CU-02 — Consultar Ventas por Período

| Campo | Detalle |
| --- | --- |
| Identificador | CU-02 |
| Nombre | Consultar Ventas por Período |
| Descripción | Permite al usuario visualizar los montos acumulados y el volumen de transacciones comerciales para un período predefinido (día, semana o mes). |
| Actores | Principal: Usuario Autorizado (Dueño o Encargado) / Secundario: Loyverse API |
| Precondiciones | Usuario autenticado en el sistema (CU-07). |
| Postcondiciones | Éxito: El usuario obtiene el resumen de ventas solicitado en su interfaz de Telegram. / Fallo: Se notifica el error sin modificar registros. |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
| --- | --- | --- |
| 1 | El usuario envía el comando `/ventas <día|semana|mes>` por Telegram. | El sistema valida al usuario (CU-07) e interpreta el parámetro de período (CU-08). |
| 2 | El sistema procesa la petición de transacciones. | El sistema realiza la consulta de ventas a la API de Loyverse (CU-09). |
| 3 | Loyverse entrega las transacciones del período. | El bot de Telegram responde con el total facturado, la cantidad de operaciones y el ticket promedio. |

### Excepciones

| # | Situación | Respuesta del sistema |
| --- | --- | --- |
| E1 | Sin ventas registradas en el período | El bot responde: "Sin ventas registradas en el período seleccionado." |
| E2 | Parámetro de período no reconocido | El bot responde: "Período no reconocido. Utilice: /ventas dia, /ventas semana o /ventas mes." |
| E3 | Falla de conectividad con Loyverse | El bot notifica: "No se pudieron sincronizar las ventas en este momento." |

| Campo | Detalle |
| --- | --- |
| Rendimiento | Tiempo total ≤ 15s (RNF-03) |
| Frecuencia | 15 a 20 consultas/día |
| Importancia | Alta |
| Urgencia | Media |

---

## CU-03 — Consultar Productos Más Vendidos

| Campo | Detalle |
| --- | --- |
| Identificador | CU-03 |
| Nombre | Consultar Productos Más Vendidos |
| Descripción | Genera un ranking con los productos que presentan mayor cantidad de unidades vendidas en el negocio. |
| Actores | Principal: Usuario Autorizado (Dueño o Encargado) / Secundario: Loyverse API |
| Precondiciones | Usuario autenticado en el sistema (CU-07). |
| Postcondiciones | Éxito: Se despliega el listado ordenado (Top) en el chat de Telegram. / Fallo: Se reporta la imposibilidad de generar el ranking. |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
| --- | --- | --- |
| 1 | El usuario envía el comando `/top <cantidad>` por Telegram. | El sistema valida el acceso (CU-07) y procesa el número límite solicitado (CU-08). |
| 2 | El sistema solicita los datos acumulados. | El sistema obtiene el historial comercial desde Loyverse (CU-09). |
| 3 | Loyverse retorna el volumen de ventas. | El sistema calcula, ordena y envía mediante el bot el ranking de productos con sus unidades vendidas. |

### Excepciones

| # | Situación | Respuesta del sistema |
| --- | --- | --- |
| E1 | Cantidad solicitada excede los productos del catálogo | El bot entrega el máximo posible agregando la nota: "Se muestran los X productos disponibles en catálogo." |
| E2 | Sin datos históricos suficientes en Loyverse | El bot responde: "Datos insuficientes para generar el ranking comercial." |
| E3 | Parámetro ingresado no numérico | El bot indica: "Ingrese un número entero válido mayor a 0 (Ejemplo: /top 5)." |

| Campo | Detalle |
| --- | --- |
| Rendimiento | Tiempo total ≤ 15s (RNF-03) |
| Frecuencia | 5 a 10 consultas/día |
| Importancia | Media |
| Urgencia | Media |

---

## CU-04 — Consultar Resumen del Negocio

| Campo | Detalle |
| --- | --- |
| Identificador | CU-04 |
| Nombre | Consultar Resumen del Negocio |
| Descripción | Proporciona un panel rápido de métricas consolidadas del comercio (total de productos, ventas del día y movimientos recientes). |
| Actores | Principal: Usuario Autorizado (Dueño o Encargado) / Secundario: Loyverse API |
| Precondiciones | Usuario autenticado en el sistema (CU-07). |
| Postcondiciones | Éxito: Se entrega el panel consolidado en Telegram. / Fallo: Se notifica la inconsistencia o indisponibilidad de datos. |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
| --- | --- | --- |
| 1 | El usuario envía el comando `/resumen` por Telegram. | El sistema valida las credenciales (CU-07) e interpreta la solicitud (CU-08). |
| 2 | El sistema requiere métricas globales. | El sistema extrae la información general acumulada desde Loyverse (CU-09). |
| 3 | Loyverse responde con los indicadores clave. | El bot responde con un mensaje estructurado que resume cantidad de productos, ventas diarias y últimas transacciones. |

### Excepciones

| # | Situación | Respuesta del sistema |
| --- | --- | --- |
| E1 | Sin movimientos registrados en la jornada | El bot reemplaza la sección de transacciones por la leyenda: "Sin movimientos registrados en el día de hoy." |
| E2 | Datos incompletos por sincronización parcial | El bot genera el resumen agregando la advertencia: "Información incompleta al momento de la consulta." |

| Campo | Detalle |
| --- | --- |
| Rendimiento | Tiempo total ≤ 15s (RNF-03) |
| Frecuencia | 10 a 15 consultas/día |
| Importancia | Alta |
| Urgencia | Media |

---

## CU-05 — Generar Reporte

| Campo | Detalle |
| --- | --- |
| Identificador | CU-05 |
| Nombre | Generar Reporte |
| Descripción | Consolida e informa un reporte sobre ventas, comportamiento de stock e indicadores del comercio para un rango de fechas. |
| Actores | Principal: Usuario Autorizado (Dueño o Encargado) / Secundario: Loyverse API |
| Precondiciones | Usuario autenticado (CU-07); existen datos registrados en el período solicitado. |
| Postcondiciones | Éxito: El reporte consolidado es transmitido al chat de Telegram del usuario. / Fallo: Se notifica la restricción o falla en la consolidación. |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
| --- | --- | --- |
| 1 | El usuario envía `/reporte <fecha_inicio> <fecha_fin>` por Telegram. | El sistema valida al usuario (CU-07) e interpreta el rango de fechas (CU-08). |
| 2 | El sistema solicita la información detallada. | El sistema realiza la extracción de datos masivos desde Loyverse (CU-09). |
| 3 | Loyverse devuelve los datos requeridos. | El sistema compila el informe y el bot entrega el cuerpo formateado al usuario. |

### Excepciones

| # | Situación | Respuesta del sistema |
| --- | --- | --- |
| E1 | Sin movimientos registrados en el período | El bot responde: "Sin movimientos en el período solicitado." |
| E2 | Rango de fechas inválido o formato erróneo | El bot indica: "Rango de fechas inválido. Formato requerido: DD/MM/AAAA DD/MM/AAAA." |
| E3 | Volumen excede el límite (> 1000 registros) | El bot envía un reporte parcial con el mensaje: "Reporte truncado por volumen. Solicite un rango menor." |
| E4 | Restricción por límite de peticiones (Rate-Limit) | El bot notifica: "Reporte en cola de procesamiento, se enviará en breve." |

| Campo | Detalle |
| --- | --- |
| Rendimiento | Tiempo total ≤ 15s para hasta 1000 registros (RNF-03) |
| Frecuencia | 3 a 6 reportes/día (concentrados entre las 20:00 y las 23:00 hs) |
| Importancia | Media |
| Urgencia | Media |

---

## CU-06 — Recibir Alerta de Stock Bajo

| Campo | Detalle |
| --- | --- |
| Identificador | CU-06 |
| Nombre | Recibir Alerta de Stock Bajo |
| Descripción | Supervisa periódicamente el inventario y notifica automáticamente al usuario cuando un producto alcanza o supera su límite mínimo configurado. |
| Actores | Principal: Temporizador del Sistema (Scheduler/Cron) / Secundario: Usuario Autorizado (Receptor), Loyverse API |
| Precondiciones | Existen productos en Loyverse con un umbral de stock mínimo asignado mayor a cero. |
| Postcondiciones | Éxito: Se entrega la notificación de advertencia por Telegram al usuario y se registra la alerta enviada. / Fallo: Se omite la notificación y se registra el evento en logs. |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
| --- | --- | --- |
| 1 | El Temporizador del Sistema dispara la revisión automática programada. | El sistema consulta el estado actual del inventario a la API de Loyverse (CU-09). |
| 2 | Loyverse entrega el listado de productos y sus cantidades. | El sistema filtra los artículos con `stock_actual <= stock_minimo`. |
| 3 | El sistema confirma la presencia de ítems críticos. | El bot de Telegram envía automáticamente la notificación preventiva al Usuario Autorizado. |

### Excepciones

| # | Situación | Respuesta del sistema |
| --- | --- | --- |
| E1 | Ningún producto está por debajo del mínimo | El proceso finaliza en silencio registrando únicamente el evento en el log interno. |
| E2 | Producto configurado como inactivo/descontinuado | El sistema omite la alerta para dicho ítem. |
| E3 | Alerta idéntica enviada previamente en la jornada | El sistema omite el reenvío para evitar la saturación de mensajes al usuario. |

| Campo | Detalle |
| --- | --- |
| Rendimiento | Detección ≤ 5s desde la ejecución del ciclo; entrega en Telegram ≤ 15s total |
| Frecuencia | Verificación programada cada 2 horas (dentro de la ventana de 07:00 a 23:00 hs) |
| Importancia | Alta |
| Urgencia | Alta |

---

## CU-07 — Validar Usuario Autorizado

| Campo | Detalle |
| --- | --- |
| Identificador | CU-07 |
| Nombre | Validar Usuario Autorizado |
| Descripción | Incluido por los casos de uso transaccionales (`<<include>>`) para verificar si el ID del emisor coincide con el personal registrado. |
| Actores | Principal: Sistema SN (Módulo de Seguridad) / Secundario: Telegram API |
| Precondiciones | El sistema recibe una solicitud entrante enviada desde Telegram. |
| Postcondiciones | Éxito: Se autentica la identidad permitiendo continuar el flujo invocador. / Fallo: Se deniega el acceso e interrumpe inmediatamente el flujo. |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
| --- | --- | --- |
| 1 | Telegram envía la trama del mensaje recibido. | El sistema extrae el identificador único del emisor (`user_id`). |
| 2 | El sistema procesa la validación. | El sistema compara el ID contra la lista autorizada, confirmando la identidad y dando paso al proceso principal. |

### Excepciones

| # | Situación | Respuesta del sistema |
| --- | --- | --- |
| E1 | Identificador de usuario no registrado | El bot responde: "Acceso no autorizado" y aborta la ejecución inmediatamente (RF-10 / RNF-04). |

| Campo | Detalle |
| --- | --- |
| Rendimiento | Tiempo de comprobación ≤ 1s |
| Frecuencia | Presente en el 100% de los mensajes recibidos (~80 a 100 ejecuciones/día) |
| Importancia | Alta |
| Urgencia | Alta |

---

## CU-08 — Procesar Solicitud vía n8n

| Campo | Detalle |
| --- | --- |
| Identificador | CU-08 |
| Nombre | Procesar Solicitud vía n8n |
| Descripción | Incluido por los CUs de usuario (`<<include>>`) para interpretar el comando, identificar la intención (*intent*) y derivar la ejecución en n8n. |
| Actores | Principal: Sistema SN (Motor n8n) / Secundario: Ninguno |
| Precondiciones | Usuario verificado correctamente en el módulo de seguridad (CU-07). |
| Postcondiciones | Éxito: La solicitud queda interpretada y dirigida al subflujo de negocio correcto. / Fallo: Se redirige al manejo de error o comando no válido. |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
| --- | --- | --- |
| 1 | El sistema recibe la estructura del comando transmitida por Telegram. | n8n parsea el texto e identifica la intención del usuario (*intent*). |
| 2 | n8n valida la coincidencia de la orden. | n8n deriva la ejecución al subflujo de automatización correspondiente. |

### Excepciones

| # | Situación | Respuesta del sistema |
| --- | --- | --- |
| E1 | Comando o intent no reconocido | Se activa la relación de extensión (`<<extend>>`) hacia el CU-10 (Notificar Comando Inválido). |
| E2 | Error de ejecución en un nodo del flujo n8n | n8n captura el error y el bot responde: "Error al procesar la solicitud, intente nuevamente." |

| Campo | Detalle |
| --- | --- |
| Rendimiento | Tiempo de procesamiento interno ≤ 10s |
| Frecuencia | Equivalente al total de comandos procesados (~80 a 100 ejecuciones/día) |
| Importancia | Alta |
| Urgencia | Alta |

---

## CU-09 — Obtener Datos de Loyverse

| Campo | Detalle |
| --- | --- |
| Identificador | CU-09 |
| Nombre | Obtener Datos de Loyverse |
| Descripción | Incluido por los CUs transaccionales (`<<include>>`) para gestionar la integración REST API con Loyverse y recuperar información. |
| Actores | Principal: Sistema SN (Integrador API) / Secundario: Loyverse API |
| Precondiciones | Credenciales de API (Token Bearer) válidas y configuradas en el entorno. |
| Postcondiciones | Éxito: Los datos requeridos se estructuran en JSON y se entregan al flujo invocador. / Fallo: Se notifica la falla de integración al flujo de trabajo. |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
| --- | --- | --- |
| 1 | n8n construye la petición HTTP GET con las credenciales de autorización. | El sistema envía la solicitud al endpoint correspondiente de Loyverse. |
| 2 | Loyverse procesa la petición y responde exitosamente (HTTP 200 OK). | n8n mapea la respuesta JSON y entrega los datos al flujo activo. |

### Excepciones

| # | Situación | Respuesta del sistema |
| --- | --- | --- |
| E1 | Token de API expirado o no válido (HTTP 401/403) | El sistema intenta renovar credenciales; de no ser posible, cancela la consulta y notifica error de integración. |
| E2 | Timeout de red (> 5s) o error en servidor (HTTP 500/503) | El sistema marca los datos como no disponibles y notifica la falla de sincronización al flujo invocador. |

| Campo | Detalle |
| --- | --- |
| Rendimiento | Tiempo de respuesta de API ≤ 5s (RNF-03) |
| Frecuencia | 100 a 130 llamadas/día (asociada a cada CU que requiere sincronizar información) |
| Importancia | Alta |
| Urgencia | Alta |

---

## CU-10 — Notificar Comando Inválido

| Campo | Detalle |
| --- | --- |
| Identificador | CU-10 |
| Nombre | Notificar Comando Inválido |
| Descripción | Extiende del CU-08 (`<<extend>>`) cuando una orden recibida no coincide con ningún comando o intención predefinida. |
| Actores | Principal: Usuario Autorizado (Dueño o Encargado) / Secundario: Ninguno |
| Precondiciones | El análisis de intención realizado en el CU-08 no arrojó ninguna coincidencia válida. |
| Postcondiciones | Éxito: El usuario recibe un mensaje de ayuda y orientación en Telegram. / Fallo: N/A |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
| --- | --- | --- |
| 1 | n8n confirma que la orden ingresada no coincide con un flujo válido (CU-08). | n8n prepara el mensaje de sugerencia. |
| 2 | El sistema envía la respuesta. | El bot responde: "Comando no válido, use /ayuda para ver las opciones disponibles." |

### Excepciones

| # | Situación | Respuesta del sistema |
| --- | --- | --- |
| E1 | Comando ingresado con alta similitud a uno válido | El bot sugiere la opción correcta (ej. si ingresa `/stok`, responde: "Comando no reconocido. ¿Quiso decir /stock?"). |

| Campo | Detalle |
| --- | --- |
| Rendimiento | Tiempo de respuesta ≤ 15s (RNF-03) |
| Frecuencia | 5% a 10% del total de comandos procesados diariamente |
| Importancia | Baja |
| Urgencia | Baja |

---

## CU-11 — Monitorear Flujos n8n

| Campo | Detalle |
| --- | --- |
| Identificador | CU-11 |
| Nombre | Monitorear Flujos n8n |
| Descripción | Permite al Administrador del Sistema supervisar ejecuciones, auditar logs de auditoría y corregir fallos en los flujos de n8n. |
| Actores | Principal: Administrador del Sistema / Secundario: Ninguno |
| Precondiciones | Acceso con credenciales administrativas a la consola de n8n. |
| Postcondiciones | Éxito: Los flujos quedan auditados y ajustados garantizando la estabilidad operativa. / Fallo: Se realiza el rollback de cambios si es necesario. |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
| --- | --- | --- |
| 1 | El Administrador del Sistema accede al panel de n8n. | El sistema muestra el historial de ejecuciones con su estado (éxito/error). |
| 2 | El Administrador del Sistema inspecciona los flujos con fallos reportados. | El sistema visualiza la estructura y datos de entrada/salida de cada nodo. |
| 3 | El Administrador del Sistema aplica correcciones y guarda los cambios. | El sistema actualiza y habilita la nueva versión del flujo automatizado. |

### Excepciones

| # | Situación | Respuesta del sistema |
| --- | --- | --- |
| E1 | Flujo no documentado detectado | Se marca el flujo como pendiente de estandarización antes de aplicar modificaciones (RNF-08). |
| E2 | Regresión o quiebre de flujo tras modificación | Se realiza la reversión (rollback) inmediata a la última versión estable (RNF-06). |

| Campo | Detalle |
| --- | --- |
| Rendimiento | Tarea de mantenimiento administrativa (sin restricción transaccional en tiempo real) |
| Frecuencia | Revisión semanal rutinaria o ante reporte de incidentes técnicos |
| Importancia | Media |
| Urgencia | Media |

---

## CU-12 — Mantener Infraestructura del Sistema

| Campo | Detalle |
| --- | --- |
| Identificador | CU-12 |
| Nombre | Mantener Infraestructura del Sistema |
| Descripción | Engloba las actividades de mantenimiento preventivo, copias de seguridad y verificación de conectividad del entorno. |
| Actores | Principal: Administrador del Sistema / Secundario: Ninguno |
| Precondiciones | Credenciales administrativas sobre servidores, webhooks e integraciones. |
| Postcondiciones | Éxito: El sistema opera dentro del nivel de disponibilidad definido (07:00 a 23:00 hs). / Fallo: Se activan planes de contingencia. |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
| --- | --- | --- |
| 1 | El Administrador del Sistema verifica el estado de n8n, webhooks y API Keys. | El sistema despliega indicadores sobre el estado de las conexiones. |
| 2 | El Administrador del Sistema aplica parches de actualización o respaldos. | El sistema registra el evento de mantenimiento y consolida la operación. |

### Excepciones

| # | Situación | Respuesta del sistema |
| --- | --- | --- |
| E1 | Ocurrencia de caída fuera del horario de servicio (23:01 a 06:59 hs) | No genera incidente crítico (RNF-02), programándose su solución antes de la apertura (07:00 hs). |
| E2 | Caída prolongada de la API de Loyverse en horario operativo | Se notifica preventivamente al Dueño/Encargado sobre la interrupción del servicio externo. |

| Campo | Detalle |
| --- | --- |
| Rendimiento | Tarea de mantenimiento administrativa |
| Frecuencia | Quincenal / Mensual o a demanda ante contingencias técnicas |
| Importancia | Alta |
| Urgencia | Media |

| Campo | Detalle |
|-------|---------|
| Rendimiento | |
| Frecuencia | |
| Importancia | |
| Urgencia | |
