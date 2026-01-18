# DeepClean - Sistema de Autenticación Web

Página web estática para gestión de contraseñas de usuarios DeepClean.

## ✨ Funcionalidades

| Tipo            | Descripción                                   |
| --------------- | --------------------------------------------- |
| `type=invite`   | Activación de cuenta nueva (desde invitación) |
| `type=recovery` | Restablecimiento de contraseña olvidada       |

## 🎨 Características de Diseño

- ✅ **Línea gráfica DeepClean**: Colores primarios `#0077B6`, `#005A86`, `#00B4D8`
- ✅ **Logo y branding** integrado
- ✅ **Glassmorphism** con efectos de blur
- ✅ **Responsive** para móviles y desktop
- ✅ **Validación visual en tiempo real** de contraseña

## 🔐 Validación de Contraseña

La página valida visualmente estos requisitos:

| Requisito             | Indicador Visual      |
| --------------------- | --------------------- |
| Mínimo 8 caracteres   | ✓ Verde cuando cumple |
| Una mayúscula (A-Z)   | ✓ Verde cuando cumple |
| Un número (0-9)       | ✓ Verde cuando cumple |
| Un símbolo (!@#$%...) | ✓ Verde cuando cumple |

### Barra de Fortaleza

- 🔴 **Débil**: 1-2 requisitos
- 🟡 **Aceptable**: 3 requisitos
- 🟢 **Fuerte**: 4 requisitos (todos)

El botón solo se habilita cuando:

1. Todos los requisitos se cumplen
2. Las contraseñas coinciden

## 🚀 Despliegue

### Opción A: Vercel (Actual)

La página está desplegada en: `https://deep-clean.vercel.app`

### Opción B: GitHub Pages

1. Crea un repositorio dedicado: `deepclean-reset-password`
2. Copia esta carpeta
3. GitHub → Settings → Pages → Deploy from `main` branch
4. URL: `https://<USUARIO>.github.io/deepclean-reset-password/`

## ⚙️ Configuración Requerida

Ver [CONFIGURATION.md](CONFIGURATION.md) para instrucciones completas.

### Resumen Rápido

| Ubicación                           | Configurar                     |
| ----------------------------------- | ------------------------------ |
| Supabase → URL Configuration        | Site URL + Redirect URLs       |
| Supabase → Edge Functions → Secrets | `PASSWORD_WEB_URL`             |
| Mobile App → `.env.local`           | `EXPO_PUBLIC_PASSWORD_WEB_URL` |

## 🧪 Testing

### Test Reset Password

```
1. App → Login → "Olvidé mi contraseña"
2. Ingresar email → Recibir email
3. Click en link → Verificar página carga
4. Probar validación visual
5. Cambiar contraseña → Login exitoso
```

### Test Invitación

```
1. Admin → Users → Crear usuario
2. Nuevo usuario recibe email
3. Click en link → Página de activación
4. Establecer contraseña → Login exitoso
```

## 📁 Archivos

| Archivo            | Descripción                        |
| ------------------ | ---------------------------------- |
| `index.html`       | Página principal (HTML + CSS + JS) |
| `CONFIGURATION.md` | Guía completa de configuración     |
| `vercel.json`      | Configuración de Vercel            |
| `.env.example`     | Variables de entorno ejemplo       |
