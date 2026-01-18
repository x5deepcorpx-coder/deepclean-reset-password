# Configuración Completa - Sistema de Autenticación DeepClean

Esta carpeta contiene la página web para:

- ✅ **Activar cuenta** (cuando un admin/coordinator invita a un usuario)
- ✅ **Restablecer contraseña** (cuando un usuario olvida su contraseña)

---

## 🔄 Flujos de Autenticación

### Flujo 1: Creación de Nuevo Usuario (Invitación)

```
Admin/Coordinator crea usuario → Supabase envía email de invitación
→ Usuario hace clic en link → Llega a esta página (type=invite)
→ Usuario establece contraseña → Listo para usar la app
```

### Flujo 2: Olvidé mi Contraseña

```
Usuario en login → Click "Olvidé contraseña" → Ingresa email
→ Supabase envía email de recuperación
→ Usuario hace clic en link → Llega a esta página (type=recovery)
→ Usuario establece nueva contraseña → Listo
```

---

## 🌐 1) Hosting con Vercel (Recomendado)

### Opción A: Vercel (más fácil)

1. Conecta este repo a Vercel
2. Deploy automático
3. URL: `https://deep-clean.vercel.app` (o tu dominio)

### Opción B: GitHub Pages

1. Crea un repo dedicado: `deepclean-reset-password`
2. Copia el contenido de esta carpeta
3. GitHub → Settings → Pages
   - Source: `Deploy from a branch`
   - Branch: `main`
   - Folder: `/ (root)`
4. URL: `https://<TU_USUARIO>.github.io/deepclean-reset-password/`

---

## ⚙️ 2) Configuración en Supabase Dashboard (CRÍTICO)

### 2.1 URL Configuration

Dashboard: https://supabase.com/dashboard/project/ykfhqeopolfhqdvckzuz/auth/url-configuration

Configura:

| Campo             | Valor                           |
| ----------------- | ------------------------------- |
| **Site URL**      | `https://deep-clean.vercel.app` |
| **Redirect URLs** | `https://deep-clean.vercel.app` |

> ⚠️ Sin estas URLs configuradas, los emails NO redirigirán correctamente.

### 2.2 Email Templates (Opcional pero recomendado)

Dashboard: https://supabase.com/dashboard/project/ykfhqeopolfhqdvckzuz/auth/templates

Puedes personalizar los templates de email para:

- **Invite User** (invitación de nuevo usuario)
- **Reset Password** (recuperación de contraseña)

### 2.3 Edge Functions → Secrets

Dashboard: Project Settings → Edge Functions → Secrets

| Secret                      | Valor                           |
| --------------------------- | ------------------------------- |
| `SUPABASE_SERVICE_ROLE_KEY` | (ya configurado)                |
| `PASSWORD_WEB_URL`          | `https://deep-clean.vercel.app` |

> Nota: La Edge Function `invite-user` usa `PASSWORD_WEB_URL` para el `redirectTo` del email.

---

## 📱 3) Configuración en App Móvil

En `Dev/mobile-app/.env.local`:

```env
EXPO_PUBLIC_PASSWORD_WEB_URL=https://deep-clean.vercel.app
```

Este valor se usa en:

- `ForgotPasswordModal.tsx` → `resetPasswordForEmail({ redirectTo })`
- `EditUserModal.tsx` → Cuando admin envía reset password
- `UserDetailsModal.tsx` → Cuando admin envía reset password
- `EditCleanerModal.tsx` → Cuando coordinator envía reset password

---

## 🔐 4) Validación de Contraseña

La página web valida visualmente que la contraseña cumpla:

| Requisito            | Regex                  |
| -------------------- | ---------------------- |
| Mínimo 8 caracteres  | `password.length >= 8` |
| Al menos 1 mayúscula | `/[A-Z]/`              |
| Al menos 1 número    | `/[0-9]/`              |
| Al menos 1 símbolo   | `/[^a-zA-Z0-9]/`       |

El botón de submit solo se habilita cuando:

- ✅ Todos los requisitos se cumplen
- ✅ Las contraseñas coinciden

---

## 🧪 5) Testing

### Probar Reset Password:

1. Ir a la app → Login → "Olvidé mi contraseña"
2. Ingresar un email registrado
3. Revisar bandeja de entrada
4. Click en el link del email
5. Verificar que la página carga correctamente
6. Probar la validación visual de contraseña
7. Establecer nueva contraseña

### Probar Invitación:

1. Como Admin → Users → Crear nuevo usuario
2. El nuevo usuario recibirá email de invitación
3. Click en link → Debe llegar a esta página
4. Establecer contraseña
5. Login con las nuevas credenciales

---

## 🐛 Troubleshooting

### "Link inválido o expirado"

- El token expira después de 24 horas
- Solicitar nuevo link desde la app

### No llega el email

- Verificar spam/junk folder
- Verificar que el email esté registrado en Supabase
- Revisar Supabase Dashboard → Auth → Logs

### Error al actualizar contraseña

- Verificar que el token no haya expirado
- Verificar conexión a internet
- Revisar consola del navegador para más detalles
