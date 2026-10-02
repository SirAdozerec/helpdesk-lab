# HelpDesk Lab

> Homelab de IT Support construido desde cero en Arch Linux con VMware Workstation Pro.

Simulación de un entorno corporativo en 4 VMs: identidad con
Active Directory, mesa de ayuda (GLPI), monitoreo con SIEM (Wazuh), y
el ciclo completo de soporte técnico: desde la creación de un usuario
hasta la resolución de un incidente. El objetivo es demostrar habilidades
prácticas de soporte técnico y administración de sistemas en un entorno
empresarial.



---

## 🎯 Objetivo

Construir un entorno de TI funcional donde:

- Existan empleados como usuarios en Active Directory.
- Reportan problemas vía tickets en GLPI (con autenticación LDAP contra AD).
- Su actividad se monitorea con Wazuh (SIEM).
- Un intento de fuerza bruta dispara una alerta que conecta Help Desk con el departamento correspondiente.

---

## Arquitectura

El laboratorio reside en una sola red virtual llamada `vmnet2`, configurada
como host-only. Las VMs se ven entre ellas pero no
pueden salir a internet ni tocar la red del host.

El segmento es `192.168.10.0/24`, que da 254 direcciones usables:

- `.2 – .20` → reservado para servidores, con IP estática para que nunca cambien.
- `.100 – .150` → clientes, asignadas por DHCP al arrancar.

El dominio utilizado es `corp.local` y cuenta con un único controlador de dominio.

<div align="center">

| VM | SO | IP | Rol |
|---|---|---|---|
| **DC01** | Windows Server 2022 | `192.168.10.2` | AD DS · DNS · DHCP |
| **CLIENT01** | Windows 11 Pro | DHCP | Estación de trabajo unida al dominio |
| **SRV-GLPI** | Ubuntu Server 24.04 | `192.168.10.10` | LAMP + GLPI (ITSM) |
| **SRV-WAZUH** | Ubuntu Server 24.04 | `192.168.10.11` | SIEM: Manager + Indexer + Dashboard |

</div>

---

## Flujo de trabajo del laboratorio

El laboratorio reproduce el ciclo completo de soporte técnico de una
empresa mediana:

1. **Identidad:** los empleados se encuentran en Active Directory. Inician sesión
   en su equipo con sus credenciales de dominio.

2. **Soporte diario:** si algo falla, abren un ticket en GLPI. El
   sistema valida su identidad vía LDAP contra AD, así usan la misma
   cuenta.

3. **Resolución:** el técnico atiende el ticket, documenta la solución,
   y lo cierra. Los procedimientos están estandarizados en los SOPs.

4. **Monitoreo:** los endpoints envían sus logs a Wazuh (SIEM),
   que vigila la actividad del dominio en tiempo real.

5. **Escalamiento (como caso puntual):** cuando un evento supera el alcance
   de L1 (por ejemplo, una alerta de fuerza bruta detectada por Wazuh),
   se escala al equipo correspondiente con evidencia técnica adjunta.

El objetivo del laboratorio es que cada máquina cumpla una función
concreta dentro de un flujo de trabajo verosímil.

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

<div align="center">

| Fase | Descripción |
| --- | --- |
| [01 — Hipervisor y red](https://github.com/SirAdozerec/helpdesk-lab/blob/main/docs/fases/fase-01-red.md) | Segmentación de red host-only y preparación del entorno virtual |
| [02 — DC01 base](https://github.com/SirAdozerec/helpdesk-lab/blob/main/docs/fases/fase-02-dc01-base.md) | Aprovisionamiento de Windows Server 2022 como futuro Domain Controller |
| [03 — Promoción del dominio](https://github.com/SirAdozerec/helpdesk-lab/blob/main/docs/fases/fase-03-promocion-ad.md) | Creación del bosque `corp.local` |
| [04 — CLIENT01](https://github.com/SirAdozerec/helpdesk-lab/blob/main/docs/fases/fase-04-client01.md) | Unión de la estación de trabajo al dominio |
| [05 — GLPI](https://github.com/SirAdozerec/helpdesk-lab/blob/main/docs/fases/fase-05-glpi.md) | Despliegue de la mesa de ayuda con autenticación LDAP |
| [06 — Wazuh](https://github.com/SirAdozerec/helpdesk-lab/blob/main/docs/fases/fase-06-wazuh-incidente.md) | SIEM, detección de fuerza bruta y escalamiento completo |

</div>

---

## Procedimientos Operativos (SOPs)

- [SOP — Onboarding de usuario](docs/sops/sop-onboarding.md)
- [SOP — Reset de contraseña](docs/sops/sop-reset-password.md)
- [SOP — Resolución de tickets en GLPI](docs/sops/sop-resolucion-tickets-glpi.md)

---

## 🧠 Skills Aplicadas

- Administración de Windows Server 2022 y Active Directory (AD DS)
- Implementación de DNS y DHCP en entornos de dominio
- Administración de Linux (Ubuntu Server) y stack LAMP
- Virtualización con VMware Workstation Pro
- Automatización con PowerShell
- Gestión de servicios IT con GLPI (ITSM, tickets, KB)
- Monitoreo y respuesta a incidentes con SIEM (Wazuh)
- Autenticación LDAP e integración con Active Directory
- Documentación técnica (SOPs) y control de versiones con Git

---
