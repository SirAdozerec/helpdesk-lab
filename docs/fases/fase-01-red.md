# Fase 01 — Preparación del Hipervisor y Segmentación de Red

## Objetivo
Preparar el host físico y crear una red virtual aislada donde va a vivir todo el laboratorio.

## Procedimiento
1. Instalé VMware Workstation Pro.

2. Creé la interfaz virtual `vmnet2` en modo Host-Only, con el segmento `192.168.10.0/24`.

3. Apagué el DHCP nativo de VMware en `vmnet2`, quería que fuera DC01 quien repartiera las IPs, no VMware. De esta manera, el laboratorio se comporta como una red donde el controlador de dominio también funge como servidor DHCP.

## Validación
En el Virtual Network Editor confirmé que `vmnet2` quedó como host-only, con el DHCP apagado y la subred en `192.168.10.0`.

<p align="center">
<img width="625" height="609" alt="Virtual Network Editor mostrando vmnet2 configurada como host-only, con DHCP deshabilitado y la subred 192.168.10.0" src="https://github.com/user-attachments/assets/300be9e6-0277-4051-9a08-c2bdbbd353fe" />
</p>

También verifiqué que el servicio `vmware-networks.service` arrancara sin errores.

## Notas
`vmware-networks.service` no queda habilitado para iniciar solo al reiniciar el host. Cada vez que reinicio, tengo que levantarlo a mano:

\`\`\`bash
sudo systemctl start vmware-networks.service
\`\`\`
