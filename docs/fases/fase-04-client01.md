# Cuarta fase — Unión de CLIENT01 al Dominio

## Objetivo

Desplegar una estación de trabajo Windows 11 Pro unida al dominio `corp.local`,
que simulará el usuario final del Help Desk.

## Procedimiento

1. Creación de la VM: Desplegué una nueva máquina virtual con Windows 11 Pro.
2. Configuración de red: Conecté el adaptador de red a vmnet2 en modo Custom (Host-Only) para mantener el aislamiento.
3. Prueba de DHCP: Antes de intentar la unión al dominio, verifiqué que la máquina estuviera recibiendo IP correctamente por DHCP desde el DC01 (el rango configurado es .100–.150).
4. Unión al dominio: Una vez con conectividad, procedí a unir el equipo al dominio corp.local utilizando las credenciales de CORP\Administrator.
5. Post-configuración: Reinicié la máquina y realicé la primera sesión de usuario con una cuenta del dominio para confirmar el acceso.


## Validación

- Ejecuté ipconfig y confirmé que la IP asignada pertenece al rango DHCP establecido por el controlador de dominio.
  
- Revisé las propiedades del sistema y verifiqué que el equipo ya figura como parte del dominio corp.local.
  
- El login con cuenta de dominio funcionó correctamente tras el primer reinicio.

<div align="center">

<img width="787" height="556" alt="image" src="https://github.com/user-attachments/assets/825e77d0-33dd-47b3-b10c-ee556090efdf" />



<br><br>

<img width="658" height="409" alt="image" src="https://github.com/user-attachments/assets/535e9e40-7db6-45d9-855e-6f1605b8989c" />

</div>

## Resolución de problemas / Troubleshooting

- Incidencia con el servicio DHCP en CLIENT01
Al intentar unir la estación al dominio, me encontré con que CLIENT01 no obtenía IP por DHCP; se quedaba con una dirección APIPA (169.254.x.x) y el ipconfig /renew no respondía.

- Tras investigar, detecté que el problema era que el rol DHCP en DC01 no estaba autorizado en el bosque de Active Directory. Como es estándar en Windows Server, el servicio no reparte IPs hasta que se autoriza formalmente para evitar conflictos de red.

- Solución aplicada:
Entré al Server Manager en DC01 y completé la autorización del servidor con la cuenta CORP\Administrator. Una vez hecho esto, CLIENT01 pudo comunicarse con el DC01 y obtuvo su IP (192.168.10.100) sin problemas.

## Notas

Tras validar la Fase 4, se tomó un snapshot de cada VM como punto de retorno seguro antes de proceder con el despliegue de GLPI.
