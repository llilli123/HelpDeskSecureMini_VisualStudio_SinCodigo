# Seguridad: controles previstos (no implementados todavía)

Este archivo sirve para documentar la implementación y las evidencias reales. No marcar ningún control como completado hasta demostrarlo.

- [ ] SQL Injection: consultas parametrizadas y permisos mínimos.
- [ ] XSS: mostrar texto no confiable sin ejecutarlo y codificar según contexto.
- [ ] Contraseñas: gestionadas por Supabase Auth, sin contraseñas en las tablas del negocio.
- [ ] JWT y OAuth/OIDC: validar firma, emisor, audiencia, expiración y autorización por recurso.
- [ ] Rate Limiting y fuerza bruta: límites efectivos en API, Supabase Auth y rutas directas pertinentes.
- [ ] Command Injection: no construir ni ejecutar comandos con entradas del usuario.
- [ ] Archivos maliciosos: máximo 3 JPG/PNG de hasta 2 MiB, validación de contenido y Storage privado.
- [ ] IDOR/BOLA: un usuario nunca consulta reportes ni imágenes ajenos.
- [ ] Escalada de privilegios: rol administrativo protegido por permisos SQL y RLS.
- [ ] Reportes cerrados: rechazo en API y base de datos.

## Evidencias

Guardar capturas, resultados de pruebas y registros sin secretos ni datos reales en `docs/evidencias/`.

## Entorno de ataques autorizado

El otro equipo puede revisar la URL pública, usar cuentas normales de pruebas y leer el repositorio. Nunca publicar contraseñas administrativas, llaves privadas o tokens de sesión.
