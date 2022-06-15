Indentación del código mediante espacios y tabulaciones
=======================================================

> id: '116f19ed-3753-498d-bb9e-e0f93b88c347'
> slug:
> 	cs: mezery-a-tabulatory
> 	es: indentacion-del-codigo-mediante-espacios-y-tabulaciones
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1'
> sourceContentHash: c3af36d7df67cf36b940c9702cb71549

Para que el código sea fácil de leer para otros programadores y se mantenga elegante, tenemos que aprender a darle un formato uniforme. Este artículo trata sobre el uso de espacios y tabulaciones.

¿Son mejores los espacios o los tabuladores para sangrar el código? Este suele ser un tema de debate interminable, si buscas una respuesta rápida e inequívoca, la mayoría de los buenos programadores prefieren usar pestañas, pero vamos a desglosarlo bien.

Espacios
----------------------

Cada programador y editor utiliza una cantidad diferente de espacios para la sangría (pero la mayoría de las veces 4), lo que lleva a un código inconsistente que puede ser más difícil de leer cuando se lee el código de otra persona. Además, se necesitan más caracteres para la sangría (lo que aumenta el tamaño de los datos).

Sin embargo, los espacios tienen una ventaja a la hora de renderizar el código en un navegador web (donde la entidad HTML `&nbsp;` se utiliza para la sangría), por lo que es un formato relativamente fácil de transportar que sólo gana una ventaja como método de renderización estable y fiable (4 espacios siempre aparecerán como 4 espacios).

Tabuladores
----------------------

Son cualquier ancho que el programador establezca en el editor (si el editor puede hacerlo), así que si te gusta una sangría en particular, no hay problema - cada uno puede ver el mismo código con diferentes anchos de tabulación. Al mismo tiempo, es un personaje muy económico que no necesita repetirse tan a menudo como los espacios.

Cuando se renderiza el código con tabulador en una página HTML, es habitual sustituir los tabuladores por espacios fijos para garantizar la correcta visualización en todos los navegadores:

```php
$code = '<?php
    $a = 5+3;
    $b = 4;
    si ($a > $b) {
        echo $a . " > " . $b;
    } si no {
        echo $b . " <= " . $a;
    }
?>';

echo str_replace("\t", '&nbsp;', $code);
```
