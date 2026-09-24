# Sexta fase — Despliegue de GLPI (Mesa de Ayuda)

## Objetivo
El objetivo de esta fase fue desplegar un sistema de gestión de servicios de TI (ITSM) mediante **GLPI**, asegurando que sea accesible desde `CLIENT01` y que permita la autenticación mediante **LDAP** contra nuestro Active Directory. Con esto, buscamos simular el entorno real de una mesa de ayuda corporativa.

## Procedimiento

1.  **Preparación del servidor:** Desplegué una máquina virtual con **Ubuntu Server 26.04 LTS**, asignándole 2 vCPUs y 4 GB de RAM para un rendimiento fluido.
2.  **Estrategia de red (Dual NIC):** Para mantener el aislamiento del laboratorio pero permitir la descarga de paquetes, utilicé una configuración de doble adaptador:
    *   **Adaptador 1:** Conectado a `vmnet2` (Host-Only) para la comunicación con el DC01 y los clientes.
    *   **Adaptador 2:** Configurado en modo **NAT**, usado exclusivamente para descargar actualizaciones y dependencias de internet sin comprometer la red aislada del lab.
3.  **Configuración de IP estática:** Mediante `netplan` (`/etc/netplan/00-installer-config.yaml`), configuré la IP estática `192.168.10.10/24`, estableciendo al DC01 (`192.168.10.2`) como servidor DNS principal.
4.  **Montaje del Stack LAMP:** Instalé el stack necesario (Apache, MariaDB y PHP 8.5), asegurándome de incluir todas las extensiones críticas para GLPI (`mysql`, `ldap`, `xml`, `mbstring`, `curl`, `gd`, `intl`, `zip`, `bz2` y `bcmath`).
5.  **Gestión de Base de Datos:** Creé la base de datos `glpi` en MariaDB y generé un usuario dedicado para el servicio, evitando así el uso de la cuenta `root` por seguridad.
6.  **Despliegue de GLPI 11:** Descargué la versión más reciente de GLPI 11 y lo desplegué en `/var/www/glpi`. Configuré un VirtualHost en Apache para que el `DocumentRoot` apuntara correctamente a la carpeta `/public`, siguiendo las buenas prácticas de seguridad.
7.  **Integración con Active Directory (LDAP):** En la configuración de GLPI, vinculé el directorio LDAP apuntando al DC01 (`192.168.10.2:389`). Configuré la `BaseDN` como `DC=corp,DC=local` y utilicé las credenciales de `CORP\Administrator` para el *bind*. Un punto clave fue mapear el campo de login al atributo `samaccountname`, que es el estándar en entornos Windows.

## Validación

Para confirmar que la implementación fue exitosa, realicé las siguientes pruebas:

*   ✅ **Prueba de conectividad LDAP:** El asistente de GLPI superó los 5 checks de validación (TCP stream, Base DN, LDAP URI, Bind connection y Search), logrando identificar correctamente las 11 entradas del dominio (10 empleados + Administrator).
*   ✅ **Autenticación de usuario de dominio:** Probé el acceso con la cuenta `victoria.alejandro` (creada previamente en AD). El login fue exitoso sin necesidad de crear el usuario manualmente en GLPI, y su perfil se asignó automáticamente como **Self-Service**.
*   ✅ **Flujo de soporte:** Verifiqué que el usuario pudiera abrir un ticket de soporte desde la interfaz sin errores.

<div align="center">
  <img width="661" height="740" alt="Wizard de configuración de GLPI" src="https://github.com/user-attachments/assets/ff53ce23-e8f6-45dd-bfc8-df5c70540c88" />
</div>

<div align="center">
  <img width="1882" height="905" alt="Dashboard de GLPI" src="https://github.com/user-attachments/assets/2a975c74-d37f-43c8-ba19-7407e24d465a" />
</div>

<div align="center">
  <img width="1297" height="794" alt="Formulario de creación de ticket" src="https://github.com/user-attachments/assets/c52d47ee-692f-4419-9d39-3447c1683383" />
</div>

<div align="center">
  <img width="1007" height="793" alt="Ticket creado por usuario de AD" src="https://github.com/user-attachments/assets/084e0867-779a-452e-8239-96530d539ea8" />
</div>

<div align="center">
  <img width="1088" height="734" alt="Usuario de Active Directory logueado en GLPI" src="https://github.com/user-attachments/assets/aeba1f62-0580-4dd2-9855-ea8387c8cdd1" />
</div>

## Troubleshooting (Retos y Soluciones)

Durante el despliegue me encontré con varios obstáculos técnicos que fueron clave para la configuración final:

*   **Conflicto de red (Aislamiento vs Internet):** Al estar en `vmnet2`, el comando `apt update` fallaba constantemente. Lo solucioné implementando la estrategia de **doble adaptador**, dejando el NAT solo para descargas y manteniendo el tráfico del lab en la red aislada.
*   **Error de ruta por defecto en Netplan:** Inicialmente, configuré una ruta por defecto hacia el DC01, lo que bloqueaba cualquier salida a internet. Corregí el archivo de configuración para que la ruta por defecto fuera gestionada por el adaptador NAT vía DHCP.
*   **Dependencias de PHP:** 
    *   Noté que `php-imap` ya no está disponible en Ubuntu 26.04, pero tras investigar, confirmé que GLPI 11 ya no lo requiere para su funcionamiento básico.
    *   Tuve que instalar manualmente `php-bcmath` para habilitar el soporte de códigos QR en la plataforma.
*   **El dilema de las versiones (PHP 8.5):** Ubuntu 26.04 trae PHP 8.5, lo cual causaba incompatibilidad con GLPI 10. Decidí saltar directamente a **GLPI 11**, que ya ofrece soporte oficial para esta versión de PHP.
*   **Prioridad de autenticación LDAP:** Al principio, GLPI intentaba validar todo contra la base de datos interna y rechazaba a los usuarios de AD. Corregí esto configurando el directorio LDAP como **servidor predeterminado** y activando la opción de "Agregar usuarios desde una fuente externa".

## Notas Técnicas
*   **Mapeo de Atributos:** Es vital recordar que en Active Directory se debe usar `samaccountname` para el login, ya que el atributo `uid` es propio de entornos OpenLDAP.
*   **Provisionamiento Automático:** Gracias a la configuración LDAP, los usuarios no necesitan ser importados manualmente; GLPI los crea "on-the-fly" en su primer inicio de sesión.

***

### ¿Qué mejoras hice?

1.  **Narrativa de resolución de problemas:** En el Troubleshooting, en lugar de solo listar el error, usé frases como *"Me encontré con..."*, *"Tras investigar, confirmé..."* o *"Decidí saltar directamente a..."*. Esto demuestra criterio técnico.
2.  **Estructura de "Retos":** Cambié el título de la sección a algo más profesional como "Retos y Soluciones".
3.  **Imágenes:** Las envolví en `<div align="center">` para que queden perfectamente centradas en GitHub, respetando los tamaños que me pasaste.
4.  **Claridad técnica:** Refiné la explicación de por qué usaste dos adaptadores de red (la estrategia de "Dual NIC"), lo cual hace que el lector entienda que fue una decisión de diseño y no un error.

¡Con esto tu proyecto va a parecer el de un Senior! :D
