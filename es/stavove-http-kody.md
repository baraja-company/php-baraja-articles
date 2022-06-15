Códigos de estado HTTP
======================

> id: '97800bb6-004c-4bdb-b126-f87e77cc5120'
> slug:
> 	cs: stavove-http-kody
> 	es: codigos-de-estado-http
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '6c1da4a7233c1eba531e9336daae5acb'

En la comunicación HTTP se transmiten los llamados "códigos de estado", que son información sobre cómo se ha realizado la transferencia. Estoy seguro de que sabes que un código `200` significa éxito y un código `404` significa una página inexistente.

Los códigos de estado se dividen en varios grupos según su prefijo.

1xx Informativo
--------------

| Código de la empresa. Significado.
|-------|--------|
| `100` | Continuar |
|101` | Protocolo de conmutación |

2xx Éxito
----------

| Código de la empresa. Significado.
|-------|--------|
| `200` | OK (todo bien) |
| Creado.
| Aceptado.
| Información no autorizada.
| Sin contenido.
| `205` | Restaurar el contenido |
| Contenido parcial.

Redirección 3xx
----------------

| Código de la empresa. Significado.
|-------|--------|
| 300` | Opción múltiple |
| 301` | Redirección permanente |
| `302` | Encontrado |
| Ver más
| No hay cambios.
| Utiliza el proxy.
| `306` | **Antiguo, pero reservado para uso futuro** |
| 307` | Redirección temporal |

4xx Error del cliente (usuario)
-----------------------------

| Código de la empresa. Significado.
|-------|--------|
| `400` | Mala solicitud |
| `401` | Conexión no autorizada |
| 402` | Pago solicitado |
| 403` | Desactivado |
| No se ha encontrado.
| `405` | Método no permitido |
| 406` | Inaceptable |
| Se requiere autenticación de proxy.
| La solicitud se ha agotado.
| 409` | Conflicto en la red |
| `410` | Datos desaparecidos |
| `411` | La longitud solicitada no coincide |
| `412` | Supuesto fallido |
| `413` | La solicitud de la entidad es demasiado grande |
| `414` | Request-URI is too long |
| `415` | Tipo de medio no soportado |
| `416` | Ámbito solicitado no satisfactorio |
| `417` | Expectativa fallida |

5xx Error del servidor
--------------

| Código de la empresa. Significado.
|-------|--------|
| `500` | Error interno del servidor |
| No se ha implementado.
| `502` | Puerta de enlace mala |
| Servicio no disponible.
| `504` | Tiempo de espera de la puerta de enlace expirado |
| `505` | Versión HTTP no soportada |
| `509` | Límite de ancho de banda excedido |
