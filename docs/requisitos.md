# Requisitos del sistema

## Descripción del sistema

El sistema SAS consiste en el desarrollo de una herramienta de consulta y monitoreo comercial orientada a pequeños y medianos comercios, diseñada para facilitar el acceso rapido a informacion importante del negocio meidante un bot de Telegram automatizado con n8n.
El sistema se integra con Loyverse, una plataforma de gestion comercial (POS), de la cual obtiene información de productos, stock y ventas mediante su API para generar consultas, reportes y alertas automaticas. El objetivo principal es que el dueño o encargado del comercio pueda acceder a esta información en tiempo real, desde cualquier lugar, utilizando unicamente Telegram, evitando ais el uso de paneles administrativos complejos.

## Requisitos funcionales

### Módulo 1 — Consulta de información general

| ID | Requisito |
|----|-----------|
| RF-01 | El sistema deberá permitir al usuario consultar el stock disponible de un producto mediante comandos enviados desde Telegram.|
| RF-02 |  El sistema deberá mostrar información relacionada con las ventas realizadas en distintos períodos, como ventas diarias, semanales o mensuales. |
| RF-03 | El sistema deberá identificar y mostrar los productos con mayor cantidad de ventas.|
| RF-04 |  El sistema deberá generar alertas automáticas cuando el stock de un producto alcance un límite mínimo definido. |
| RF-05 | El usuario podrá solicitar información resumida del negocio, como cantidad de productos, ventas totales o movimientos recientes. |

### Módulo 2 — Comunicación e integración

| ID | Requisito |
|----|-----------|
| RF-06 | El sistema deberá responder automáticamente las consultas realizadas por el usuario a través del bot de Telegram. |
| RF-07 | El sistema deberá utilizar flujos automatizados en n8n para procesar solicitudes y gestionar respuestas. |
| RF-08 |  El sistema deberá restringir el acceso únicamente al usuario autorizado mediante validación de Telegram. |
| RF-09 | El sistema deberá permitir que las consultas puedan realizarse desde cualquier dispositivo que tenga acceso a Telegram. |

### Módulo 3 — Registro y reportes

| ID | Requisito |
|----|-----------|
| RF-10 | El sistema deberá permitir almacenar información básica de los productos, incluyendo nombre, código, categoría y stock disponible. |
| RF-11 | El sistema deberá almacenar información básica de las ventas realizadas para generar reportes y estadísticas. |
| RF-12 | El sistema deberá generar reportes básicos relacionados con ventas, stock y movimiento de productos. |

## Requisitos no funcionales

### Rendimiento y disponibilidad

| ID | Requisito |
|----|-----------|
| RNF-01 |  El sistema deberá estar disponible entre las 07:00 hs hasta las 23:00 hs. |
| RNF-02 | Las consultas realizadas por el usuario deberán responderse en 15 segundos para garantizar una interacción rápida y eficiente. |
| RNF-03 | La información proporcionada por el sistema deberá ser precisa y mantenerse actualizada en tiempo real o con mínima demora. |

### Seguridad y usabilidad

| ID | Requisito |
|----|-----------|
| RNF-04 | El sistema deberá ser simple e intuitivo, permitiendo que el usuario pueda realizar consultas sin necesidad de conocimientos técnicos avanzados. |
| RNF-05 |  El sistema deberá permitir el acceso únicamente a usuarios autorizados mediante validación del identificador de Telegram. |
| RNF-06 | El sistema deberá funcionar correctamente en dispositivos móviles, computadoras y cualquier dispositivo compatible con Telegram. |

### Mantenimiento y escalabilidad

| ID | Requisito |
|----|-----------|
| RNF-07 | El sistema deberá permitir futuras mejoras o incorporación de nuevas funcionalidades sin necesidad de rediseñar completamente la estructura. |
| RNF-08 |  La estructura del sistema y los flujos de n8n deberán desarrollarse con buenas prácticas para facilitar tareas de mantenimiento y actualización. |