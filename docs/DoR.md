# Definition of Ready (DoR)

_Antes de que una historia entre a desarrollo, tiene que pasar un filtro: el Definition of
Ready. Es un acuerdo del equipo sobre qué condiciones mínimas debe cumplir una historia para
considerarse "lista para trabajar". Si no las cumple, vuelve a refinamiento._

---

## Checklist del equipo

_Entre 6 y 10 ítems. Cada uno redactado como una condición verificable ("la historia tiene
criterios de aceptación escritos"), no como un deseo ("la historia está bien definida")._

| # | Ítem | Justificación (qué problema evita, máx. 3 renglones) |
|---|------|--------------------------------------------------------|
| 1 | La historia usa el formato "Como [rol], quiero [acción], para [objetivo]". | Evita historias que son tareas técnicas sin un usuario ni un valor claro. |
| 2 | Tiene al menos 3 criterios de aceptación escritos, cada uno verificable con sí/no. | Evita que "terminada" sea una opinión: sin criterios, cada integrante entiende algo distinto. |
| 3 | Está vinculada a al menos un RF, RNF o CU del repositorio. | Evita construir algo que ningún requisito pidió. |
| 4 | Los criterios con tiempos o límites citan el RNF correspondiente, sin adjetivos como "rápido". | Evita criterios que no se pueden medir ni probar. |
| 5 | Tiene definido al menos un caso de error o excepción (ej. producto inexistente, Loyverse caído). | Evita descubrir recién en desarrollo qué pasa cuando algo sale mal. |
| 6 | No depende de otra historia sin terminar, o la dependencia está escrita. | Evita historias bloqueadas que no se pueden empezar ni probar solas. |
| 7 | Cubre una sola acción del usuario con un único tipo de resultado, y se puede implementar y probar en una iteración. | Evita historias gordas que no se pueden estimar ni terminar a tiempo. |

---

## Aplicación a tres historias propias

_Elijan TRES historias de usuario de su propio trabajo del primer semestre y pásenlas por su
propia checklist. Es esperable —y deseable— que alguna no pase._

### Historia 1 — HU-01 Consulta de Stock

| Ítem (según checklist) | ¿Pasa? | Qué le falta (si no pasa) |
|-------------------------|--------|-----------------------------|
| 1 — Formato clásico | Sí | — |
| 2 — Criterios verificables | Sí | — |
| 3 — Vinculada a RF/RNF/CU | Sí | — (RF-01, RNF-02) |
| 4 — Tiempos citan el RNF | Sí | — (el criterio 3 cita RNF-02) |
| 5 — Caso de error definido | Sí | — (el criterio 2 cubre producto inexistente) |
| 6 — Sin dependencias ocultas | Sí | — |
| 7 — Tamaño de iteración | Sí | — |

---

### Historia 2 — HU-03 Recepción de Respuestas Automáticas

| Ítem (según checklist) | ¿Pasa? | Qué le falta (si no pasa) |
|-------------------------|--------|-----------------------------|
| 1 — Formato clásico | Sí | — |
| 2 — Criterios verificables | Sí | — |
| 3 — Vinculada a RF/RNF/CU | Sí | — (RF-06, RF-07, RNF-02) |
| 4 — Tiempos citan el RNF | Sí | — (el objetivo dice "rápida y sencilla", pero el criterio 2 lo mide con RNF-02) |
| 5 — Caso de error definido | Sí | — (el criterio 3 cubre el comando no reconocido) |
| 6 — Sin dependencias ocultas | Sí | — |
| 7 — Tamaño de iteración | Sí | — |

---

### Historia 3 — HU-04 Generación de Reporte

| Ítem (según checklist) | ¿Pasa? | Qué le falta (si no pasa) |
|-------------------------|--------|-----------------------------|
| 1 — Formato clásico | Sí | — |
| 2 — Criterios verificables | No | El criterio 3 ("formato legible") no se puede verificar con sí/no, y el 2 ("el reporte correspondiente") es vago. Además, ningún criterio fija un tiempo de respuesta (RNF-02), y se dice que los datos se obtienen "desde Loyverse", mientras que el MER (Decisión 1) los toma de la copia local. |
| 3 — Vinculada a RF/RNF/CU | Sí | — (RF-12, RNF-03; podría sumar CU-05) |
| 4 — Tiempos citan el RNF | Sí | — (el criterio 2 cita RNF-03; el tiempo de respuesta no tiene criterio, ver ítem 2) |
| 5 — Caso de error definido | No | Ningún criterio cubre un error. El CU-05 ya tiene excepciones (sin movimientos, rango de fechas inválido, más de 1000 registros): incorporar al menos una. |
| 6 — Sin dependencias ocultas | Sí | — |
| 7 — Tamaño de iteración | No | Promete reportes de ventas, stock e indicadores en una sola historia. Conviene partirla por tipo de reporte (se conecta con el ejercicio de slicing). |
