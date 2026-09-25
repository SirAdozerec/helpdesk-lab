# Sexta fase — Wazuh + Escenario de Incidente

## Objetivo
Desplegar un SIEM (**Wazuh**) para centralizar los logs de seguridad del laboratorio y ejecutar un escenario completo de detección y respuesta ante un ataque de fuerza bruta. El fin es demostrar un flujo operativo.

## Procedimiento

1.  **Creación de la VM:** Desplegué una máquina virtual con **Ubuntu Server 24.04 LTS**, asignándole **4 vCPUs y 8 GB de RAM** para cumplir con los requisitos del Wazuh Indexer.

2.  **Estrategia de red (Dual NIC):** Usé el mismo patrón que en GLPI: un adaptador en `vmnet2` para la red interna y otro en modo **NAT** exclusivamente para descargar paquetes durante la instalación.

3.  **Configuración de IP estática:** Configuré la IP `192.168.10.11/24` vía **netplan**, usando al DC01 (`192.168.10.2`) como DNS.

4.  **Instalación de Wazuh (all-in-one):** Utilicé el script oficial `wazuh-install.sh -a` para desplegar los tres componentes en una sola VM: **Manager, Indexer y Dashboard**. La interfaz quedó accesible en `https://192.168.10.11`.

<div align="center">
  <img width="843" alt="Instalación de Wazuh finalizada" src="https://github.com/user-attachments/assets/c86b5c20-fdb4-4fdc-b642-0bdd84cc13b5" />
  <br><em style="font-size: 0.9em;">Instalación de Wazuh finalizada con éxito.</em>
</div >
<br></br>

5.  **Instalación del agente en DC01:** Descargué el MSI del agente desde un servidor HTTP local (SRV-WAZUH) y lo instalé en DC01 apuntando al manager (`192.168.10.11`).

<div align="center">
  <img width="1005" alt="Instalación del agente de Wazuh en DC01" src="https://github.com/user-attachments/assets/8c220d0b-4f08-479f-95ee-76df3a938f9f" />
  <br><em style="font-size: 0.9em;">Agente de Wazuh instalado y corriendo en DC01.</em>
</div >
<br></br>


## Validación

*   ✅ **Estado de servicios:** Verifiqué que los 4 servicios principales (**Manager, Indexer, Dashboard y Filebeat**) arrancaran correctamente.

*   ✅ **Acceso web:** Confirmé que el dashboard es accesible desde la red del laboratorio mediante HTTPS.

*   ✅ **Registro de agente:** El agente de DC01 aparece como activo en la vista de Agents Summary dentro del dashboard.

<div align="center">
  <img width="1899" alt="Dashboard de Wazuh" src="https://github.com/user-attachments/assets/d04a20d0-34ad-46d2-9927-511fbac4fcbe" />
  <br><em style="font-size: 0.9em;">Dashboard de Wazuh tras la instalación inicial.</em>
  <br><br>
  <img width="1896" alt="Agente DC01 activo" src="https://github.com/user-attachments/assets/d6257c9e-580f-43d7-abd8-04eccdcfb3c6" />
  <br><em style="font-size: 0.9em;">Agente DC01 registrado y activo en el dashboard.</em>
</div >
<br></br>

## Escenario de demostración — Ataque de fuerza bruta

Con el SIEM operativo y el agente enviando eventos, ejecuté un ataque controlado para probar la capacidad de detección.

1.  **Simulación del ataque:** Ejecuté un bucle de intentos fallidos contra la cuenta `victoria.alejandro` desde DC01. Esto generó múltiples eventos en el log de seguridad.

2.  **Detección por parte de Wazuh:** El SIEM procesó los eventos y disparó la regla **60122 — "Multiple Windows Logon Failures"**, detectando **133 hits** contra la cuenta en menos de un minuto.

<div align="center">
  <img width="1512" alt="Alerta de fuerza bruta en Wazuh" src="https://github.com/user-attachments/assets/bb0eb63e-9a28-45d0-9862-5c4625be6c69" />
  <br><em style="font-size: 0.9em;">Vista general de la alerta: 133 hits de la regla 60122.</em>
  <br><br>
  <img width="1501" alt="Detalle de los eventos de fuerza bruta" src="https://github.com/user-attachments/assets/91e507a8-fe6e-40a5-98da-9d3ef6ebb722" />
  <br><em style="font-size: 0.9em;">Detalle de los eventos.</em>
</div >
<br></br>

3.  **Escalamiento Help Desk:** Con la evidencia del SIEM, abrí un ticket en GLPI como incidente de seguridad y adjunté las capturas de Wazuh. El ticket se escaló al grupo **N3 - Seguridad**, ya que el evento superaba el alcance de Help Desk L1.

<div align="center">
  <img width="1700" alt="Ticket escalado a N3 - Seguridad" src="https://github.com/user-attachments/assets/f5459050-e6b8-479d-8b1f-1240ef952248" />
  <br><em style="font-size: 0.9em;">Ticket en GLPI escalado a N3 - Seguridad con evidencia adjunta.</em>
</div >

## Resolución de problemas/Troubleshooting:

* **Falta de espacio en disco:** La instalación fallaba con el error `Wazuh dashboard installation failed` porque el volumen raíz solo tenía 9.8 GB asignados. **Solución:** Expandí la partición LVM usando `growpart`, `pvresize`, `lvextend` y `resize2fs` hasta alcanzar los 33 GB necesarios.

* **Descarga del agente en red aislada:** Como DC01 no tiene internet, no podía bajar el MSI directamente. **Solución:** Descargué el archivo en SRV-WAZUH y lo serví mediante un servidor HTTP local con `python3 -m http.server 8000`.

## Notas:

* El escenario completo (detección $\rightarrow$ ticket $\rightarrow$ escalamiento) está documentado en [`docs/post-incident-report-demo.md`](../post-incident-report-demo.md).
* Como mejora pendiente, se podría implementar **Active Response** en Wazuh para bloquear automáticamente la IP atacante al detectar el patrón de fuerza bruta.
