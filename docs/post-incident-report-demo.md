# Post-Incident Report — Fuerza Bruta Simulada

## Resumen
El día [FECHA] se detectó un patrón de intentos de logon fallidos contra una
cuenta de dominio en DC01. El SIEM Wazuh disparó una alerta, la cual se
convirtió en un ticket de GLPI y se escaló al equipo de seguridad.

## Cronología
| Hora | Evento |
|---|---|
| T+0 | Inicio del ataque simulado contra la cuenta `test.user` |
| T+2min | Wazuh detecta el patrón (12 intentos fallidos en 60 segundos) |
| T+3min | Alerta visible en el dashboard de Wazuh |
| T+5min | Se abre ticket en GLPI reportando actividad sospechosa |
| T+8min | Ticket escalado a NOC citando la alerta de Wazuh |
| T+10min | Cuenta bloqueada en Active Directory |
| T+12min | Contraseña reseteada y notificada al usuario |
| T+15min | Ticket cerrado con post-incident report |

## Causa raíz
Simulación de fuerza bruta realizada por el propio administrador del lab
como ejercicio de respuesta a incidentes.

## Acciones tomadas
- Bloqueo de la cuenta afectada.
- Reset de contraseña.
- Verificación de que no hubo accesos exitosos desde IPs no autorizadas.

## Lecciones aprendidas
- El tiempo de detección (2 min) es adecuado para un SIEM básico.
- La integración Wazuh → GLPI permite trazabilidad completa del incidente.
- Se recomienda agregar reglas de bloqueo automático para futuros casos.

## Pendiente
- [ ] Ejecutar el escenario real y llenar este reporte con datos reales
