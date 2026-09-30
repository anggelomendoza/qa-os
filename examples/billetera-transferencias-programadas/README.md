# Ejemplo: transferencias programadas (billetera ficticia "Mora")

Dominio inventado para demostrar los módulos de QA-OS. No representa ningún producto real.

| Archivo | Para qué sirve |
|---|---|
| `historia.md` | La historia de usuario con criterios de aceptación y reglas de negocio |
| `suite.csv` | Suite de 12 casos en formato de import de QASE. Estructuralmente válida, pero con gaps de cobertura a propósito |
| `suite-con-errores.csv` | La misma suite con errores de estructura sembrados (campos vacíos, enum inválido, título que no coincide con el id, numeración de steps rota, typo en la ruta de suite) |
| `gaps-esperados.md` | La respuesta correcta: los gaps de cobertura que el Gap Analyzer debería encontrar, y las trampas que no debería reportar |

## Qué demuestra cada módulo

**Schema validator** (sin IA): `suite.csv` pasa y `suite-con-errores.csv` es rechazada con 7 errores. Corre en CI en cada push.

```bash
python3 modules/schema-validator/qase_schema_validator.py examples/billetera-transferencias-programadas/suite-con-errores.csv
```

**Gap Analyzer** (con IA): le pasás `historia.md` y `suite.csv`, y se compara su reporte contra `gaps-esperados.md`. La suite está bien armada pero **no cubre todo lo que la historia pide**: esa diferencia es justamente lo que el validador de estructura no puede ver.
