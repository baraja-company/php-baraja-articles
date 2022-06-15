Obtener una lista de todos los archivos cargados
================================================

> id: '72716cbc-90a6-4870-848b-125e8430707f'
> slug:
> 	cs: ziskani-seznamu-vsech-nactenych-souboru
> 	es: obtener-una-lista-de-todos-los-archivos-cargados
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: f611e5d3-ed7b-4fe9-84ca-9271fc2bd2e3
> sourceContentHash: '7e896aa0e501d0fe33dc6595c1bfb43e'

Al depurar aplicaciones más complejas, a veces ocurre que no sé qué archivos se han cargado y si falta algo.

Si utilizas Composer o cualquier otro tipo de <a href="/autoloading-trid">autolectura de clases</a>, probablemente no conozcas este problema. Sin embargo, puede ser algo relativamente común cuando se depuran aplicaciones antiguas de otros desarrolladores.

La obtención de todos los archivos cargados puede hacerse con la función `get_included_files()`, que los devuelve como una matriz de cadenas de ruta absoluta.

Es común en el desarrollo tener un gran número de archivos cargados (por ejemplo, incluso este blog relativamente simple utiliza casi 160 archivos). La mayoría de las veces, sin embargo, el gran volumen no importa, porque el contenido de los archivos se recupera de OPCache, que está optimizado para lecturas masivas.
