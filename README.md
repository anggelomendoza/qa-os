# QA-OS

**Un sistema operativo de QA: método + herramientas para que las suites de casos estén bien armadas y cubran lo que tienen que cubrir.**

QA-OS nace de la práctica diaria de un QA Senior en fintech. No busca tener "muchos agentes", sino un método de QA explícito, versionado y reutilizable, donde las herramientas automatizan lo mecánico, la IA asiste en lo que requiere criterio y el QA decide.

[![CI](https://github.com/anggelomendoza/qa-os/actions/workflows/ci.yml/badge.svg)](https://github.com/anggelomendoza/qa-os/actions/workflows/ci.yml)

---

## Las dos preguntas de un crosscheck

Todo crosscheck de una suite responde dos preguntas distintas, y QA-OS tiene un módulo para cada una:

| Pregunta | Módulo | Cómo |
|---|---|---|
| ¿La suite está **bien armada**? | [`schema-validator`](modules/schema-validator/) | Determinístico, sin IA. Campos vacíos, valores inválidos, IDs, numeración de steps, rutas de suite |
| ¿La suite **cubre lo que tiene que cubrir**? | [`gap-analyzer`](skills/gap-analyzer/) | Skill de IA. Deriva condiciones de la historia, las cruza contra la suite y reporta solo gaps verificados |

Primero lo barato y confiable, después la IA. Un crosscheck humano no debería gastar tiempo en encontrar un `is_flaky` vacío.

## Arquitectura

| Capa | Qué contiene | Ejemplo |
|---|---|---|
| **Proceso** | Etapas agnósticas del trabajo de QA, iguales en cualquier proyecto | Historia → condiciones → casos → ejecución → cierre |
| **Dominio** | Conocimiento de negocio que se acumula proyecto a proyecto | Reglas de cálculo, estados, restricciones regulatorias |
| **Ejecución** | Adaptadores a las herramientas concretas | Formato de import de QASE, Jira, CSV |

Principios:

1. **Single source of truth**: cada regla de negocio vive en un solo lugar.
2. **Conocimiento versionado**: las reglas tienen estado (Confirmada / Hipótesis) y fuente.
3. **Separación proceso / dominio**: el método no cambia cuando cambia el negocio.
4. **Degradación a modo manual**: todo lo que hace la IA se puede hacer a mano con el mismo checklist.

## Módulos

| Módulo | Tipo | Estado |
|---|---|---|
| [`schema-validator`](modules/schema-validator/) | Script Python | ✅ Estable |
| [`gap-analyzer`](skills/gap-analyzer/) | Skill de IA | 🚧 En desarrollo |
| `test-conditions-generator` | Skill de IA | 📋 Planificado |
| `case-writer` | Skill de IA | 📋 Planificado |
| `bug-reporter` | Plantilla + skill | 📋 Planificado |
| `uat-packager` | Skill de IA | 📋 Planificado |

## Probalo en 30 segundos

```bash
git clone https://github.com/anggelomendoza/qa-os.git
cd qa-os
pip install -r modules/schema-validator/requirements.txt
python3 modules/schema-validator/qase_schema_validator.py examples/billetera-transferencias-programadas/suite-con-errores.csv
```

Ver [el ejemplo completo](examples/billetera-transferencias-programadas/): una historia ficticia, una suite con errores de estructura sembrados y otra con gaps de cobertura sembrados.

## Cómo se valida la IA

Cada ejemplo trae la respuesta correcta (`gaps-esperados.md`) con gaps sembrados a propósito y trampas que no son gaps. Eso permite medir:

- **Recall**: ¿encontró los gaps que sabemos que están?
- **Precisión**: ¿reportó gaps que no existen?
- **Regresión**: ¿un cambio en el prompt empeoró algo que antes funcionaba?

Ver [docs/roadmap.md](docs/roadmap.md).

## Estructura

```
qa-os/
├── modules/
│   └── schema-validator/     # validador de estructura de suites QASE (Python)
├── skills/
│   └── gap-analyzer/         # skill de IA para crosscheck de cobertura
├── examples/
│   └── billetera-transferencias-programadas/
├── docs/
│   └── roadmap.md
└── .github/workflows/ci.yml
```

## Sobre los ejemplos

Todos los ejemplos usan **dominios ficticios**. Ningún dato, regla ni caso proviene de proyectos reales de empleadores o clientes.

## Autor

Anggelo Mendoza — QA Senior en fintech. [github.com/anggelomendoza](https://github.com/anggelomendoza)

## Licencia

MIT — ver [LICENSE](LICENSE).
