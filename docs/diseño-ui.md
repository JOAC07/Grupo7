# Diseño UI

_Presentar al menos un wireframe por pantalla o módulo relevante._
_Los wireframes en imagen o PDF van en `diagramas/wireframes/`; acá se documenta la justificación de cada uno._

---

## Pantalla / Módulo 1 — [Nombre]

**Wireframe:** `diagramas/wireframes/[archivo]`

**Patrones de diseño utilizados:** _(ej: Card, Modal, Step-by-step, Tabla con paginación, etc.)_

**Justificación:** _¿Por qué estos patrones tienen sentido para este caso de estudio y estos usuarios? No alcanza con nombrar el patrón._

**Formulario (si aplica):**
- Cantidad de campos:
- Flujo (todo en una pantalla / por pasos):
- Validaciones relevantes:

---

## Pantalla / Módulo 2 — [Nombre]

**Wireframe:** `diagramas/wireframes/[archivo]`

**Patrones de diseño utilizados:**

**Justificación:**

**Formulario (si aplica):**
- Cantidad de campos:
- Flujo (todo en una pantalla / por pasos):
- Validaciones relevantes:

---

## Consideraciones de accesibilidad

_Al menos una consideración concreta, relacionada con el sistema y sus usuarios reales
(no una mención genérica de "cumple con WCAG"). Ejemplos: contraste para usuarios con
baja visión, tamaño de tap targets para uso móvil, navegación por teclado, textos
alternativos en ícono-only buttons._

-----------------------------------------------------------------------------------------------

# Diseño UI

El sistema no tiene pantallas web: la única interfaz es el chat del bot de Telegram. Los "wireframes" son maquetas de conversación (mensaje del usuario → respuesta del bot). Los datos de los ejemplos son ficticios.

## Convenciones de los wireframes

```
👤 = mensaje del usuario
🤖 = respuesta del bot
```

Comandos disponibles (se escriben como texto en el chat): `ventas`, `ultima venta`, `resumen`, `medios de pago`, `top productos`, `stock`, `ayuda`.

---

## Pantalla / Módulo 1 — Ayuda y primer contacto

### Wireframe

```
👤 ayuda

🤖 Comandos disponibles:
   • ventas – ventas del día
   • ultima venta – última venta realizada
   • resumen – resumen general del negocio
   • medios de pago – ventas por medio de pago
   • top productos – productos más vendidos
   • stock – productos con menos stock
   • ayuda – muestra esta lista
```

Usuario no autorizado (RF-08, RNF-05):

```
👤 ventas

🤖 No tenés acceso a este bot.
```

Comando desconocido (CU-07):

```
👤 stok

🤖 Comando no válido. ¿Quisiste decir "stock"?
   Escribí "ayuda" para ver todas las opciones.
```

### Patrones de diseño utilizados
Menú de ayuda (lista de comandos), mensaje de error guiado.

### Justificación
El usuario (dueño o encargado) no tiene conocimientos técnicos (RNF-04) y no hay botones ni menús visuales: la única forma de descubrir qué se puede hacer es `ayuda`. Por eso la lista es corta, con una línea por comando y su descripción, y el bot también la sugiere cuando el comando es inválido. El error guiado propone el comando más cercano en lugar de solo rechazar, para que el usuario corrija en un solo mensaje y no abandone la consulta. El mensaje de acceso denegado es breve y no revela información del negocio.

### Formulario
No hay formulario. Cantidad de campos: 1 (la palabra del comando). Flujo: un mensaje del usuario, una respuesta del bot.

---

## Pantalla / Módulo 2 — Consulta de ventas

Cubre `ventas`, `ultima venta` y `medios de pago` (RF-02, RF-05).

### Wireframe

```
👤 ventas

🤖 Ventas de hoy (13/10):
   Total: $ 184.500 en 23 ventas

   • 09:12 – $ 4.200
   • 09:40 – $ 12.800
   • 10:05 – $ 3.500
   • ...
```

```
👤 ultima venta

🤖 Última venta registrada:
   • Hora: 15:32
   • Total: $ 6.700
   • Productos: Yerba 1 kg x2, Galletitas x1
   • Medio de pago: Efectivo
```

```
👤 medios de pago

🤖 Ventas de hoy por medio de pago:
   • Efectivo: $ 98.000 
   • Tarjeta: $ 61.500 
   • Transferencia: $ 25.000 
   Total: $ 184.500
```

Sin datos:

```
👤 ventas

🤖 Sin ventas registradas en el período.
```

### Patrones de diseño utilizados
Comando–respuesta, mensaje estructurado (introducción + ítems), resumen primero y detalle después.

### Justificación
El usuario consulta desde el celular, muchas veces entre clientes (picos de 07 a 09 hs), por lo que necesita el dato clave sin escribir ni leer de más. Cada consulta se resuelve con una palabra (RNF-04: máximo 2 comandos). La respuesta empieza con una introducción de una línea y el total, y recién después el detalle en ítems: así el dato principal se ve sin scrollear. Se eligió lista de ítems y no una tabla porque Telegram no renderiza tablas de forma confiable en pantallas chicas, y una lista corta con viñetas se lee igual en móvil, computadora y web (RF-09, RNF-06). 

### Formulario
No hay formulario. Cantidad de campos: 1. Flujo: una pregunta, una respuesta. Los montos usan el formato local ($ 184.500) para evitar errores de lectura.

---

## Pantalla / Módulo 3 — Inventario y productos

Cubre `stock` y `top productos` (RF-01, RF-03).

### Wireframe

```
👤 stock

🤖 Productos con menos stock:
   • Yerba 1 kg: 2 unidades ⚠️ bajo mínimo
   • Aceite 900 ml: 4 unidades ⚠️ bajo mínimo
   • Fideos 500 g: 9 unidades
   • Azúcar 1 kg: 11 unidades
```

```
👤 top productos

🤖 Productos más vendidos:
   1. Gaseosa 1,5 l – 48 unidades
   2. Pan lactal – 35 unidades
   3. Yerba 1 kg – 29 unidades
   4. Leche 1 l – 27 unidades
   5. Galletitas – 21 unidades
```

Error de servicio (RNF-02, CU-01 E2):

```
👤 stock

🤖 Consultando stock...
   (si Loyverse no responde, tras un reintento)
   Servicio no disponible. Intentá de nuevo en unos minutos.
```

### Patrones de diseño utilizados
Comando–respuesta, lista ordenada (ranking), mensaje de estado ("Consultando...") y mensaje de error con acción sugerida.

### Justificación
El encargado necesita saber qué reponer, por eso `stock` ordena de menor a mayor y marca los productos bajo el mínimo con el texto "bajo mínimo", además del ícono. `top productos` usa lista numerada porque la posición es la información principal. El mensaje "Consultando..." aparece cuando la consulta puede tardar, ya que el sistema tiene hasta 15 segundos para responder (RNF-02): sin ese aviso el usuario podría pensar que el bot no funciona y repetir el comando. El error indica qué hacer (reintentar) en lugar de mostrar un código técnico.

### Formulario
No hay formulario. Cantidad de campos: 1. Flujo: una respuesta; si el nombre de un producto fuera ambiguo, el bot lista las coincidencias y el usuario responde con el nombre exacto (2 mensajes, dentro del límite de RNF-04).

---

## Pantalla / Módulo 4 — Resumen del negocio

Cubre `resumen` (RF-05, RF-12).

### Wireframe

```
👤 resumen

🤖 Resumen del negocio (13/10, 15:40):
   • Ventas de hoy: $ 184.500 (23 ventas)
   • Productos registrados: 312
   • Ticket promedio: $17.500
   
```

Datos incompletos (CU-04 E2):

```
🤖 Resumen del negocio (13/10, 15:40):
   • Ventas de hoy: $ 184.500 (23 ventas)
   • Productos registrados: 312
   ⚠️ Información incompleta al momento de la consulta.
```

### Patrones de diseño utilizados
Mensaje estructurado tipo tarjeta (Card textual) con indicadores clave, aviso de estado.

### Justificación
El resumen es lo que el dueño mira primero al abrir el chat. Se diseñó como una tarjeta de pocas líneas, cada una con una etiqueta y un valor, para leerse de un vistazo. Incluye fecha y hora para que sepa a qué momento corresponde el dato (los datos reflejan Loyverse con hasta 5 s de demora, RNF-03). Si falta información, el bot lo avisa en el mismo mensaje y no omite el aviso, para que el usuario no tome decisiones con datos parciales sin saberlo.

### Formulario
No hay formulario. Cantidad de campos: 1. Flujo: una respuesta.

---

## Pantalla / Módulo 5 — Alerta automática de stock bajo

Cubre RF-04 (CU-06). El bot escribe sin que el usuario pregunte.

### Wireframe

```
🤖 ⚠️ Alerta de stock bajo
   Producto: Yerba 1 kg
   Stock actual: 2 unidades (mínimo: 5)
```

### Patrones de diseño utilizados
Notificación proactiva (push) con mensaje estructurado.

### Justificación
Es el único flujo que inicia el sistema: avisa en el momento en que Loyverse notifica el cambio de stock, sin que el usuario deba acordarse de consultar. El mensaje es breve y trae el nombre, la cantidad y el mínimo, que es lo necesario para decidir una reposición.

### Formulario
No hay formulario ni interacción requerida.

---

## Patrones considerados y descartados

| Patrón | Motivo |
|---|---|
| Tabla con paginación | Telegram no la soporta bien en móvil; el volumen por respuesta es chico. |
| Modal | No existe en el chat; no hay altas ni ediciones desde el bot. |
| Step-by-step | Ninguna consulta necesita más de 1 o 2 mensajes (RNF-04). |
| Botones en línea | El alcance del proyecto define la interacción solo por comandos escritos. |

## Consideraciones de accesibilidad

- **No depender de íconos ni color:** los avisos llevan texto ("bajo mínimo", "Información incompleta"), y el ícono ⚠️ solo refuerza. Un lector de pantalla o un usuario que no distinga colores recibe la misma información.
- **Mensajes cortos y con estructura constante:** título, total y lista, siempre en el mismo orden, para reducir la carga de lectura y facilitar el uso con lectores de pantalla (cada línea se lee como un ítem independiente).
- **Compatibilidad con tamaño de letra y modo oscuro del dispositivo:** se usa texto plano y formato mínimo, de modo que Telegram aplique las preferencias del usuario en móvil, computadora y web (RNF-06).
- **Comandos tolerantes:** se aceptan mayúsculas/minúsculas y se sugiere el comando correcto ante un error de tipeo, útil para quien escribe con una mano o desde el teclado del celular.

## Coherencia con los requisitos no funcionales

| Requisito | Decisión de UI |
|---|---|
| RNF-02 | Mensaje "Consultando..." y errores que indican qué hacer. |
| RNF-03 | Fecha y hora en el resumen. |
| RNF-04 | Un comando por consulta; máximo 2 mensajes. |
| RNF-05 / RF-08 | Respuesta de acceso denegado sin datos del negocio. |
| RNF-06 / RF-09 | Texto plano y listas, sin tablas ni imágenes. |
