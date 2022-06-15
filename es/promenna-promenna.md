Variables Variables
===================

> id: a0baeb3a-ab6e-4dd9-b1bc-b1872a12ee08
> slug:
> 	cs: promenna-promenna
> 	es: variables-variables
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '590515cc711ec27cc0f0bfe190c8fbff'

> **Atención:** Este artículo fue escrito hace muchos años y parte de la información puede ser obsoleta o incorrecta. Téngalo en cuenta al leerlo.

**Las variables** no están pensadas para el despliegue común (resuelven problemas que pueden ser resueltos de otras maneras), se utilizan principalmente para hacer las escrituras más concisas y los accesos a la memoria más complejos.

Considere el siguiente ejemplo:

```php
$x = 25;                  // contiene 25
$nacitana_promenna = 'x'; // contiene "x"
$y = $$nacitana_promenna; // contiene 25
echo $y;                  // imprime 25
```

Fíjate en los dos dólares que se suceden. En este caso, el valor de la variable $y se cargará en la variable que tiene el nombre contenido en la variable $nacitana.

Un poco confuso, ¿no? Por eso es mejor no usar variables.
> **Nota:** Las variables variables son una especialidad de PHP por el signo de dólar. En otros lenguajes, el inicio del nombre de la variable no está marcado con ningún carácter, por lo que no se puede utilizar variables de tipo variable porque sería ambiguo cuando es una variable clásica y cuando no lo es.
