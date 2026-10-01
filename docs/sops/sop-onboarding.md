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
  <img width="800" alt="Asistente New User en Active Directory Users and Computers llenando nombre, apellido y username" src="PON_AQUI_TU_LINK" />
</p>
<p align="center"><em>Alta del usuario en ADUC con el formato nombre.apellido.</em></p>

<p align="center">
  <img width="800" alt="Usuario agregado a los grupos de seguridad correspondientes al departamento" src="PON_AQUI_TU_LINK" />
</p>
<p align="center"><em>Membresías de grupo asignadas tras el alta.</em></p>

<p align="center">
  <img width="800" alt="Inicio de sesión exitoso en CLIENT01 con las credenciales del nuevo usuario" src="PON_AQUI_TU_LINK" />
</p>
<p align="center"><em>Verificación: el usuario nuevo inicia sesión en CLIENT01 sin problema.</em></p>
