# Fase 04 — Unión de CLIENT01 al Dominio

## Objetivo
Desplegar una estación de trabajo Windows 11 Pro unida al dominio `corp.local`,
que simulará el usuario final del Help Desk.

## Procedimiento
1. Creación de VM con Windows 11 Pro.
2. Conexión del adaptador de red a `vmnet2` en modo Custom (Host-Only).
3. Verificación de recepción de IP por DHCP (rango `.100–.150` desde DC01).
4. Unión al dominio `corp.local` con credenciales de `CORP\Administrator`.
5. Reinicio y login con cuenta de dominio.

## Validación
- `ipconfig` muestra IP dentro del rango DHCP.
- La ventana de Sistema muestra `corp.local` como dominio.

![CLIENT01 recibiendo IP por DHCP](../../assets/08-client01-ipconfig.png)

![CLIENT01 unido al dominio](../../assets/09-client01-domain-join.png)

## Pendiente
- [ ] Crear la VM
- [ ] Instalar Windows 11
- [ ] Verificar DHCP
- [ ] Unir al dominio
