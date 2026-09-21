# Fase 06 — Wazuh + Escenario de Incidente

## Objetivo
Desplegar un SIEM (Wazuh) que centralice logs de seguridad del laboratorio,
y ejecutar un escenario completo de detección y respuesta a un ataque simulado.

## Procedimiento
1. Creación de VM con Ubuntu Server 24.04 LTS.
2. IP estática: `192.168.10.11`.
3. Instalación de Wazuh Manager, Indexer y Dashboard.
4. Instalación de agente Wazuh en DC01 y CLIENT01.
5. Configuración de reenvío de Windows Event Logs.

## Escenario de demostración

1. Simulación de intentos de logon fallidos (fuerza bruta) contra una cuenta de prueba en DC01.
2. Wazuh detecta el patrón y dispara la alerta `multiple authentication failures`.
3. Se abre un ticket en GLPI reportando actividad sospechosa.
4. El ticket se escala a NOC citando la alerta de Wazuh como evidencia.
5. Se bloquea la cuenta afectada en AD y se resetea la contraseña.
6. Se redacta un post-incident report.

## Validación
- Dashboard de Wazuh con la alerta visible.
- Ticket en GLPI con estado "escalado a NOC".

![Alerta de fuerza bruta en Wazuh](../../assets/12-wazuh-bruteforce-alert.png)

![Ticket escalado a NOC en GLPI](../../assets/13-glpi-ticket-escalated.png)

## Pendiente
- [ ] Crear la VM
- [ ] Instalar Wazuh
- [ ] Configurar agentes
- [ ] Ejecutar el escenario
