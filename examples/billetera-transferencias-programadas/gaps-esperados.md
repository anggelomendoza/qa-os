# Gaps sembrados (answer key)

> Este archivo es la "respuesta correcta" del ejemplo. Sirve para medir al Gap Analyzer: recall (¿los encontró?) y precisión (¿inventó otros?).
> No se le pasa a la IA durante el análisis.

## Gaps reales que el analizador DEBE encontrar

| ID | Prioridad | Gap | Fuente |
|---|---|---|---|
| G1 | Alta | Cancelación bloqueada en **backend** después de las 23:59 del día anterior (la suite solo valida que el botón no aparezca) | CA 7 + RN-02 |
| G2 | Alta | Límite diario combinado: transferencia inmediata + programada del mismo día superan el límite | RN-01 |
| G3 | Media | Notificación **push y mail** al fallar por falta de saldo (TC009 valida el estado pero no la notificación) | CA 6 |
| G4 | Media | Fecha a **91 días** rechazada (borde superior fuera de rango; solo se prueba 90) | CA 2 |
| G5 | Media | Fecha en **feriado** ejecuta el día hábil siguiente (solo se prueba sábado) | CA 4 |
| G6 | Media | Ejecución **Fallida** queda registrada en el historial (TC008 solo cubre la exitosa) | CA 10 |
| G7 | Baja | Monto exacto **$100** aceptado (borde inferior válido; solo se prueba $99) | CA 3 |

## Cobertura parcial esperada

| Caso | Qué le falta |
|---|---|
| TC007 | Verifica el aviso con sábado, pero no que la ejecución real ocurra el lunes |
| TC011 | Valida el día de ejecución, pero no el límite exacto (23:59 del día anterior vs 00:00) |

## Trampas (NO son gaps; el analizador no debe reportarlas)

| Condición | Por qué no es gap |
|---|---|
| Programar con fecha = mañana | Cubierta por TC002 |
| Monto mayor al límite diario | Cubierta por TC006 |
| Máximo de 10 programadas | Cubierta por TC012 |
| Fecha = hoy rechazada | Cubierta por TC003 |

## Preguntas abiertas legítimas (no gaps)

- ¿Qué pasa si el contacto guardado se elimina antes de la ejecución? La historia no lo define.
- ¿Cuenta una transferencia Fallida para el máximo de 10 pendientes? La historia no lo aclara.
