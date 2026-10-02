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

<div align="center">
  <table style="border: none;">
    <tr style="border: none;">
      <td align="center" style="border: none; padding: 8px;">
        <img width="450" alt="Diálogo Reset Password en ADUC" src="https://github.com/user-attachments/assets/2fd7493a-b288-45ee-91be-e9cd03b4550a" />
      </td>
      <td align="center" style="border: none; padding: 8px;">
        <img width="450" alt="Confirmación de cambio de contraseña" src="https://github.com/user-attachments/assets/e4eb1781-10c8-4637-ac02-45c21ad2536b" />
      </td>
    </tr>
  </table>
  <br><em style="font-size: 0.9em;">Reset de contraseña en ADUC. </em>
</div>

<br></br>

<div align="center">
  <table style="border: none;">
    <tr style="border: none;">
      <td align="center" style="border: none; padding: 8px;">
        <img width="350" alt="Aviso de cambio de contraseña obligatorio" src="https://github.com/user-attachments/assets/dfdf72c7-ef64-4129-8f6b-4e0396072067" />
      </td>
      <td align="center" style="border: none; padding: 8px;">
        <img width="350" alt="Pantalla de cambio de contraseña en CLIENT01" src="https://github.com/user-attachments/assets/2f21d56b-fe58-4fa6-ba9c-0a172e7d26a3" />
      </td>
    </tr>
  </table>
  <br><em style="font-size: 0.9em;">Verificación: se solicita el cambio de contraseña al primer logon.</em>
</div>
