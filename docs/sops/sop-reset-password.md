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

## Pendiente
- [ ] Escribir el SOP completo con capturas
