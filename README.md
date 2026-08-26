# Homelab Help Desk

Documentación, scripts,  y procedimientos operativos estándar (SOPs) de un entorno corporativo simulado desde cero en Arch Linux utilizando **VMware Workstation Pro**.

---

# 🎯 Objetivo:

Crear desde cero una infraestructura corporativa simulada para poner en práctica y demostrar conocimientos de administración de redes y servicios en un entorno empresarial.

La idea es llevar a la práctica conocimientos útiles en puestos de **Help Desk**, trabajando con herramientas y situaciones que pueden encontrarse en un entorno de TI.

Entre las principales áreas del proyecto se encuentran:

1. **Gestión de usuarios y accesos:** Configurar **Active Directory (AD DS)** para administrar usuarios, grupos y permisos, además de implementar políticas de seguridad mediante **GPO**. También se utilizarán **DNS y DHCP** en Windows Server 2022 para gestionar la red.

2. **Automatización:** Utilizar **PowerShell y Python** para automatizar tareas repetitivas, como la creación de usuarios y la configuración de equipos, buscando reducir el trabajo manual y hacer más eficientes las tareas administrativas.

3. **Mesa de ayuda e ITSM:** Implementar **GLPI** sobre **Ubuntu Server** para simular una mesa de ayuda real, gestionando tickets, categorizando incidentes y documentando su resolución.

4. **Redes y segmentación:** Configurar direcciones IP, DHCP y distintas redes virtuales para simular la segmentación y el aislamiento de dispositivos dentro de una infraestructura empresarial.

En general, el proyecto busca ser una simulación práctica de un entorno de TI, pasando desde la configuración inicial de la infraestructura hasta la administración de usuarios, equipos, red e incidencias.


## 🖧 Topología y Arquitectura de Red

Toda la infraestructura local opera dentro del segmento privado aislado **Host-Only** (`192.168.10.0/24`) con el servicio DHCP de VMware desactivado para delegar la administración central a Windows Server.

<img width="2720" height="1920" alt="homelab_helpdesk_topology (1)" src="https://github.com/user-attachments/assets/7220c070-ff4a-49be-9174-7602813ca70d" />


* **Subred privada:** `192.168.10.0/24`

  
* **Nodos del laboratorio:**
  
  * **DC01 (Windows Server 2022):** `192.168.10.2` — Controlador de Dominio (`corp.local`), DNS y servidor DHCP.
    
  * **CLIENT01 (Windows 11 Pro):** Asignación dinámica (`192.168.10.100 - .200`) — Estación de trabajo corporativa unida al dominio.
    
  * **SRV-GLPI (Ubuntu Server 24.04):** `192.168.10.3` — Pila LAMP y mesa de ayuda GLPI.

---

## Stack Tecnológico

* **Sistema Anfitrión:** Arch Linux (KDE Plasma / Wayland)
* **Virtualización:** VMware Workstation Pro
* **Sistemas Operativos:** Windows Server 2022 Standard, Windows 11 Pro, Ubuntu Server 24.04 LTS
* **Roles & Protocolos:** Active Directory (AD DS), DNS, DHCP, LAMP Stack, GLPI Help Desk, RDP, SSH
* **Automatización:** Python (Faker) y PowerShell

---

## Fases de Implementación

### Fase 1: Preparación del Hipervisor y Segmentación de Red
* Creación de la interfaz virtual `VMnet2` en modo **Host-Only** (`192.168.10.0/24`).
* Desactivación del servidor DHCP nativo de VMware para evitar conflictos de red.

<img width="621" height="606" alt="Captura 1 - Topología de Red" src="https://github.com/user-attachments/assets/cabe073c-d1ce-49d2-92d8-4a65630ae7d4" />


---

### Fase 2: Aprovisionamiento y Configuración Base de DC01

- Despliegue de máquina virtual con Windows Server 2022 Standard (Desktop Experience).
- Asignación de recursos: 4 GB RAM, 4 vCPUs y 40 GB de almacenamiento NVMe.
- Conexión del adaptador de red virtual a `VMnet2` en modo Host-Only.
- Configuración de dirección IP estática: `192.168.10.2/24`.
- Puerta de enlace predeterminada sin configurar debido al aislamiento de la red del laboratorio.

<img width="828" height="689" alt="Captura 2 - Hardware y enlace de la VM" src="https://github.com/user-attachments/assets/bf62aabb-3d4e-4e21-a13c-0904056626e2" />

La interfaz de red de `DC01` fue configurada con la dirección IP estática `192.168.10.2/24`, correspondiente a la red privada `192.168.10.0/24`.

<img width="724" height="374" alt="image" src="https://github.com/user-attachments/assets/a3070df7-0731-4857-bdcf-c935eb7b894c" />


