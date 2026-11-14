# HelpDesk Secure Mini

Proyecto universitario: prototipo de reportes de soporte técnico, organizado para 10 integrantes en 5 parejas.

## Importante

**Este ZIP contiene la estructura con solución de Visual Studio y proyecto .NET 10 válido.** Los archivos de código `.cs`, `.html`, `.css` y `.js` siguen vacíos. No hay implementación de la API, por lo que todavía no compila ni se puede ejecutar. No contiene credenciales. Cada integrante debe programar su parte y escribir su ADR.

La especificación de referencia se encuentra en `docs/Especificacion_Proyecto.pdf`.

## Arquitectura acordada

- **Frontend:** HTML, CSS y JavaScript, desplegado en Cloudflare Pages.
- **API:** ASP.NET Core Web API (C#), desplegada con Docker en Render.
- **Datos:** Supabase PostgreSQL a través de REST y políticas RLS.
- **Identidad:** Supabase Auth con Google OAuth/OIDC y JWT validado por la API.
- **Imágenes:** Supabase Storage privado; máximo 3 imágenes JPG/PNG de hasta 2 MiB por reporte.

## Reglas de negocio

Dos roles: Usuario y Administrador. Cuatro pantallas: acceso, reportes, detalle y administración.
Estados del reporte: Pendiente, En proceso y Cerrado. No se pueden modificar ni responder reportes cerrados.

## Equipos

| Grupo | Integrantes | Módulo |
|---|---|---|
| G1 | Johansell y John | Controladores, DTOs y contratos de API |
| G2 | Eliana y Alejandro | HTML, CSS, JavaScript y consumo de API |
| G3 | Albert y Junior | Servicios y reglas de negocio |
| G4 | Angel y Chamo | Supabase, RLS, autenticación y Storage |
| G5 | Jorge y Rachel | Seguridad transversal, validaciones, despliegue e integración |

**Jorge es el único responsable de fusionar Pull Requests a `develop`.** Cada integrante trabaja en su propia rama `feature/Gn/...` y redacta un ADR individual en `docs/ADR/`.

## Abrir en Visual Studio Community

1. Usar **Visual Studio Community 2026** con **ASP.NET y desarrollo web** y SDK **.NET 10** instalados.
2. Extraer el ZIP y abrir `HelpDeskSecureMini.sln` desde Archivo > Abrir > Proyecto o solución.
3. El proyecto `HelpDesk.Api` aparecerá en el Explorador de soluciones.
4. `Program.cs` está vacío intencionalmente. **La solución abre pero no compila ni arranca** hasta que el equipo implemente el punto de entrada y sus dependencias.
5. Para ver todas las carpetas, abrir la carpeta raíz completa en VS Code.

## Cómo comenzar

1. Crear el repositorio GitHub a partir de esta carpeta.
2. Crear las ramas `main`, `develop` y las 10 ramas individuales listadas en `docs/ARQUITECTURA.md`.
3. Leer la especificación y fijar los contratos G1/G3/G4 antes de implementar.
4. Completar el código y las configuraciones vacías; no añadir secretos al repositorio.
5. Abrir Pull Requests contra `develop`; Jorge revisa y fusiona.
6. Actualizar los ADR con decisiones reales, no afirmaciones de pruebas aún no ejecutadas.

## Archivos vacíos por diseño

Los archivos `.cs`, `.html`, `.css`, `.js`, `.sql`, `Dockerfile` y `.env.example` están vacíos. `.sln`, `.csproj` y `appsettings.json` contienen solo la estructura mínima necesaria para que Visual Studio reconozca el proyecto. Son espacios reservados para el trabajo de los integrantes, no una aplicación lista para publicar.
