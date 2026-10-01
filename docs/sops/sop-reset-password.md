# SOP — Reset de contraseña

## Objetivo
Resetear la contraseña de un usuario de dominio cuando la olvida o está
bloqueado.

## Procedimiento
1. Verificar identidad del solicitante (política interna).
2. Abrir **Active Directory Users and Computers**.
3. Buscar al usuario.
4. Clic derecho → Reset Password.
5. Asignar contraseña temporal y marcar "User must change password at next logon".
6. Si la cuenta estaba bloqueada, desbloquearla.
7. Registrar en el ticket de GLPI.

## Verificación
- El usuario puede iniciar sesión.
- Se le solicita cambiar contraseña al primer logon.

## Evidencia

<p align="center">
  <img width="800" alt="Diálogo Reset Password en Active Directory Users and Computers" src="PON_AQUI_TU_LINK" />
</p>
<p align="center"><em>Reset de contraseña con "User must change password at next logon" marcado.</em></p>

<p align="center">
  <img width="800" alt="Usuario forzado a cambiar su contraseña en el primer inicio de sesión" src="PON_AQUI_TU_LINK" />
</p>
<p align="center"><em>Verificación: se solicita el cambio de contraseña al primer logon.</em></p>
