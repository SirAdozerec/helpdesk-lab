# Fase 03 — Promoción del Dominio (`corp.local`)

## Objetivo
Convertir DC01 en el primer Controlador de Dominio del bosque `corp.local`.

## Procedimiento
1. Instalación del rol **AD DS** sobre DC01 vía Server Manager.
2. Ejecución del asistente de promoción: **Add a new forest** → `corp.local`.
3. Functional Level: **Windows Server 2016** para dominio y bosque.
4. Contraseña DSRM establecida.
5. Rutas por defecto para NTDS y SYSVOL.
6. Validación de prerrequisitos: todos los checks en verde.
7. Install → reinicio automático.

## Validación post-promoción

Comandos ejecutados en PowerShell como `CORP\Administrator`:

    Get-ADDomain
    Get-ADForest
    dcdiag /v
    Get-Service NTDS, ADWS, DNS, Netlogon, KDC
    net share

**Resultado:** el bosque `corp.local` quedó operativo con DC01 como único
Domain Controller, los 5 servicios críticos en Running, y todos los tests
de `dcdiag /v` en `passed`.

![Salida de Get-ADDomain](../../assets/05-get-addomain.png)

![Salida de dcdiag /v](../../assets/06-dcdiag.png)

![Salida de net share](../../assets/07-net-share.png)

## Notas / Troubleshooting

Durante la primera promoción, la instalación quedó incompleta: el servidor
reinició pero los servicios de AD nunca arrancaron (NTDS, ADWS, KDC y
Netlogon en `Stopped`). `ntdsutil` reportaba
`"The machine is not an Active Directory Domain Controller"`.

**Solución:** reinstalación completa del bosque con `Install-ADDSForest -Force`.
Tras el segundo reinicio, todos los servicios arrancaron correctamente y
`dcdiag /v` pasó sin errores.

**Lección aprendida:** apagar siempre la VM con `shutdown /s /t 0` desde
Windows (nunca con "Power Off" en VMware) y tomar un snapshot tras cada
hito importante.

## Snapshot
Se tomó un snapshot de VMware nombrado **"Fase 3 completada"** al finalizar esta fase.
