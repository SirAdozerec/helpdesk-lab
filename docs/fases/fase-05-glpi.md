Quinta fase — Despliegue de GLPI (Mesa de Ayuda)

## Objetivo
El objetivo de esta fase fue desplegar un sistema de gestión de tickets (**GLPI**) que sea accesible desde `CLIENT01` y que permita a los empleados entrar usando sus cuentas habituales del dominio (Active Directory).

## Procedimiento

1.  **Preparación del servidor:** Desplegué una máquina virtual con **Ubuntu Server 26.04 LTS**.

2.  **Estrategia de red (Dual NIC):** Para mantener el aislamiento del laboratorio pero poder descargar paquetes, usé dos adaptadores: uno en `vmnet2` para la red interna y otro en modo **NAT** solo para internet.

3.  **Configuración de IP estática:** Mediante `netplan`, configuré la IP estática `192.168.10.10/24`, usando al DC01 (`192.168.10.2`) como DNS.

4.  **Montaje del Stack LAMP:** Instalé Apache, MariaDB y PHP 8.5 con todas las extensiones necesarias para que GLPI funcione correctamente.

5.  **Gestión de Base de Datos:** Creé la base de datos `glpi` y un usuario dedicado para el servicio, evitando usar la cuenta `root`.

6.  **Instalación de GLPI:** Instalé la versión 11 de GLPI y configuré el servidor web (Apache) para que la plataforma fuera accesible y segura.

7.  **Conexión con el Dominio (LDAP):** Vinculé GLPI con nuestro Active Directory para que los usuarios pudieran iniciar sesión con sus cuentas habituales del dominio. Para esto, configuré la comunicación con el DC01 y me aseguré de que el sistema reconociera correctamente los nombres de usuario de la red.

<div align="center">
  <img width="661" alt="Wizard de configuración de GLPI" src="https://github.com/user-attachments/assets/ff53ce23-e8f6-45dd-bfc8-df5c70540c88" />
  <br><em style="font-size: 0.9em;">Asistente de configuración LDAP completado con éxito.</em>
</div >

## Validación

Para confirmar que todo quedó funcionando, realicé las siguientes pruebas:

*   ✅ **Prueba de conectividad LDAP:** El asistente de GLPI pasó los 5 checks de validación (TCP stream, Base DN, LDAP URI, Bind connection y Search), encontrando correctamente a los usuarios del dominio.
*   ✅ **Autenticación de usuario de dominio:** Probé el acceso con la cuenta `victoria.alejandro`. El login fue exitoso sin necesidad de crear el usuario manualmente en GLPI, asignándole automáticamente su perfil.

<div align="center">
  <img width="1088" alt="Usuario de Active Directory logueado en GLPI" src="https://github.com/user-attachments/assets/aeba1f62-0580-4dd2-9855-ea8387c8cdd1" />
  <br><em style="font-size: 0.9em;">Validación de login: Usuario de AD autenticado correctamente.</em>
</div >

*   ✅ **Interfaz y flujo de soporte:** Una vez dentro, verifiqué que el Dashboard cargaba bien y que un usuario podía abrir un ticket sin problemas.

<div align="center">
  <img width="1882" alt="Dashboard de GLPI" src="https://github.com/user-attachments/assets/2a975c74-d37f-43c8-ba19-7407e24d465a" />
  <br><em style="font-size: 0.9em;">Vista general del Dashboard tras el primer inicio de sesión.</em>
</div >

<div align="center">
  <table style="border: none;">
    <tr style="border: none;">
      <td align="center" style="border: none;">
        <img width="450" alt="Formulario de creación de ticket" src="https://github.com/user-attachments/assets/c52d47ee-692f-4419-9d39-3447c1683383" />
        <br><em style="font-size: 0.8em;">Paso 1: Creación del ticket.</em>
      </td>
      <td align="center" style="border: none;">
        <img width="450" alt="Ticket creado por usuario de AD" src="https://github.com/user-attachments/assets/084e0867-779a-452e-8239-96530d539ea8" />
        <br><em style="font-size: 0.8em;">Paso 2: Ticket registrado en el sistema.</em>
      </td>
    </tr>
  </table>
</div >

## Troubleshooting (Retos y Soluciones)

*   **Conflicto de red (Aislamiento vs Internet):** Al estar en `vmnet2`, el comando `apt update` fallaba. Lo resolví usando un segundo adaptador NAT solo para las descargas.
*   **Error de ruta por defecto en Netplan:** Inicialmente, la ruta por defecto apuntaba al DC01 y bloqueaba internet; lo corregí para que la ruta fuera gestionada por el adaptador NAT.
*   **Dependencias de PHP:** Instalé manualmente `php-bcmath` (necesario para códigos QR) y confirmé que GLPI 11 ya no requiere `php-imap`.
*   **Incompatibilidad de versiones:** Como Ubuntu 26.04 trae PHP 8.5, instalé **GLPI 11**, ya que la versión 10 no era compatible con esa versión de PHP.
*   **Prioridad de autenticación LDAP:** Configuré el directorio LDAP como servidor predeterminado para que los usuarios de AD pudieran entrar directamente sin usar la base de datos interna.

## Notas Técnicas
*   **Mapeo de Atributos:** Se utilizó `samaccountname` para el login, ya que es el estándar en Active Directory.
*   **Provisionamiento Automático:** Los usuarios se crean automáticamente en GLPI al iniciar sesión por primera vez.
