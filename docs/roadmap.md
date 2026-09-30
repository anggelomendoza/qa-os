# Roadmap

Meta: al 2 de octubre de 2027, QA-OS con 3–4 módulos publicados, suite de automatización con CI y evaluación de los módulos de IA.

## Fase 1 — Octubre 2026: Gap Analyzer
- [x] Estructura del repo y README
- [x] `schema-validator` integrado como primer módulo, con CI
- [x] Suite de ejemplo con errores de estructura sembrados
- [x] Skill `gap-analyzer` (primera versión)
- [x] Ejemplo ficticio con gaps sembrados (billetera, transferencias programadas)
- [ ] Reemplazar el checklist por la versión de 13 dimensiones del QA-OS original
- [ ] Correr el analizador contra el ejemplo y comparar con `gaps-esperados.md`
- [ ] Renombrar el repo a `qa-os` en GitHub y pushear

## Fase 2 — Noviembre / Diciembre 2026: Evaluación
- [ ] 3–5 ejemplos más, en dominios distintos (e-commerce, turnos médicos, suscripciones)
- [ ] Script de evaluación: recall y precisión por ejemplo
- [ ] GitHub Actions que corra la evaluación en cada cambio de prompt

## Fase 3 — 2027: más módulos y app demo
- [ ] `test-conditions-generator` como módulo independiente
- [ ] `case-writer` con export a formato QASE
- [ ] App demo con bugs sembrados + suite Playwright/TypeScript con POM
- [ ] `bug-reporter` y `uat-packager`

## Contenido (@actesteando.ar)
Cada hito del roadmap alimenta al menos un post: qué se construyó, qué falló, qué se aprendió.
