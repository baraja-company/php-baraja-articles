Trabajar con archivos
=====================

> id: '12330cfb-5729-419a-b2e4-cb3d1db998cc'
> slug:
> 	cs: prace-se-soubory
> 	es: trabajar-con-archivos
> 
> publicationDate: '2020-02-16 16:30:05'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '0f5b9e84a974285dcb63baafc912be5f'

Hay una serie de funciones en PHP para trabajar con archivos.

- <a href="/fopen">fopen()</a>, acceso de bajo nivel a los archivos del disco
- <a href="/file-get-contents">file_get_contents()</a>, recuperar el contenido de un archivo o URL
- <a href="/file-put-contents">file_put_contents()</a>, guardando una cadena en un archivo local.

Funciones del disco
--------------

- `unlink($ruta)`, borrar archivo
- `copy($from, $to)`, copia un archivo de una ubicación a otra (puede ser de una URL al disco local)
- `rename()`, renombrar o mover un archivo en el disco
- `chmod()`, cambiar los permisos de lectura, escritura o ejecución de un archivo
