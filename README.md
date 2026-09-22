# HelpDesk Lab

> Homelab de Help Desk, IT Support y monitoreo de seguridad. Construido desde cero en Arch Linux con VMware Workstation Pro.

Simulación de un entorno corporativo completo en 4 VMs: identidad con
Active Directory, mesa de ayuda (GLPI), monitoreo con SIEM (Wazuh), y el
flujo completo de escalamiento Help Desk → NOC → Respuesta a Incidentes.
El objetivo es demostrar habilidades prácticas de soporte técnico y
administración de sistemas en un entorno empresarial.

------AKI VA LA TOPOLOGIA EN GIF EKISDE XD--------

---

## 🎯 Objetivo

Construir un entorno de TI funcional donde:

- Existan empleados como usuarios en Active Directory.
- Reportan problemas vía tickets en GLPI (con autenticación LDAP contra AD).
- Su actividad se monitorea con Wazuh (SIEM).
- Un intento de fuerza bruta dispara una alerta que conecta Help Desk con el departamento correspondiente.

---

## Arquitectura

El laboratorio vive en una sola red virtual llamada `vmnet2`, configurada
en modo host-only. Las VMs se ven entre ellas pero no
pueden salir a internet ni tocar la red del host.

El segmento es `192.168.10.0/24`, que da 254 direcciones usables:

- `.2 – .20` → reservado para servidores, con IP estática para que nunca cambien.
- `.100 – .150` → clientes, asignadas por DHCP al arrancar.

El dominio utilizado es `corp.local` y cuenta con un único controlador de dominio.



| VM | SO | IP | Rol |
|---|---|---|---|
| **DC01** | Windows Server 2022 | `192.168.10.2` | AD DS · DNS · DHCP |
| **CLIENT01** | Windows 11 Pro | DHCP | Estación de trabajo unida al dominio |
| **SRV-GLPI** | Ubuntu Server 24.04 | `192.168.10.10` | LAMP + GLPI (ITSM) |
| **SRV-WAZUH** | Ubuntu Server 24.04 | `192.168.10.11` | SIEM: Manager + Indexer + Dashboard |

---

## Entorno del laboratorio

- **Virtualización:** VMware Workstation Pro
- **Sistemas Operativos:** Windows Server 2022, Windows 11, Ubuntu Server 24.04 LTS
- **Identidad:** Active Directory (AD DS), DNS, DHCP, LDAP
- **ITSM:** GLPI sobre LAMP
- **SIEM:** Wazuh
- **Automatización:** PowerShell

---

## Fases de Implementación

| Fase | Descripción | Estado | Detalle |
|---|---|---|---|
| 01 | Preparación del hipervisor y segmentación de red | ✅ | [Ver detalle](docs/fases/fase-01-red.md) |
| 02 | Aprovisionamiento y configuración base de DC01 | ✅ | [Ver detalle](docs/fases/fase-02-dc01-base.md) |
| 03 | Promoción del dominio `corp.local` | ✅ | [Ver detalle](docs/fases/fase-03-promocion-ad.md) |
| 04 | Unión de CLIENT01 al dominio | 🔴 | [Ver detalle](docs/fases/fase-04-client01.md) |
| 05 | Despliegue de GLPI + autenticación LDAP | 🔴 | [Ver detalle](docs/fases/fase-05-glpi.md) |
| 06 | Despliegue de Wazuh + escenario de incidente | 🔴 | [Ver detalle](docs/fases/fase-06-wazuh-incidente.md) |

---

## Procedimientos Operativos (SOPs)

- [SOP — Onboarding de usuario](docs/sops/sop-onboarding.md)
- [SOP — Reset de contraseña](docs/sops/sop-reset-password.md)
- [SOP — Resolución de tickets en GLPI](docs/sops/sop-resolucion-tickets-glpi.md)

---

## Escenario de Demostración

El laboratorio incluye un flujo completo de respuesta a incidentes:

1. Simulación de intentos de logon fallidos (fuerza bruta) en DC01.
2. Wazuh detecta el patrón → dispara alerta.
3. Se abre ticket en GLPI reportando actividad sospechosa.
4. El ticket se escala a NOC citando la alerta de Wazuh como evidencia.
5. Se bloquea la cuenta afectada en AD y se cambia contraseña.
6. Se redacta un [post-incident report](docs/post-incident-report-demo.md).

---

## Estructura del Repositorio

    .
    ├── README.md
    ├── .gitignore
    ├── assets/          # Capturas y diagramas
    ├── docs/
    │   ├── fases/       # Detalle técnico de cada fase
    │   ├── sops/        # Procedimientos operativos estándar
    │   └── post-incident-report-demo.md
    └── scripts/         # Automatización (Python + PowerShell)

---

## 🧠 Skills Técnicas Demostradas

- Administración de Windows Server 2022 y Active Directory (AD DS)
- Diseño e implementación de DNS y DHCP en entornos de dominio
- Administración de Linux (Ubuntu Server) y stack LAMP
- Virtualización con VMware Workstation Pro (redes host-only)
- Automatización con PowerShell y Python
- Gestión de servicios IT con GLPI (ITSM, tickets, KB)
- Monitoreo y respuesta a incidentes con SIEM (Wazuh)
- Autenticación LDAP e integración con Active Directory
- Documentación técnica (SOPs) y control de versiones con Git
