Envío de un archivo CSV
=======================

> id: '35c4f30c-8638-496b-82a7-1297a598fbdb'
> slug:
> 	cs: csv-response
> 	es: envio-de-un-archivo-csv
> 
> publicationDate: '2022-12-18 11:10:00'
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1
> sourceContentHash: '582e41a6e41b568d09be08c482523df8'

Al enviar archivos binarios, piense siempre qué cabeceras HTTP elegir. En caso de enviar un archivo CSV (formato casi ideal para tablas de texto sencillas, que pueden ser procesadas por Excel), es útil `Content-Type: application/csv`, en codificación `UTF-8`.

Sin embargo, en algunas versiones de Excel existe un problema con la codificación UTF-8. Para asegurarnos de que se detecta la codificación correcta, necesitamos insertar el `UTF-8 BOM`, que es un carácter especial ``xEF\xBB\xBF`` que indica al cliente que se trata de UTF-8, ya que no existe en ninguna otra codificación.

Por lo tanto, envíe las cabeceras de la siguiente manera:

```php
header('Content-Type: application/csv; charset=utf-8');
header('Content-Disposition: attachment; filename=' . date('d-m-y') . 'archivo.csv');
header('Pragma: no-cache');
echo "\xEF\xBB\xBF";
```
