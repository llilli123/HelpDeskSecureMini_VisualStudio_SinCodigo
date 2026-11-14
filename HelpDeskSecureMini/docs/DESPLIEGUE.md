# Despliegue (pendiente de implementación)

## Cloudflare Pages

Alojar los archivos estáticos del frontend. Las únicas variables públicas deben ser la URL de la API, la URL de Supabase y la clave pública de aplicación.

## Render

Alojar la API ASP.NET Core mediante Docker. Conservar en secretos del servidor la configuración privada de Storage y los parámetros de validación JWT. Restringir CORS al origen real del frontend.

## Supabase

Crear las cuatro tablas (`perfiles`, `reportes`, `respuestas`, `adjuntos`), habilitar RLS, configurar los permisos SQL y las funciones RPC necesarias, habilitar Google OAuth y crear el bucket privado `reportes-evidencias`.

## Pendiente de registrar por el equipo

- URL pública del frontend:
- URL pública de la API:
- Identificador público de proyecto Supabase (no secretos):
- Fecha del despliegue:
- Responsable:
- Verificación de `/health`:
- Verificación de inicio de sesión:
- Verificación de RLS y URL privada de imágenes:

No registrar credenciales ni tokens activos en este documento.
