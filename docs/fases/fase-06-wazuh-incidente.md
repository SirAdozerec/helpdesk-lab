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

(imagen de instalacion de wazuh xd): <img width="843" height="168" alt="image" src="https://github.com/user-attachments/assets/c86b5c20-fdb4-4fdc-b642-0bdd84cc13b5" />

(imagen de dashboard de wazuh xd): <img width="1899" height="822" alt="image" src="https://github.com/user-attachments/assets/d04a20d0-34ad-46d2-9927-511fbac4fcbe" />


(imagen del instalador de wazuh en el win server xd) <img width="1005" height="228" alt="image" src="https://github.com/user-attachments/assets/8c220d0b-4f08-479f-95ee-76df3a938f9f" />

(imagen del dashboard de wazuh con 1 vm xd) <img width="1896" height="561" alt="image" src="https://github.com/user-attachments/assets/d6257c9e-580f-43d7-abd8-04eccdcfb3c6" />


(log de fuerza bruta xd) <img width="1512" height="757" alt="image" src="https://github.com/user-attachments/assets/bb0eb63e-9a28-45d0-9862-5c4625be6c69" />

(un log más extendido del atake de fuerza bruta xd) <img width="1501" height="840" alt="image" src="https://github.com/user-attachments/assets/91e507a8-fe6e-40a5-98da-9d3ef6ebb722" />

(mi escalamiento como help desk a seguridad. Ticket de seguridad, múltiples intentos de logeo a una cuenta) <img width="1700" height="845" alt="image" src="https://github.com/user-attachments/assets/f5459050-e6b8-479d-8b1f-1240ef952248" />




## Pendiente
- [ ] Crear la VM
- [ ] Instalar Wazuh
- [ ] Configurar agentes
- [ ] Ejecutar el escenario
