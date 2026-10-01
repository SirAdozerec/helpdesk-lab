# SOP — Resolución de tickets en GLPI

## Objetivo
Proceso estándar para atender, categorizar y cerrar tickets de soporte.

## Flujo
1. **Recepción:** el ticket llega a la cola de GLPI.
2. **Triage:** clasificar por severidad y categoría.
  - Crítica: afecta producción o múltiples usuarios.
  - Alta: bloquea trabajo de un usuario.
  - Media: molesta pero hay workaround.
  - Baja: cosmético o solicitud.
3. **Asignación:** tomar el ticket o escalar al grupo correspondiente.
4. **Resolución:** aplicar la solución y documentar pasos.
5. **Cierre:** confirmar con el usuario y cerrar el ticket.

## Escalado
Si el ticket requiere intervención de otro equipo:
1. Actualizar la categoría.
2. Añadir nota interna con el motivo de escalado.
3. Reasignar al grupo correspondiente.

## Evidencia

<p align="center">
  <img width="800" alt="Cola de tickets en GLPI clasificados por severidad durante el triage" src="PON_AQUI_TU_LINK" />
</p>
<p align="center"><em>Triage de tickets en GLPI por nivel de severidad.</em></p>

<p align="center">
  <img width="800" alt="Ticket escalado en GLPI con categoría actualizada y nota interna del motivo" src="PON_AQUI_TU_LINK" />
</p>
<p align="center"><em>Escalado de un ticket: categoría actualizada y nota interna documentando el motivo.</em></p>
