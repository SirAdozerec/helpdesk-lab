# Cuarta fase — Unión de CLIENT01 al Dominio

## Objetivo

Desplegar una estación de trabajo Windows 11 Pro unida al dominio `corp.local`,
que simulará el usuario final del Help Desk.

## Procedimiento

Procedimiento

1. Creación de la VM: Desplegué una nueva máquina virtual con Windows 11 Pro.
2. Configuración de red: Conecté el adaptador de red a vmnet2 en modo Custom (Host-Only) para mantener el aislamiento.
3. Prueba de DHCP: Antes de intentar la unión al dominio, verifiqué que la máquina estuviera recibiendo IP correctamente por DHCP desde el DC01 (el rango configurado es .100–.150).
4. Unión al dominio: Una vez con conectividad, procedí a unir el equipo al dominio corp.local utilizando las credenciales de CORP\Administrator.
5. Post-configuración: Reinicié la máquina y realicé la primera sesión de usuario con una cuenta del dominio para confirmar el acceso.


## Validación

- Ejecuté ipconfig y confirmé que la IP asignada pertenece al rango DHCP establecido por el controlador de dominio.
  
- Revisé las propiedades del sistema y verifiqué que el equipo ya figura como parte del dominio corp.local.
  
- El login con cuenta de dominio funcionó correctamente tras el primer reinicio.

<img width="585" height="201" alt="image" src="https://github.com/user-attachments/assets/317dd976-8b13-4c1d-b688-d2673af2bfa7" />


<img width="658" height="409" alt="image" src="https://github.com/user-attachments/assets/535e9e40-7db6-45d9-855e-6f1605b8989c" />


