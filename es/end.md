Fin()
=====

> id: '879c7c9a-a497-4080-865d-f15e6c9e8578'
> slug:
> 	cs: end
> 	es: fin
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '2d097dd1fae4e4f125d879c92bc2cfbe'

- Soporta PHP 4, PHP 5
- Descripción breve: establece el puntero interno de un array a su último elemento.
- Requisitos.

Descripción
--------------------------

La función `end()` establece un puntero interno del array al último elemento y devuelve su valor.

Funciones similares
--------------------------

- `actual()`
- `each()`
- `prev()`
- <a href="/reset">reset()</a>
- `siguiente()`

Ejemplo
--------------------------

```php
echo end([
    'manzana',
    'plátano',
    'arándanos',
]); // escribe el arándano
```



```php
$a = [];
$a[1] = 1;
$a[0] = 0;

echo end($a); // imprime 0
```

Parámetros
--------------------------

| # | Tipo | Descripción |
| --- | ------- | ----- |
| 1. El nombre de la matriz con la que se va a trabajar.

Valores de retorno
--------------------------

Devuelve el valor del último elemento o `false` para un array vacío.

Diferencias con las versiones anteriores
--------------------------

Ninguno
