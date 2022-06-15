Palabras clave de PHP
=====================

> id: d58620d7-57a0-410a-ba73-31a1f9d984fb
> slug:
> 	cs: keywordy-jazyka-php
> 	es: palabras-clave-de-php
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '38fbdb53b4455903f3300c3c8b4a8ad2'

Casi todos los lenguajes de programación constan de "palabras clave", que son expresiones especiales del lenguaje que tienen un significado especial.

Un ejemplo de palabra clave es la palabra `cadena`, `si` o `eco`.

Es importante tener en cuenta que las palabras clave (a veces también `comandos`) <a href="/comandos-y-funciones">no son funciones</a>, por lo que `echo`, por ejemplo, tampoco es una función.

Es bueno conocer la lista de palabras clave porque tienen significados especiales en PHP y no siempre pueden usarse para todo. Por ejemplo, el nombre de una clase no debe ser el mismo que una de las palabras clave existentes.

Ejemplo de error de análisis
-------------------

Cuando se intenta procesar una clase llamada `String`, se produce un `Error de comprensión` porque PHP no sabe si es un nombre de clase o un tipo de datos:

Esto es un error:

```php
class String
{
   // ...
}
```

Lista de palabras clave
-------------------

Lista de palabras clave actualizada para PHP 7.1:

`and`, `or`, `xor`, `for`, `do`, `while`, `foreach`, `as`, `return`, `die`, `exit`, `if`, `then`, `elsese`, `elseif`, `new`, `delete`, `try`, `throw`, `catch`, `finally`, `class`, `function`, `string`, `array`, `object`, `resource`, `var`, `bool`, `boolean`, `int`, `integer`, `float`, `double`, `real`, `string`, `array`, `global`, `const`, `static`, `public`, `private`, `protected`, `published`, `extends`, `switch`, `true`, `false`, `null`, `void`, `this`, `self`, `struct`, `char`, `signed`, `unsigned`, `short`, `long`
