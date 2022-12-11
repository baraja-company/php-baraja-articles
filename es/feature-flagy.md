Indicadores/interruptores de activación/desactivación de funciones
==================================================================

> id: '4209c32f-655c-46fa-9090-e5ec3883ea61'
> slug:
> 	cs: feature-flagy
> 	es: indicadores-interruptores-de-activacion-desactivacion-de-funciones
> 
> publicationDate: '2022-12-11 15:00:00'
> mainCategoryId: '28a2aef7-7490-43a9-add7-80d8a051f8a9'
> sourceContentHash: '1e32c312eab2ca16539cdb7431e2dda9'

Cuando desarrolle una aplicación más compleja, apreciará la posibilidad de desarrollar más funciones por adelantado, distribuirlas con la siguiente versión de su software y activar la función más adelante.

Precisamente para eso se crearon las banderas de características. Este artículo le mostrará cómo utilizarlos.

Aplicación básica
---------------------

Las banderas de características son básicamente un concepto muy simple de llamar a una única función/método que decide si una nueva característica está activa.

Por ejemplo:

```php
echo '<h1>Aplicaciones meteorológicas</h1>';
echo 'Hoy sí:' . getWeather();

if (feature('mapa')) {
   echo 'Mapa:' . getMap();
}
```

Para comprobar la disponibilidad de una noticia concreta, se llama a la función `feature()`, que decide si puede permitir o ignorar la característica concreta basándose en el nombre de la llamada.

Aplicación de la lógica de decisión
-------------------------------

La lógica de la decisión suele ser compleja. Por ejemplo, sólo puede ejecutar una función específica a partir de una fecha concreta, o para usuarios de un grupo específico. Por ejemplo, a menudo pruebo de este modo la implantación de una nueva función en, digamos, el 5% de los usuarios, para que no afecte a todos a la vez.

Por ejemplo, cuando desarrollamos sotfware corporativo, así realizamos campañas publicitarias y descuentos válidos a partir de una fecha determinada.

Si una nueva función se rompe, es posible simplemente desactivarla con una bandera de función para los usuarios, y activarla para un grupo de desarrolladores que la probarán y aportarán una solución, por ejemplo.
