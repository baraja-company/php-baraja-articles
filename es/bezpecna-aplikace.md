Aplicación segura
=================

> id: cc4667a3-aea3-4e0a-b323-da31ab78e019
> slug:
> 	cs: bezpecna-aplikace
> 	es: aplicacion-segura
> 
> publicationDate: '2019-10-01 14:19:04'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '74e52382b995b303550d8e4b783bbb84'

Si usted se toma en serio el desarrollo de aplicaciones web y el sitio estará disponible posteriormente en Internet, es muy importante abordar la seguridad.

Siendo realistas, a los desarrolladores les esperan las siguientes amenazas:

- **La aplicación tiene un error interno**, por ejemplo, porque el programador cometió un error del que no se dio cuenta al escribir el código, o sólo se manifiesta "a veces"
- **El servidor tiene una configuración errónea** o el entorno **ha cambiado** porque el administrador del servidor cambió el comportamiento del mismo y los sitios no estaban adaptados para ello. Por otro lado, estamos desplegando en una nueva máquina y no sabemos la configuración exacta.
- Alguien está intentando atacar**, ya sea un atacante externo o un antiguo empleado desde dentro del sistema.

Todas las situaciones son extremadamente desagradables y requieren una acción inmediata. Diseñar una aplicación para que nunca falle no es técnicamente posible.

Durante el desarrollo, es muy importante probar el código una vez que se ha escrito, y si se produce un error en el servidor, es importante informar al desarrollador inmediatamente con una descripción precisa del problema.

Para detectar bugs y monitorear la salud del servidor, recomiendo la herramienta <a href="https://tracy.nette.org/">Tracy</a>, que registra todos los errores fatales, excepciones no manejadas y más en archivos directamente en el disco del servidor. Además, puedes configurar un correo electrónico donde se enviarán las notificaciones.

¿Está interesado en una consulta de seguridad para su aplicación?

Contacta conmigo.
