---
name: gap-analyzer
description: Cruza una historia de usuario (con criterios de aceptación y reglas de negocio) contra una suite de casos de prueba existente y reporta gaps de cobertura verificados y priorizados, sin inventar hallazgos. Usar cuando se pida un crosscheck, un análisis de cobertura o "qué le falta a esta suite".
---

# Gap Analyzer

Objetivo: encontrar lo que la suite **no** prueba y debería, con evidencia, y dejar explícito lo que se revisó y **no** era gap.

Regla de oro: **no generar humo.** Un gap solo se reporta si, después de leer la suite completa, no existe ningún caso que lo cubra (ni total ni parcialmente). Un gap falso le cuesta credibilidad al crosscheck.

## Entradas

1. **Historia**: descripción, criterios de aceptación, reglas de negocio, contrato de API o diseño si hay.
2. **Suite**: lista de casos con al menos ID, título, precondiciones, pasos y resultado esperado (CSV de QASE o equivalente).
3. *(Opcional)* **Reglas conocidas**: reglas ya confirmadas con el PO o Dev, con su estado.

Si falta la historia o la suite, pedirla antes de analizar. No inferir la historia a partir de la suite.

**Antes de analizar cobertura**, si la suite viene en formato QASE, conviene pasarla por `modules/schema-validator`. Los errores de estructura (campos vacíos, IDs, numeración) los resuelve ese script sin IA; este análisis se concentra en la cobertura.

## Proceso

### Paso 1 — Derivar condiciones de prueba de la historia

Recorrer la historia con este checklist y listar cada condición como una afirmación verificable ("Si X, entonces Y"). Anotar de qué parte de la historia sale cada una.

<!-- TODO: reemplazar este checklist por la versión de 13 dimensiones del QA-OS original -->

- **Camino feliz**: el flujo principal con datos válidos.
- **Reglas de negocio**: cada fórmula, condición o cálculo explícito.
- **Límites**: valores en el borde exacto, justo antes y justo después (≥ vs >).
- **Validaciones de entrada**: vacío, nulo, formato inválido, fuera de rango.
- **Estados y transiciones**: qué se puede hacer en cada estado y qué no.
- **Permisos y roles**: quién puede y quién no, validado en UI **y** en backend.
- **Caminos de error**: fallas de integraciones, timeouts, datos faltantes.
- **Cancelación y arrepentimiento**: qué pasa si el usuario cancela a mitad de camino.
- **Tiempo y calendario**: horarios de corte, fines de semana, feriados, cierres de período.
- **Monedas y unidades**: conversión, redondeo, múltiples monedas a la vez.
- **Visualización y copy**: textos exactos, montos mostrados, estados vacíos.
- **Consistencia entre vistas**: el mismo dato en pantallas o exportables distintos.
- **Efectos secundarios**: notificaciones, auditoría, registros contables, sincronizaciones.

No todas las dimensiones aplican a todas las historias. Marcar las que no aplican y por qué, en una línea.

### Paso 2 — Mapear cada condición contra la suite

Para cada condición, buscar en la suite **completa** (título, precondiciones, pasos y resultado esperado, no solo el título):

- **Cubierta**: hay un caso que la valida explícitamente. Anotar el ID.
- **Parcial**: hay un caso que la toca pero le falta algo concreto (dato, aserción, variante). Anotar el ID y qué falta exactamente.
- **No cubierta**: ningún caso la valida.

### Paso 3 — Auto-verificación antes de reportar

Para cada gap candidato ("Parcial" o "No cubierta"):

1. Releer la suite buscando sinónimos y formas indirectas de cubrirlo.
2. Confirmar que la condición sale de la historia y no de una suposición propia. Si sale de una suposición, pasa a "Preguntas abiertas", no a gap.
3. Si un caso existente ya lo cubre, moverlo a "Descartados" con el ID que lo cubre.

### Paso 4 — Priorizar

- **Alta**: regla de negocio, dinero, permisos o datos de usuario sin cubrir.
- **Media**: camino de error, borde o estado alternativo sin cubrir.
- **Baja**: visualización, copy o variante de bajo riesgo.

## Salida

Responder con este formato:

```markdown
# Crosscheck — <historia>

**Resumen:** <N> condiciones derivadas · <N> cubiertas · <N> parciales · <N> gaps · <N> preguntas abiertas

## Gaps

| # | Prioridad | Condición no cubierta | Fuente en la historia | Caso sugerido |
|---|---|---|---|---|

## Cobertura parcial

| # | Caso existente | Qué le falta |
|---|---|---|

## Preguntas abiertas

Ambigüedades de la historia que impiden definir el resultado esperado. No son gaps de la suite: son consultas para el PO o Dev.

## Descartados (revisados, no eran gap)

| Condición | Cubierta por |
|---|---|

## Observaciones de calidad de la suite

Casos no atómicos, metadata incompleta, resultados esperados ambiguos. Opcional y breve.
```

## Reglas de estilo

- Títulos de casos sugeridos: solo lo que el caso valida, sin contexto de cómo surgió.
- Sin nombres propios de personas en los casos sugeridos.
- Si algo queda por confirmar, va como nota breve: "(Consulta abierta: ...)".
- No reescribir casos existentes salvo que se pida.
