Funciones puras en PHP
======================

> id: d94c18a9-8bc4-4377-8042-e7c8b48320a2
> slug:
> 	cs: pure-funkce
> 	es: funciones-puras-en-php
> 
> publicationDate: '2021-10-27 10:30:00'
> mainCategoryId: f611e5d3-ed7b-4fe9-84ca-9271fc2bd2e3
> sourceContentHash: e47a3507ddb189a218ad8e3a38703e5c

En programación funcional, existe el concepto de **función pura**, que se refiere a una función que siempre devuelve la misma salida a la misma entrada (es decir, es determinista), y al mismo tiempo no sufre ningún efecto secundario (es decir, no afecta a su entorno).

Cómo es una función pura
----------------------

Ejemplo de función pura:

```php
// Esta es una función pura
function add(int $a, int $b): int
{
	return $a + $b;
}
```

Se trata de una función pura porque la salida es siempre la misma en función de los argumentos de entrada.

Lo que no es una función pura
-------------------

```php
// Esta es una función impura
function add(int $a, int $b): int
{
	echo 'Añadiendo...';
	file_put_contents('archivo.txt', 'Valor:' . $a);
	return $a + $b;
}
```

Este tipo de función no es pura porque la función cambia el sistema de archivos. Otro tipo de función impura es cuando interactúa con la base de datos, imprime en la pantalla, etc.
