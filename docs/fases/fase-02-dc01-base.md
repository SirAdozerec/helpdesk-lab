# Segunda fase — Configuración Base de DC01

## Objetivo
Desplegar la VM de Windows Server 2022 que funcionará como el Controlador de Dominio.

## Procedimiento
1. Creación de VM con Windows Server 2022 Standard (Desktop Experience).
2. Asignación de recursos: **4 GB RAM, 4 vCPUs, 40 GB NVMe**.
3. Conexión del adaptador de red a `vmnet2` en modo Custom (Host-Only).
4. Configuración de IP estática: `192.168.10.2/24`, sin gateway (red aislada).
5. Hostname configurado como `DC01`.

## Validación
- `ipconfig` muestra IP `192.168.10.2`, máscara `255.255.255.0`, sin gateway.
- El adaptador de red está conectado a vmnet2 en VMware.

<p align="center">
  <img width="828" height="689" alt="Captura 2 - Hardware y enlace de la VM" src="https://github.com/user-attachments/assets/b7d0dea9-39d2-4440-a0f9-7e8d7a82f763" />
</p>

<p align="center">
  <img width="724" height="526" alt="Configuración de red IP estática en DC01" src="https://github.com/user-attachments/assets/06179dbe-e869-439a-b781-193f59cadc4c" />
</p>


## Notas
- Sin gateway porque la red es host-only sin salida a internet.
