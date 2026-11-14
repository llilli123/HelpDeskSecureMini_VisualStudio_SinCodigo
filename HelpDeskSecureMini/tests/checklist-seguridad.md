# Lista de verificación de seguridad (pendiente)

- [ ] Acceso sin JWT a rutas protegidas rechazado.
- [ ] JWT manipulado o expirado rechazado.
- [ ] Usuario A no puede ver reportes ni imágenes de B.
- [ ] Usuario no puede invocar acciones administrativas.
- [ ] Usuario no puede cambiar su rol directamente a través de Supabase.
- [ ] Texto HTML en comentarios no se ejecuta.
- [ ] Búsqueda de reportes no ejecuta SQL inyectado.
- [ ] Se rechazan archivos no válidos o de tamaño excesivo.
- [ ] Se acepta un reporte con 0 a 3 imágenes válidas.
- [ ] Fallo parcial de subida activa limpieza compensatoria.
- [ ] No se pueden responder reportes cerrados ni llamar directamente a Supabase para evadirlo.
- [ ] Rate Limiting produce respuesta controlada en la API.
- [ ] No existe ejecución de comandos arbitrarios.
- [ ] Ningún secreto ni contraseña real aparece en repositorio, errores o capturas.

Registrar el resultado real, fecha y evidencia una vez realizados los ensayos.
