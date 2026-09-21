# Fase 01 — Preparación del Hipervisor y Segmentación de Red

## Objetivo
Preparar el host físico y crear una red virtual aislada donde operará todo el laboratorio.

## Procedimiento
1. Instalación de VMware Workstation Pro en Arch Linux (KDE Plasma / Wayland).
2. Creación de la interfaz virtual `vmnet2` en modo **Host-Only** (`192.168.10.0/24`).
3. Desactivación del servicio DHCP nativo de VMware en `vmnet2` — la administración de IPs queda delegada a DC01.

## Validación
- `vmnet2` aparece en Virtual Network Editor con: tipo host-only, DHCP: no, subnet `192.168.10.0`.
- El servicio `vmware-networks.service` inicia correctamente en Arch.

![Configuración de vmnet2](../../assets/01-vmnet2-config.png)

## Notas
- Al reiniciar el host, el servicio `vmware-networks` se apaga. Se inicia manualmente:

    sudo systemctl start vmware-networks.service
