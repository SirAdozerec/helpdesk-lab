# Fase 05 — Despliegue de GLPI (Mesa de Ayuda)

## Objetivo
Desplegar un sistema de tickets (ITSM) accesible desde CLIENT01, con
autenticación LDAP contra Active Directory.

## Procedimiento
1. Creación de VM con Ubuntu Server 24.04 LTS.
2. IP estática: `192.168.10.10`.
3. Instalación de stack LAMP (Apache + MariaDB + PHP).
4. Instalación de GLPI.
5. Configuración de autenticación LDAP contra `DC01`.

## Validación
- GLPI accesible desde CLIENT01 en `http://192.168.10.10/glpi`.
- Login de técnico funciona con credenciales de dominio.

![Login de GLPI con LDAP](../../assets/10-glpi-ldap-login.png)

![Ticket de ejemplo en GLPI](../../assets/11-glpi-ticket.png)

## Pendiente
- [ ] Crear la VM
- [ ] Instalar LAMP
- [ ] Instalar GLPI
- [ ] Configurar LDAP
