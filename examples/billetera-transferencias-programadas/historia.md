# Historia: Transferencias programadas (Billetera ficticia "Mora")

> Dominio ficticio creado para demostrar el Gap Analyzer. No representa ningún producto real.

## Descripción

**Como** usuario de la billetera Mora,
**quiero** programar una transferencia para una fecha futura,
**para** no tener que acordarme de hacerla ese día.

## Criterios de aceptación

1. El usuario puede programar una transferencia a un contacto guardado, eligiendo monto y fecha de ejecución.
2. La fecha de ejecución debe ser entre **mañana** y **90 días** desde hoy (ambos inclusive).
3. El monto mínimo es **$100** y el máximo es el **límite diario** del usuario ($500.000 por defecto).
4. Si la fecha elegida cae en fin de semana o feriado, la transferencia se ejecuta el **día hábil siguiente**. La pantalla de confirmación debe avisarlo con el texto: *"Tu transferencia se hará el próximo día hábil: <fecha>"*.
5. El saldo **no** se reserva al programar. Se valida al momento de ejecutar.
6. Si al ejecutar no hay saldo suficiente, la transferencia pasa a estado **Fallida** y se notifica al usuario por push y mail.
7. El usuario puede **cancelar** una transferencia programada hasta las **23:59 del día anterior** a la ejecución. Después de eso, el botón "Cancelar" no aparece.
8. El usuario puede tener como máximo **10 transferencias programadas** pendientes a la vez.
9. Estados: `Programada` → `Ejecutada` | `Fallida` | `Cancelada`.
10. Toda ejecución (exitosa o fallida) queda registrada en el historial de movimientos.

## Reglas de negocio confirmadas

- **RN-01**: el límite diario se evalúa sobre la suma de transferencias inmediatas + programadas que se ejecutan ese día. *(Confirmada)*
- **RN-02**: la cancelación también debe estar bloqueada en el backend, no solo ocultando el botón. *(Confirmada)*
