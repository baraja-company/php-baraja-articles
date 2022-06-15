Incluir (doblar las páginas de las piezas)
==========================================

> id: '4984832e-c11f-4e9e-8d3b-60561685389d'
> slug:
> 	cs: include-soubor
> 	es: incluir-doblar-las-paginas-de-las-piezas
> 
> publicationDate: '2019-08-23 15:06:33'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: aea5867cf788e623dddd3ced4ea24a7a

PHP es originalmente un lenguaje de plantillas, que fue creado para facilitar la unión de piezas de páginas.

Formatos admitidos
-------------------

El plegado funciona como texto, por lo que es aconsejable utilizar formatos relevantes como `.html` o `.md`.

Cuando se pega un archivo PHP, su contenido se ejecuta como si existiera físicamente en la ubicación pegada.

Plegado de páginas e inserción de contenidos comunes
---------------------------------------------

A menudo tenemos que crear varias páginas que tienen un contenido común, por ejemplo, un menú.

En HTML plano, primero crearíamos una página con un menú y luego la copiaríamos muchas veces. Pero en PHP podemos automatizar todo el proceso.

Tenemos un archivo `menu.html` donde está el contenido del menú y `index.php` donde ponemos el contenido y el menú.

Un ejemplo sencillo:

```php
<div class="página">
    <div class="contenido">
        <?php
            include __DIR__
            . '/artículo/' . ($_GET['página'] ?? 'Índice') . '.html';
        ?>
    </div>
    <div class="menu">
                    include 'menú.html';
        ?>
    </div>
</div>
```

Este script inserta automáticamente el contenido de la página desde el directorio `/article` y lee el nombre del archivo según la entrada del usuario (parámetro URL `?page=...`). Si no se pasa ningún parámetro, se utiliza `index.html`.

Así, la URL podría ser, por ejemplo, `ejemplo.com?page=contacts` y cargar `/artículo/contacts.html`.
