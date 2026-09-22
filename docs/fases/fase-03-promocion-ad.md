# Tercera fase — Promoción del Dominio (`corp.local`)

## Objetivo
Establecer el bosque `corp.local` mediante la promoción de DC01 como el primer **Domain Controller**.

## Procedimiento
1. Comencé instalando el rol **AD DS (Active Directory Domain Services)** en DC01 a través de **Server Manager**.
2. Inicié el asistente de promoción seleccionando la opción **"Add a new forest"** para crear el dominio `corp.local`.
3. Configuré el **Forest and Domain Functional Level** en Windows Server 2016.
4. Definí la contraseña de recuperación **DSRM** y mantuve las rutas por defecto para los directorios **NTDS** y **SYSVOL**.
5. Verifiqué que todos los "prerequisites" aparecieran en verde y, tras confirmar que todo estaba listo, ejecuté la instalación hasta el reinicio automático.

## Validación post-promoción

Para confirmar que el dominio quedó correctamente configurado, ejecuté los siguientes comandos en **PowerShell** con la cuenta `CORP\Administrator`:

```powershell
Get-ADDomain
Get-ADForest
dcdiag /v
Get-Service NTDS, ADWS, DNS, Netlogon, KDC
net share
```

**Resultado:** El bosque `corp.local` se encuentra operativo con DC01 como único **Domain Controller**. Los servicios críticos (**NTDS, ADWS, DNS, Netlogon, KDC**) están en estado `Running` y las pruebas de `dcdiag /v` arrojaron resultados satisfactorios (`passed`).


<p align="center">
  <img width="1009" height="715" alt="Captura 1 - Descripción" src="https://github.com/user-attachments/assets/a2df57c5-b745-4518-83ec-8efc6c34aa68" />
</p>

<p align="center">
  <img width="1009" height="715" alt="Captura 2 - Descripción" src="https://github.com/user-attachments/assets/d73e2017-8eae-490b-8673-52433175f524" />
</p>

<p align="center">
  <img width="762" height="358" alt="Captura 3 - Descripción" src="https://github.com/user-attachments/assets/b4a8b542-18f2-4b23-9696-3cb4446287ae" />
</p>



## Resolución de problemas/Troubleshooting:

Durante el primer intento de promoción, la instalación falló después del reinicio: los servicios de Active Directory (NTDS, ADWS, KDC y Netlogon) se quedaron en estado `Stopped` y `ntdsutil` reportaba que la máquina no era un controlador de dominio.

**Solución:** Realicé una limpieza y volví a ejecutar la promoción de forma forzada con el comando `Install-ADDSForest -Force`. Tras el segundo reinicio, los servicios arrancaron correctamente y el sistema quedó estable.
