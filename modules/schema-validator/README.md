# QASE Schema Validator

> Módulo de [QA-OS](../../README.md). Responde la pregunta "¿la suite está bien armada?". Para "¿cubre lo que tiene que cubrir?" ver [`gap-analyzer`](../../skills/gap-analyzer/).

A small, dependency-light Python script that validates a QASE "Test Designer / repository import" `.xlsx` file **before** you upload it — instead of finding out about empty fields, invalid values, or malformed steps from a crosscheck after the cases are already live.

No API calls, no QASE credentials, nothing project-specific hardcoded. Point it at a file, get a plain-text report.

Reads both `.xlsx` (the "Test Designer / repository import" format) and `.csv` (the format you export from QASE after closing a crosscheck) — the format is detected from the file extension alone, and both are validated against the same column schema.

## Why this exists

Manually built or edited QASE suites are easy to get subtly wrong in ways that don't show up until someone reviews the suite by hand: a required field left blank, an invalid enum value that slipped past a copy-paste, a suite path with a typo, step numbering with a gap. This script catches that class of mistake mechanically, so a human reviewer can spend their attention on the actual test design instead of re-checking formatting every time.

## What it checks

- **Required fields** are not empty (configurable list).
- **Enum fields** (priority, severity, type, behavior, automation, status, layer, ...) only contain values you've declared valid.
- **Title format**: the title matches a configurable pattern, and the case number embedded in it matches the row's id.
- **Unique, sane ids**: no duplicates; gaps are flagged as a warning (not an error — appending a new batch of cases to an existing suite legitimately starts mid-sequence).
- **Step numbering**: `steps_actions`, `steps_result`, and `steps_data` each have well-formed internal numbering (no gaps, no duplicates). A *different* number of steps between actions and result is reported only as a warning — in practice, a single action step commonly opens into several separately-numbered result assertions, which is valid, not a defect.
- **Suite consistency**: `suite_id` / `suite_parent_id` / `suite` hold the same value across the whole file — catches a suite path typo that would otherwise route cases to the wrong place.

## Usage

```bash
pip install openpyxl   # only needed if you're validating .xlsx files
python3 modules/schema-validator/qase_schema_validator.py your_suite.xlsx
python3 modules/schema-validator/qase_schema_validator.py your_suite.csv
```

Exit code is `1` if any error was found, `0` otherwise — usable as a gate in a script or CI step.

Output an .md report instead of stdout:

```bash
python3 modules/schema-validator/qase_schema_validator.py your_suite.xlsx --out report.md
```

## Adapting it to your own suite's conventions

Nothing about title format, required fields, or valid enum values is hardcoded — they all live in `DEFAULT_CONFIG` at the top of the script, and you can override any of them with your own JSON:

```bash
python3 modules/schema-validator/qase_schema_validator.py your_suite.xlsx --config my_config.json
```

See `examples/config.example.json` for a config that uses a different title prefix and a different priority/severity scale than the defaults.

## What it deliberately does *not* do

- It doesn't talk to the QASE API — it only reads the `.xlsx` file you point it at.
- It doesn't try to judge whether your test design is *good* — only whether the file is structurally sound enough to import cleanly.
- It doesn't flag a differing step-action/step-result count as an error, on purpose — see "What it checks" above. An earlier, stricter version of this rule produced false positives on a real, already-approved 40+ case suite before this exception was added.

## License

MIT — see `LICENSE`.
