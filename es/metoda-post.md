Recepción de datos por el método POST
=====================================

> id: c2f273fa-7730-4521-82d0-1b3096269fac
> slug:
> 	cs: metoda-post
> 	es: recepcion-de-datos-por-el-metodo-post
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '51a30cb759cdbb184ee5bb0bf3070af1'

El envío de datos por POST es bastante diferente de <a href="/method-get">GET</a>, es más seguro, el texto puede ser más largo y su valor no puede determinarse salvo por un formulario o una cabecera (que no obtendrá por error).

Fuente
--------------------------

Source no es tan diferente del método <a href="/method-get">GET</a>. Es más o menos lo mismo, excepto que los parámetros no se muestran en la URL, sólo el nombre del archivo es visible.

```php
echo $_POST['Artículo'] ?? '';
```

Características, ventajas e inconvenientes
--------------------------

- Los parámetros no se pueden vincular, pero hay que presentar un formulario
- No se puede indexar (relacionado con el punto anterior)
- Más seguro que <a href="/method-get">GET</a> (los datos se envían de forma oculta, los valores no se muestran en el historial)
- No se debe confundir con SSL, el POST no está cifrado
