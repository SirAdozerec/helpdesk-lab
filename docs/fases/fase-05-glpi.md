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

<img width="661" height="740" alt="image" src="https://github.com/user-attachments/assets/ff53ce23-e8f6-45dd-bfc8-df5c70540c88" />


<img width="1882" height="905" alt="image" src="https://github.com/user-attachments/assets/2a975c74-d37f-43c8-ba19-7407e24d465a" />

<img width="1297" height="794" alt="image" src="https://github.com/user-attachments/assets/c52d47ee-692f-4419-9d39-3447c1683383" />

<img width="1007" height="793" alt="image" src="https://github.com/user-attachments/assets/084e0867-779a-452e-8239-96530d539ea8" />

<img width="1088" height="734" alt="image" src="https://github.com/user-attachments/assets/aeba1f62-0580-4dd2-9855-ea8387c8cdd1" />







## Pendiente
- [ ] Crear la VM
- [ ] Instalar LAMP
- [ ] Instalar GLPI
- [ ] Configurar LDAP
