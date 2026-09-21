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

## Pendiente
- [ ] Escribir el SOP completo con capturas
