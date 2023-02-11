Cómo solucionar los bloqueos repentinos de los scripts PHP
==========================================================

> id: '41edbf1b-3ca5-487f-a54c-fac9926bc3d2'
> slug:
> 	cs: senior-7-sessions
> 	es: como-solucionar-los-bloqueos-repentinos-de-los-scripts-php
> 
> publicationDate: '2023-02-11 14:30:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: bdd1e207ee9c0ba19c39f0b5c7d83c43

Una anécdota de finales de 2016, cuando me salvó literalmente un colega: en una aplicación PHP, decides facturar imágenes a través de un script proxy que, entre otras cosas, puede ajustar sus dimensiones y otros parámetros en función de la petición entrante. Como parte de la optimización, también se guardan físicamente en disco las variantes generadas.

Sin embargo, en la operación de producción, de repente empiezas a ver una carga enorme y miles de peticiones en cola. Las imágenes se cargan secuencialmente una a una para cada usuario. Las actualizaciones de página y los clics en enlaces no funcionan. La aplicación parece completamente congelada. Sólo funciona esperar a que todo se procese.

¿Cuál puede ser el problema? He enumerado 3 pistas principales en el texto para permitir una búsqueda rápida del problema. Hotfix tiene una solución trivial.
