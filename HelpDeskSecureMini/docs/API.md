# Contratos de la API (pendientes de implementación)

Las siguientes rutas forman parte del acuerdo de diseño. Requieren JWT válido salvo `/health`.

| Método | Ruta | Responsabilidad |
|---|---|---|
| POST | `/api/me/bootstrap` | Crear perfil del usuario si no existe |
| GET | `/api/me` | Leer perfil autorizado |
| GET | `/api/reportes` | Listar / buscar reportes autorizados |
| GET | `/api/reportes/{id}` | Ver detalle autorizado |
| POST | `/api/reportes` | Crear reporte con 0 a 3 imágenes |
| GET | `/api/reportes/{id}/respuestas` | Consultar conversación |
| POST | `/api/reportes/{id}/respuestas` | Responder cuando el reporte no está cerrado |
| PATCH | `/api/admin/reportes/{id}/cerrar` | Cerrar reporte (administrador) |
| GET | `/api/reportes/{id}/adjuntos/{archivoId}/url` | Generar URL temporal autorizada |
| GET | `/health` | Estado superficial del servicio |

Los tipos y campos de cada DTO aparecen en el PDF. Los controladores llaman a los servicios de G3, sin consultar directamente la base de datos. No se aceptan `usuario_id`, `autor_id` ni `rol` arbitrarios del navegador.
