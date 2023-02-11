Reparación urgente de un servidor sobrecargado
==============================================

> id: '07c5d1ed-f854-4797-8efe-350a24594762'
> slug:
> 	cs: senior-6-pretizeny-server
> 	es: reparacion-urgente-de-un-servidor-sobrecargado
> 
> publicationDate: '2023-02-11 14:25:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: f8239dd6b0eee234efc999c0dd3e0aeb

Una herramienta de monitorización externa le informará de que el tiempo medio de respuesta de las 5 URL monitorizadas se ha duplicado en los últimos 30 minutos. El proyecto se ejecuta en un único servidor físico que no está bajo su gestión y se ejecuta en algún lugar de un centro de datos. Te conectas por SSH, inicias htop y ves que la carga de la CPU es del 95% y la memoria está desbordada desde hace tiempo.

Según git, sabes que hace una semana hicieron una migración de base de datos a una nueva estructura de tablas, y un compañero escribe en el chat que tuvo que ejecutar la migración durante la noche porque el recálculo de columnas e índices tardó unas 5 horas, durante las cuales casi toda la base de datos estaba bloqueada, y no funcionaban ni INSERT ni SELECT.

Así que los problemas de rendimiento probablemente se deban a índices mal diseñados, consultas SQL mal rediseñadas o grandes agrupaciones de conexiones. No hay tiempo para una reversión, hay 7 mil usuarios en el sitio según Google Analytics, y un corte de 5 horas significaría un riesgo reputacional para el cliente, y una pérdida de decenas a cientos de miles de coronas durante ese tiempo (es difícil de estimar, los proyeccionistas se inventan bastante). Se da cuenta de que probar sólo la funcionalidad en un entorno de prueba no es suficiente, y necesita implementar también una prueba de carga.

Como se trata de una importante tienda de comercio electrónico de su mayor cliente, y prevé que la situación puede empeorar, dispone de 30 segundos para tomar una decisión.

¿Cómo proceder?

1. No haces nada. El sitio será más lento, pero mientras el servidor pueda soportarlo, supongo que está bien...
2. Intentará preparar un plan para revertir la estructura de la BD y ejecutarlo lo antes posible.
3. Migras el sitio a un servidor más potente (pero esto significa subir el precio al cliente y esperar unas 6 horas a que se complete la migración, porque tienes cientos de GB de tablas de base de datos).
4. Averigüe qué consultas SQL y qué páginas consumen más tiempo, y simplemente desactívelas.
5. Ejecutas Cloudflare y dejas que compruebe estáticamente lo que pueda.
6. Alguna otra magia (no creo que haya mucho que inventar)...
