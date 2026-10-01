# SOP — Onboarding de usuario nuevo

## Objetivo
Crear una cuenta de usuario en Active Directory y proporcionar los accesos
iniciales necesarios.

## Pre-requisitos
- Estar logueado como administrador de dominio en DC01.
- Tener un ticket abierto en GLPI solicitando el alta del usuario.

## Procedimiento
1. Abrir **Active Directory Users and Computers** en DC01.
2. Navegar a la OU correspondiente al departamento.
3. Clic derecho → New → User.
4. Llenar nombre, apellido, y username (formato: `nombre.apellido`).
5. Asignar contraseña temporal y marcar "User must change password at next logon".
6. Agregar el usuario a los grupos necesarios.
7. Registrar la acción en el ticket de GLPI y cerrarlo.

## Verificación
- El usuario puede iniciar sesión en CLIENT01 con sus credenciales.
- Pertenece a los grupos correctos.

## Evidencia

<p align="center">
  <img width="752" height="531" alt="Alta del usuario en ADUC" src="https://github.com/user-attachments/assets/74af4606-4720-4105-8f29-7237d51b728c" />
</p>
<p align="center"><em>Alta del usuario en ADUC con el formato nombre.apellido.</em></p>

<br></br>

<p align="center">
  <img width="756" height="536" alt="Verificación de membresías de grupo" src="https://github.com/user-attachments/assets/0eefd16e-b838-42c8-b480-fb1038d40b09" />
</p>
<p align="center"><em>Verificación de las membresías de grupo del usuario.</em></p>

<br></br>

<div align="center">
  <table style="border: none;">
    <tr style="border: none;">
      <td align="center" style="border: none; padding: 8px;">
        <img width="300" alt="Pantalla de login de Windows 11" src="https://github.com/user-attachments/assets/4bdfbf70-823f-4362-8c9e-f0f03252e7e9" />
      </td>
      <td align="center" style="border: none; padding: 8px;">
        <img width="400" alt="Sesión iniciada con el usuario nuevo" src="https://github.com/user-attachments/assets/13608882-baad-455d-a11d-e91f8f9cffcb" />
      </td>
    </tr>
  </table>
</div>
<p align="center"><em>Verificación: el usuario nuevo inicia sesión en CLIENT01 sin problema.</em></p>
