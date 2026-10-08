 ## CU-01 — Consultar Stock de Producto

| Campo | Detalle |
|-------|---------|
| Identificador | CU-01 |
| Nombre | Consultar Stock de Producto |
| Descripción | Permite al usuario autorizado consultar la cantidad disponible de un producto. |
| Actores | Principal: Usuario Autorizado / Secundario: Loyverse |
| Precondiciones | Usuario validado y producto registrado en el sistema de gestión. |
| Postcondiciones | Éxito: Se informa la cantidad disponible / Fallo: Se informa el error correspondiente. |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
|---|----------------|--------------------|
| 1 | Usuario envía `/stock <producto>`. | El sistema valida la autorización del usuario. |
| 2 | — | El sistema procesa la solicitud y consulta la información de stock en Loyverse. |
| 3 | — | El sistema recibe la información y responde la cantidad disponible. |

### Excepciones

| # | Situación | Respuesta del sistema |
|---|-----------|-----------------------|
| E1 | Producto no existe. | Responde "Producto no encontrado". |
| E2 | Loyverse presenta timeout o error 500. | Realiza un reintento y, si falla, informa "Servicio no disponible". |
| E3 | Comando sin producto. | Responde "Debe indicar un producto". |
| E4 | Nombre ambiguo. | Muestra las coincidencias y solicita precisar el producto. |

| Campo | Detalle |
|-------|---------|
| Rendimiento | ≤15s total (RNF-02); consulta a Loyverse ≤5s (RNF-03). |
| Frecuencia | 40-60 consultas/día, con picos de 10/hora (07-09hs). |
| Importancia | Alta. |
| Urgencia | Alta durante el horario operativo. |

---

## CU-02 — Consultar Ventas por Período

| Campo | Detalle |
|-------|---------|
| Identificador | CU-02 |
| Nombre | Consultar Ventas por Período |
| Descripción | Permite al usuario autorizado consultar las ventas realizadas durante un período determinado. |
| Actores | Principal: Usuario Autorizado / Secundario: Loyverse |
| Precondiciones | Usuario validado. |
| Postcondiciones | Éxito: Se informa el total y detalle de las ventas / Fallo: Se informa el error correspondiente. |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
|---|----------------|--------------------|
| 1 | Usuario envía `/ventas <día\|semana\|mes>`. | El sistema valida la autorización y procesa la solicitud. |
| 2 | — | El sistema consulta las ventas correspondientes en Loyverse. |
| 3 | — | El sistema responde el total y detalle de las ventas. |

### Excepciones

| # | Situación | Respuesta del sistema |
|---|-----------|-----------------------|
| E1 | Período sin ventas. | Responde "Sin ventas registradas en el período". |
| E2 | Parámetro de período inválido. | Responde "Período no reconocido, use día/semana/mes". |
| E3 | Loyverse no disponible. | Informa un mensaje de error temporal. |

| Campo | Detalle |
|-------|---------|
| Rendimiento | ≤15s (RNF-02); datos con demora máxima de 5s (RNF-03). |
| Frecuencia | 15-20 consultas/día. |
| Importancia | Alta. |
| Urgencia | Media. |

---

## CU-03 — Consultar Productos Más Vendidos

| Campo | Detalle |
|-------|---------|
| Identificador | CU-03 |
| Nombre | Consultar Productos Más Vendidos |
| Descripción | Permite al usuario autorizado consultar un ranking de los productos más vendidos. |
| Actores | Principal: Usuario Autorizado / Secundario: Loyverse |
| Precondiciones | Usuario validado. |
| Postcondiciones | Éxito: Se muestra el ranking de productos / Fallo: Se informa el error correspondiente. |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
|---|----------------|--------------------|
| 1 | Usuario envía `/top <cantidad>`. | El sistema valida la autorización y procesa la solicitud. |
| 2 | — | El sistema consulta las ventas registradas en Loyverse. |
| 3 | — | El sistema genera y responde el ranking de productos. |

### Excepciones

| # | Situación | Respuesta del sistema |
|---|-----------|-----------------------|
| E1 | Cantidad solicitada excede el catálogo disponible. | Devuelve el máximo posible e informa la situación. |
| E2 | No existen suficientes ventas históricas. | Responde "Datos insuficientes para generar ranking". |
| E3 | Parámetro no numérico. | Responde "Ingrese un número válido". |

| Campo | Detalle |
|-------|---------|
| Rendimiento | ≤15s (RNF-02). |
| Frecuencia | 5-10 consultas/día. |
| Importancia | Media. |
| Urgencia | Baja. |

---

## CU-04 — Consultar Resumen del Negocio

| Campo | Detalle |
|-------|---------|
| Identificador | CU-04 |
| Nombre | Consultar Resumen del Negocio |
| Descripción | Permite al usuario autorizado consultar un resumen general de la información del negocio. |
| Actores | Principal: Usuario Autorizado / Secundario: Loyverse |
| Precondiciones | Usuario validado. |
| Postcondiciones | Éxito: Se muestra el resumen del negocio / Fallo: Se informa si existen datos incompletos. |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
|---|----------------|--------------------|
| 1 | Usuario envía `/resumen`. | El sistema valida la autorización y procesa la solicitud. |
| 2 | — | El sistema obtiene la información necesaria de Loyverse. |
| 3 | — | El sistema responde cantidad de productos, ventas totales y movimientos recientes. |

### Excepciones

| # | Situación | Respuesta del sistema |
|---|-----------|-----------------------|
| E1 | Falta uno de los indicadores. | Omite esa sección e informa "Sin movimientos recientes". |
| E2 | Loyverse contiene datos parciales. | El resumen indica "Información incompleta al momento de la consulta". |

| Campo | Detalle |
|-------|---------|
| Rendimiento | ≤15s (RNF-02). |
| Frecuencia | 10-15 consultas/día. |
| Importancia | Alta. |
| Urgencia | Media. |

---

## CU-05 — Generar Reporte

| Campo | Detalle |
|-------|---------|
| Identificador | CU-05 |
| Nombre | Generar Reporte |
| Descripción | Permite al usuario autorizado generar un reporte sobre la información del negocio para un período determinado. |
| Actores | Principal: Usuario Autorizado / Secundario: Loyverse |
| Precondiciones | Usuario validado y existencia de datos en el período solicitado. |
| Postcondiciones | Éxito: Se entrega el reporte en el chat / Fallo: Se informa el inconveniente correspondiente. |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
|---|----------------|--------------------|
| 1 | Usuario envía `/reporte <tipo> <período>`. | El sistema valida la autorización y procesa la solicitud. |
| 2 | — | El sistema obtiene de Loyverse los datos necesarios para generar el reporte. |
| 3 | — | El sistema genera y entrega el reporte en el chat. |

### Excepciones

| # | Situación | Respuesta del sistema |
|---|-----------|-----------------------|
| E1 | Sin movimientos. | Responde "Sin movimientos en el período solicitado". |
| E2 | Rango de fechas inválido. | Responde "Rango de fechas inválido". |
| E3 | Más de 1000 registros. | Entrega un reporte parcial e informa "Reporte truncado, solicite un rango menor". |
| E4 | Loyverse limita las solicitudes. | Informa "Reporte en proceso, se enviará en breve". |

| Campo | Detalle |
|-------|---------|
| Rendimiento | ≤15s hasta aproximadamente 1000 registros (RNF-02); demora máxima de datos de 5s (RNF-03). |
| Frecuencia | 3-6 reportes/día, concentrados entre 20-23hs. |
| Importancia | Alta. |
| Urgencia | Media. |

---

## CU-06 — Recibir Alerta de Stock Bajo

| Campo | Detalle |
|-------|---------|
| Identificador | CU-06 |
| Nombre | Recibir Alerta de Stock Bajo |
| Descripción | Permite informar automáticamente al usuario responsable cuando un producto alcanza o queda por debajo de su stock mínimo. |
| Actores | Principal: Administrador del Sistema / Secundario: Loyverse, Telegram |
| Precondiciones | Producto con stock mínimo configurado. |
| Postcondiciones | Éxito: Se envía una alerta por Telegram / Fallo: Se registra el inconveniente y no se genera una alerta incorrecta. |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
|---|----------------|--------------------|
| 1 | — | El sistema verifica periódicamente el stock de los productos. |
| 2 | — | Detecta que el stock es menor o igual al mínimo configurado. |
| 3 | — | El sistema envía una alerta por Telegram con el nombre y cantidad disponible. |

### Excepciones

| # | Situación | Respuesta del sistema |
|---|-----------|-----------------------|
| E1 | No existe mínimo definido. | No genera alerta y registra el evento internamente. |
| E2 | Producto discontinuado o inactivo. | No genera alerta aunque el stock sea 0. |

| Campo | Detalle |
|-------|---------|
| Rendimiento | Detección ≤5s desde el cambio real (RNF-03); envío ≤15s desde la detección. |
| Frecuencia | Verificación periódica durante el funcionamiento del sistema. |
| Importancia | Alta. |
| Urgencia | Alta. |

---

## CU-07 — Notificar Comando Inválido

| Campo | Detalle |
|-------|---------|
| Identificador | CU-07 |
| Nombre | Notificar Comando Inválido |
| Descripción | Permite informar al usuario cuando el comando ingresado no corresponde a ninguna funcionalidad disponible. |
| Actores | Principal: Usuario Autorizado |
| Precondiciones | El usuario se encuentra autorizado y el comando no coincide con una funcionalidad válida. |
| Postcondiciones | Éxito: Se informa el error y, cuando corresponde, se sugiere un comando válido. |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
|---|----------------|--------------------|
| 1 | Usuario envía un comando no válido. | El sistema identifica que el comando no corresponde a una funcionalidad disponible. |
| 2 | — | El bot responde "Comando no válido, use /ayuda para ver opciones". |

### Excepciones

| # | Situación | Respuesta del sistema |
|---|-----------|-----------------------|
| E1 | Comando parcialmente similar a uno válido. | Sugiere el comando más cercano. Ejemplo: `/stok` → "¿Quiso decir /stock?". |

| Campo | Detalle |
|-------|---------|
| Rendimiento | Respuesta ≤15s (RNF-02). |
| Frecuencia | Estimado 5-10% del total de comandos recibidos por día. |
| Importancia | Media. |
| Urgencia | Baja. |

---

## CU-08 — Monitorear Flujos del Sistema

| Campo | Detalle |
|-------|---------|
| Identificador | CU-08 |
| Nombre | Monitorear Flujos del Sistema |
| Descripción | Permite al administrador supervisar el funcionamiento de los procesos automatizados, detectar fallos y realizar las correcciones necesarias. |
| Actores | Principal: Administrador del Sistema |
| Precondiciones | Administrador autenticado y con acceso a las herramientas de administración del sistema. |
| Postcondiciones | Éxito: Procesos supervisados y corregidos cuando sea necesario / Fallo: Se revierte o aísla el proceso afectado. |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
|---|----------------|--------------------|
| 1 | Administrador revisa las ejecuciones y registros del sistema. | El sistema muestra la información disponible sobre las ejecuciones. |
| 2 | Administrador detecta fallos o cuellos de botella. | El sistema permite identificar el proceso afectado. |
| 3 | Administrador realiza la corrección correspondiente. | El sistema aplica los cambios realizados. |

### Excepciones

| # | Situación | Respuesta del sistema |
|---|-----------|-----------------------|
| E1 | Proceso sin documentación. | Se marca como pendiente de estandarización antes de modificarlo (RNF-08). |
| E2 | Un cambio afecta otro proceso existente. | Se revierte el cambio y se aísla el proceso afectado (RNF-07). |

| Campo | Detalle |
|-------|---------|
| Rendimiento | Sin restricción de tiempo real; corresponde a una tarea de mantenimiento. |
| Frecuencia | Revisión estimada una vez por semana o ante un incidente reportado. |
| Importancia | Alta. |
| Urgencia | Media. |

---

## CU-09 — Mantener Infraestructura del Sistema

| Campo | Detalle |
|-------|---------|
| Identificador | CU-09 |
| Nombre | Mantener Infraestructura del Sistema |
| Descripción | Permite al administrador supervisar y mantener la disponibilidad de los componentes necesarios para el funcionamiento del sistema. |
| Actores | Principal: Administrador del Sistema / Secundarios: Loyverse, Telegram |
| Precondiciones | Acceso a las herramientas de administración correspondientes. |
| Postcondiciones | Éxito: Infraestructura disponible y operativa / Fallo: Incidente identificado y notificado. |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
|---|----------------|--------------------|
| 1 | Administrador supervisa la disponibilidad del sistema. | El sistema permite verificar el estado de sus componentes. |
| 2 | Administrador verifica las conexiones externas. | Se informa el estado de las conexiones con Loyverse y Telegram. |
| 3 | Administrador aplica correcciones o actualizaciones. | El sistema incorpora los cambios realizados. |

### Excepciones

| # | Situación | Respuesta del sistema |
|---|-----------|-----------------------|
| E1 | Caída fuera del horario 07-23hs. | No genera incidente crítico según RNF-01; se resuelve antes de la apertura. |
| E2 | Integración con Loyverse caída durante el horario operativo. | Se notifica al administrador de forma prioritaria. |

| Campo | Detalle |
|-------|---------|
| Rendimiento | Disponibilidad garantizada de 07:00 a 23:00hs (RNF-01). |
| Frecuencia | Monitoreo continuo/diario. |
| Importancia | Alta. |
| Urgencia | Alta durante el horario operativo. |

---


## Correcciones realizadas

- Se eliminaron los casos de uso **CU-08 "Procesar Solicitud vía n8n"** y **CU-09 "Obtener Datos de Loyverse"**, porque son tareas que se realizan internamente para poder completar otros casos de uso y no representan un objetivo del usuario.

- Se quitó a **n8n como actor**, ya que es la herramienta que usamos para armar y automatizar los flujos del sistema, no un actor que interactúe con el sistema por cuenta propia.

- Los procesos que realiza n8n quedaron incluidos dentro de los pasos de los casos de uso donde corresponden.

- **Loyverse sigue apareciendo como sistema externo**, porque el sistema obtiene de ahí la información de productos, stock y ventas.

- Se eliminó **CU-07 "Validar Usuario Autorizado"** como caso de uso independiente, ya que la validación se realiza como parte de las distintas consultas y no es una acción que el usuario solicite por separado.

- Se reorganizó la numeración de los casos de uso para que queden ordenados desde **CU-01 hasta CU-09**.

- Se mantuvieron las funciones principales del sistema: consultar stock y ventas, consultar los productos más vendidos, ver el resumen del negocio, generar reportes, recibir alertas de stock bajo, informar comandos inválidos y realizar tareas de administración.
