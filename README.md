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

| Fase | Descripción |
| --- | --- |
| [01 — Hipervisor y red](https://github.com/SirAdozerec/helpdesk-lab/blob/main/docs/fases/fase-01-red.md) | Segmentación de red host-only y preparación del entorno virtual |
| [02 — DC01 base](https://github.com/SirAdozerec/helpdesk-lab/blob/main/docs/fases/fase-02-dc01-base.md) | Aprovisionamiento de Windows Server 2022 como futuro Domain Controller |
| [03 — Promoción del dominio](https://github.com/SirAdozerec/helpdesk-lab/blob/main/docs/fases/fase-03-promocion-ad.md) | Creación del bosque `corp.local` |
| [04 — CLIENT01](https://github.com/SirAdozerec/helpdesk-lab/blob/main/docs/fases/fase-04-client01.md) | Unión de la estación de trabajo al dominio |
| [05 — GLPI](https://github.com/SirAdozerec/helpdesk-lab/blob/main/docs/fases/fase-05-glpi.md) | Despliegue de la mesa de ayuda con autenticación LDAP |
| [06 — Wazuh](https://github.com/SirAdozerec/helpdesk-lab/blob/main/docs/fases/fase-06-wazuh-incidente.md) | SIEM, detección de fuerza bruta y escalamiento completo |

---

## Procedimientos Operativos (SOPs)

- [SOP — Onboarding de usuario](docs/sops/sop-onboarding.md)
- [SOP — Reset de contraseña](docs/sops/sop-reset-password.md)
- [SOP — Resolución de tickets en GLPI](docs/sops/sop-resolucion-tickets-glpi.md)

---

## Flujo de escalamiento: de Help Desk a Respuesta a Incidentes

Un ticket de Help Desk no siempre se queda en Help Desk. En este laboratorio se documenta un caso completo de cuándo y cómo escalar algo que supera el alcance de un analista de Soporte Técnico:

**1. El SIEM detecta un patrón de fuerza bruta contra una cuenta de AD.**
   
**2. Se abre un ticket en GLPI documentando la actividad sospechosa, el
   primer punto de contacto.**

**3. Reconociendo que el caso supera el alcance de L1, se escala al grupo
   de Seguridad citando la alerta de Wazuh como evidencia.**

**4. Se aplica la contención inmediata (bloqueo de cuenta) y se documenta
   el cierre con un [post-incident report](https://github.com/SirAdozerec/helpdesk-lab/blob/main/docs/post-incident-report-demo.md).***

La intención de este añadido al lab, es el reconocimiento del
límite de alcance y escalar con evidencia.

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
