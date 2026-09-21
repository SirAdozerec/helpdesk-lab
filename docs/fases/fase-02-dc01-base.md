# Fase 02 — Aprovisionamiento y Configuración Base de DC01

## Objetivo
Desplegar la VM de Windows Server 2022 que funcionará como Controlador de Dominio.

## Procedimiento
1. Creación de VM con Windows Server 2022 Standard (Desktop Experience).
2. Asignación de recursos: **4 GB RAM, 4 vCPUs, 40 GB NVMe**.
3. Conexión del adaptador de red a `vmnet2` en modo Custom (Host-Only).
4. Configuración de IP estática: `192.168.10.2/24`, sin gateway (red aislada).
5. Hostname configurado como `DC01`.

## Validación
- `ipconfig` muestra IP `192.168.10.2`, máscara `255.255.255.0`, sin gateway.
- El adaptador de red está conectado a vmnet2 en VMware.

![Configuración de hardware de DC01](../../assets/02-dc01-hardware.png)

![ipconfig de DC01 con IP estática](../../assets/03-dc01-ipconfig.png)

## Notas
- Sin gateway porque la red es host-only sin salida a internet.
