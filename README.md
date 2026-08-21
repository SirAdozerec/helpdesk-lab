# 🏢 Enterprise Help Desk & Hybrid Infrastructure Homelab

Documentación técnica, scripts de automatización y procedimientos operativos estándar (SOPs) de un entorno corporativo simulado desde cero en **Arch Linux** utilizando **VMware Workstation Pro**.

---

## 📐 Topología y Arquitectura de Red

Toda la infraestructura local opera dentro del segmento privado aislado **Host-Only** (`192.168.10.0/24`) con el servicio DHCP de VMware desactivado para delegar la administración central a Windows Server.

<img width="2720" height="1920" alt="homelab_helpdesk_topology (1)" src="https://github.com/user-attachments/assets/7220c070-ff4a-49be-9174-7602813ca70d" />


* **Subred privada:** `192.168.10.0/24`
* **Nodos del laboratorio:**
  * **DC01 (Windows Server 2022):** `192.168.10.2` — Controlador de Dominio (`corp.local`), DNS y servidor DHCP.
  * **CLIENT01 (Windows 11 Pro):** Asignación dinámica (`192.168.10.100 - .200`) — Estación de trabajo corporativa unida al dominio.
  * **SRV-GLPI (Ubuntu Server 24.04):** `192.168.10.3` — Pila LAMP y mesa de ayuda GLPI.

---

## 🛠️ Stack Tecnológico

* **Sistema Anfitrión:** Arch Linux (KDE Plasma / Wayland)
* **Virtualización:** VMware Workstation Pro
* **Sistemas Operativos:** Windows Server 2022 Standard, Windows 11 Pro, Ubuntu Server 24.04 LTS
* **Roles & Protocolos:** Active Directory (AD DS), DNS, DHCP, LAMP Stack, GLPI Help Desk, RDP, SSH
* **Automatización:** Python (Faker) y PowerShell

---

## 🚀 Fases de Implementación

### Fase 1: Preparación del Hipervisor y Segmentación de Red
* Creación de la interfaz virtual `VMnet2` en modo **Host-Only** (`192.168.10.0/24`).
* Desactivación del servidor DHCP nativo de VMware para evitar conflictos de red.

<!-- 2. ARRASTRA AQUÍ LA CAPTURA 1 (Virtual Network Editor) -->

---

### Fase 2: Aprovisionamiento y Configuración Base de DC01
* Despliegue de máquina virtual con Windows Server 2022 Standard (Desktop Experience).
* Asignación de recursos: 4 GB RAM, 4 vCPUs, 40 GB NVMe.
* Enlace explícito del adaptador de red a la interfaz aislada `/dev/vmnet2`.

<!-- 3. ARRASTRA AQUÍ LA CAPTURA 2 (Hardware y enlace de la VM) -->
