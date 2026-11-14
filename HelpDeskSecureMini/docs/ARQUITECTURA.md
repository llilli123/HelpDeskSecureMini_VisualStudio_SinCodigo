# Arquitectura y organización

## Flujo acordado

Navegador (Cloudflare Pages) → API ASP.NET Core (Render) → Supabase REST (PostgreSQL con RLS) y Storage privado. El navegador se autentica con Supabase Auth y envía el JWT a la API; la API verifica la identidad, las reglas y los permisos antes de operar.

## Capas

- G1: controladores de API y DTOs.
- G2: páginas e integración del navegador.
- G3: servicios y lógica del negocio.
- G4: base de datos, políticas, cliente REST y Storage.
- G5: JWT, Rate Limiting, archivos, manejo de errores, Docker y despliegue.

## Ramas individuales acordadas

| Grupo | Rama | Responsable |
|---|---|---|
| G1 | `feature/G1/api-johansell` | Johansell |
| G1 | `feature/G1/dtos-john` | John |
| G2 | `feature/G2/frontend-eliana` | Eliana |
| G2 | `feature/G2/integracion-alejandro` | Alejandro |
| G3 | `feature/G3/servicios-albert` | Albert |
| G3 | `feature/G3/respuestas-junior` | Junior |
| G4 | `feature/G4/database-angel` | Angel |
| G4 | `feature/G4/storage-chamo` | Chamo |
| G5 | `feature/G5/seguridad-jorge` | Jorge |
| G5 | `feature/G5/validaciones-rachel` | Rachel |

Las ramas no se crean al extraer el ZIP: deben crearse en Git una vez inicializado el repositorio.

## Regla de integración

Solo Jorge fusiona cambios a `develop`. La pareja revisa su código y presenta el Pull Request. Ningún integrante añade claves privadas, tokens activos o credenciales al repositorio.

Consultar el diagrama y las dependencias detalladas en `Especificacion_Proyecto.pdf`.
