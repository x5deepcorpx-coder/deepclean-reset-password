# Pruebas End-to-End (Invitar usuario + Olvidé contraseña)

Estas pruebas validan que:
- Al crear un usuario nuevo (invite) llegue un email con link
- El link abre la web y permite establecer contraseña
- El usuario puede iniciar sesión en la app
- El flujo de “Olvidé contraseña” también envía email y permite cambiar contraseña

## A) Prueba 1: Crear usuario (Invitación)

1. Asegúrate de tener configurado:
   - [CONFIGURATION.md](CONFIGURATION.md)
   - `PASSWORD_WEB_URL` (Edge Secrets)
   - `EXPO_PUBLIC_PASSWORD_WEB_URL` (app)
   - Supabase Auth URL Configuration (Site URL + Redirect URLs)

2. En la app (sesión Admin/Supervisor/Manager):
   - Usuarios → Crear usuario
   - Email: usa un email real que puedas abrir

3. Esperado:
   - Llega email “Invite” de Supabase
   - Al abrir el link, se abre la web de GitHub Pages
   - Pide “Nueva contraseña” y “Confirmar contraseña”
   - Acepta solo si cumple la dificultad
   - Muestra “¡Contraseña actualizada!”

4. Finalmente:
   - Abre la app → Login con el email del usuario y la nueva contraseña

## B) Prueba 2: Olvidé contraseña

1. En el login de la app:
   - “¿Olvidaste tu contraseña?”
   - Ingresa el email del usuario

2. Esperado:
   - Llega email “Reset password”
   - Al abrir el link, se abre la misma web (GitHub Pages)
   - Permite establecer la nueva contraseña con las mismas reglas

## C) Diagnóstico rápido (si no llega el email)

- Revisa en Supabase:
  - Authentication → Logs
  - Authentication → Users (que el usuario exista)
  - Email provider habilitado

- Revisa URL Configuration:
  - Site URL y Redirect URLs deben incluir exactamente la URL de GitHub Pages (con `/` final)

- Revisa Edge Secrets:
  - `PASSWORD_WEB_URL` debe ser https y coincidir
