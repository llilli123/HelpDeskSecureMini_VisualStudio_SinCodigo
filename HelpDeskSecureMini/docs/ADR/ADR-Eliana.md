# ADR-G2-01 - Estructura de páginas estáticas con plantillas y diseño minimalista accesible

**Autor:** Eliana  
**Grupo:** G2 (Frontend)  
**Rama:** `feature/G2/Frontend-Eliana`  
**Fecha:** 16 de noviembre de 2026  
**Estado:** Propuesto (pendiente de integración con el JavaScript de Alejandro y revisión de Jorge)

## Tema

Estructura de páginas y diseño minimalista accesible.

## Contexto

El frontend de HelpDesk Secure Mini se publica como sitio estático en Cloudflare Pages y debe cubrir cuatro pantallas
(acceso, mis reportes, detalle y administración). La lógica (sesión Google, JWT, llamadas a la API, búsqueda y
renderizado de datos) la implementa Alejandro en `js/`. Mi responsabilidad es el HTML y el CSS.

Había tres problemas concretos que resolver:

1. **Trabajo en paralelo:** Alejandro necesita saber desde el primer día qué elementos existen y cómo se llaman, sin
   esperar a que yo termine el diseño, y yo no debo escribir lógica de su módulo.
2. **XSS:** títulos, descripciones, respuestas, nombres de autor y nombres de archivo son texto controlado por usuarios.
   El riesgo aparece cuando el JS construye HTML con cadenas (`innerHTML`, plantillas de texto).
3. **Usabilidad y accesibilidad:** el profesor y otro equipo probarán la aplicación en distintos dispositivos; las
   pantallas deben ser responsivas, navegables con teclado y comunicar estados (carga, vacío, error, cerrado).

## Alternativas consideradas

| Opción | Ventajas | Desventajas |
|---|---|---|
| A. Framework (React/Vue) o librería CSS (Bootstrap/Tailwind) desde CDN | Componentes listos | Más dependencias de terceros en una app que guarda un JWT en el navegador; requiere relajar la CSP (`script-src`/`style-src` externos); fuera del alcance acordado (HTML/CSS/JS). |
| B. Páginas casi vacías y que el JS genere todo el marcado con `innerHTML` | Menos HTML | Mezcla estructura y lógica; facilita errores de XSS; dificulta revisar accesibilidad. |
| C. **HTML semántico completo + `<template>` por cada elemento repetido + CSS propio sin dependencias** | El HTML define toda la estructura; el JS solo clona plantillas y asigna `textContent`; CSP estricta posible | Hay que mantener un contrato de IDs entre HTML y JS; el encabezado se repite en tres páginas (no hay sistema de build). |

## Decisión tomada

Se eligió la **opción C**:

- **Cuatro páginas HTML semánticas** (`header`, `nav`, `main`, `section`, `article`, `table`, `dl`, `ol`, `dialog`)
  con IDs estables documentados abajo. Cada `<body>` indica su pantalla con `data-pagina`.
- **Plantillas `<template>`** para filas de tabla, respuestas de la conversación, adjuntos y vista previa de imágenes.
  Cada hueco a rellenar lleva `data-campo="..."`, de modo que el JS hace
  `fila.querySelector('[data-campo="titulo"]').textContent = dto.titulo` y **nunca** necesita `innerHTML`.
- **Estados por atributos, no por clases generadas en JS:** `hidden` para mostrar/ocultar bloques, `data-estado` con el
  valor exacto de la API (`Pendiente`, `En proceso`, `Cerrado`) para las insignias, `aria-invalid` para campos con error,
  `data-solo-admin` / `data-solo-usuario` para elementos dependientes del rol.
- **CSP en `<meta>`** en las cuatro páginas: `script-src 'self'`, `style-src 'self'`, sin estilos ni scripts en línea,
  `object-src 'none'`, `base-uri 'none'`. Por eso el CSS no usa `style=""` y las páginas no tienen `<script>` en línea.
  `<meta name="referrer" content="no-referrer">` evita filtrar URLs (incluidas las firmadas) en el encabezado Referer.
- **CSS propio sin dependencias**, con variables de diseño, modo oscuro automático (`prefers-color-scheme`), foco
  visible, enlace «Saltar al contenido», `prefers-reduced-motion`, `forced-colors` y diseño de una columna en móvil
  (las tablas pasan a tarjetas con `data-etiqueta`).
- **Límites del formulario alineados con los DTO** (`CrearReporteDto`, `CrearRespuestaDto`): título 150, descripción 4000,
  respuesta 2000, imágenes `accept="image/png,image/jpeg"` y texto de ayuda «hasta 3, máx. 2 MiB». Son ayudas de UX: la
  validación real está en la API (G1/G5) y en la base de datos (G4).

## Contrato con `js/` (Alejandro)

| Página | Elementos clave |
|---|---|
| `index.html` | `#btn-google`, `#form-prueba` (`#prueba-correo`, `#prueba-clave`, `#error-prueba`), `#mensaje-sesion`, `#cargando-sesion` |
| Común (3 páginas internas) | `#usuario-nombre`, `#usuario-rol`, `#btn-cerrar-sesion`, `#mensaje-pagina`, `[data-solo-admin]`, `[data-solo-usuario]` |
| `reportes.html` | `#form-reporte` (`titulo`, `descripcion`, `imagenes`), `#contador-descripcion`, `#lista-imagenes` + `#tpl-vista-previa`, `#error-imagenes`, `#error-reporte`, `#btn-enviar-reporte`; `#form-busqueda` (`search`), `#cuerpo-reportes` + `#tpl-fila-reporte`, `#cargando-reportes`, `#vacio-reportes`, `#error-reportes`, `#btn-pagina-anterior`, `#btn-pagina-siguiente`, `#indicador-pagina` |
| `detalle.html` | `#detalle-reporte`, `#detalle-titulo`, `#detalle-estado`, `#detalle-creado`, `#detalle-autor`, `#detalle-id`, `#detalle-descripcion`, `#lista-adjuntos` + `#tpl-adjunto` (`[data-accion="ver-adjunto"]`), `#lista-respuestas` + `#tpl-respuesta`, `#form-respuesta` (`mensaje`), `#contador-respuesta`, `#aviso-cerrado`, `#btn-cerrar-reporte`, `#dialogo-cerrar`, `#cargando-detalle`, `#error-detalle`, `#enlace-volver` |
| `admin.html` | `#panel-admin`, `#acceso-denegado`, `#form-busqueda-admin`, `#cuerpo-admin` + `#tpl-fila-admin` (`[data-accion="cerrar"]`), `#dialogo-cerrar`, `#dialogo-cerrar-nombre`, paginación `#btn-admin-anterior` / `#btn-admin-siguiente` |

Atributos que el CSS interpreta: `data-estado` en `.estado`, `data-propio="true"` y `data-rol="Administrador"` en
`.mensaje`, `data-limite="cerca|excedido"` en `.contador`, `aria-disabled="true"` en `.zona-archivos`, clase
`arrastrando` en `.zona-archivos` durante drag & drop, clase `aviso--exito` / `aviso--error` en `#mensaje-pagina`.

El enlace al detalle se forma como `detalle.html?id=<uuid>`; el UUID debe asignarse con `URLSearchParams` /
`encodeURIComponent`, nunca concatenando HTML.

## Consecuencias y riesgos

**Ventajas**

- Alejandro y yo trabajamos en paralelo con un contrato explícito; revisar su PR consiste en comprobar que usa los IDs
  y `textContent`.
- La prueba de aceptación T08 (HTML con script se muestra como texto) se apoya en dos capas: renderizado por
  `textContent` y CSP que bloquea scripts en línea.
- Sin dependencias externas en la interfaz: menos superficie de cadena de suministro y carga rápida en Cloudflare Pages.

**Limitaciones y riesgos**

- **Ocultar elementos no es seguridad.** `data-solo-admin` solo mejora la experiencia; la API (403) y RLS deben rechazar
  cualquier acción administrativa de un usuario normal. Esto se indica en comentarios del HTML.
- **CSP en `<meta>`** no admite `frame-ancestors` ni reportes; la protección contra clickjacking debe añadirse como
  cabecera HTTP (archivo `_headers` de Cloudflare o configuración de G5) previa revisión de integración.
- `connect-src 'self' https:` es amplio porque aún no se conocen los dominios definitivos de Render y Supabase. **Antes
  de la release** se debe restringir a esos dos orígenes.
- Si Alejandro decide cargar `supabase-js` desde un CDN, hay que añadir ese origen exacto a `script-src` (con SRI). Si
  usa los endpoints REST de Supabase Auth directamente, la CSP actual no cambia.
- El encabezado está duplicado en `reportes.html`, `detalle.html` y `admin.html`; un cambio de navegación exige editar
  los tres archivos.
- `:has()` (ocultar tablas vacías) no existe en navegadores muy antiguos; en ese caso solo se ve la cabecera de la tabla
  vacía, sin afectar la funcionalidad.

**Dependencias**

- G1 (John): nombres finales de campos de `ReporteDto`, `ReporteDetalleDto`, `RespuestaDto`, `PerfilDto`.
- G2 (Alejandro): `config.js`, `auth.js`, `api.js`, `reportes.js`, `detalle.js`, `admin.js`.
- G5 (Jorge/Rachel): cabeceras HTTP finales del sitio y dominios para la CSP.

## Implementación y archivos propios

- `src/frontend/index.html` - Pantalla de acceso: botón «Continuar con Google», acceso con cuenta de prueba en
  `<details>`, mensajes de sesión y estado de carga del bootstrap.
- `src/frontend/reportes.html` - Formulario de nuevo reporte (título, descripción con contador, zona de 0-3 imágenes con
  vista previa), listado con búsqueda por título y paginación; columna de autor visible solo para administrador.
- `src/frontend/detalle.html` - Título, estado, metadatos, descripción, galería de adjuntos con URL temporal bajo
  demanda, conversación cronológica, formulario de respuesta (oculto si está Cerrado), aviso de cerrado y cierre con
  diálogo de confirmación para el administrador.
- `src/frontend/admin.html` - Listado global con autor, búsqueda, acceso a detalle, botón de cierre con confirmación
  y aviso de acceso denegado.
- `src/frontend/css/styles.css` - Sistema visual completo: variables, modo oscuro, componentes, insignias de estado,
  tablas responsivas, accesibilidad.
- `docs/ADR/ADR-Eliana.md` - Este documento.

**Comportamiento observable sin JS:** las páginas cargan, muestran la estructura y los formularios, sin datos. Con JS,
los bloques se activan quitando `hidden` y clonando plantillas.

**Pendiente de verificar tras integrar con Alejandro** (no se marca como probado todavía): navegación completa con
teclado, lector de pantalla en formulario y conversación, vista en móvil de 360 px, T08 con un comentario
`<script>alert(1)</script>`.

## Pull Request de integración

(Completar con el enlace del PR `feature/G2/Frontend-Eliana` → `develop` una vez revisado por Alejandro y fusionado por Jorge.)
