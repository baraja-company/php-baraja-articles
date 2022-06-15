Validación y formato de los números de teléfono
===============================================

> id: bbfe35c3-3a8c-4fe5-a9bd-5bff0fc234e0
> slug:
> 	cs: telefonni-cisla
> 	es: validacion-y-formato-de-los-numeros-de-telefono
> 
> publicationDate: '2021-06-18 09:20:00'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: '224b3857a6bb898d9d322499cd1d3757'

No hay una manera fácil de validar y formatear los números de teléfono en PHP, así que escribí una biblioteca simple que no tiene dependencias, pero puede manejar esta función.

El objetivo es comprobar el formato de un número de teléfono, o convertirlo en una forma canónica básica (que siempre es válida).

Instalación de
---------

Simplemente por el compositor:

```txt
$ composer require baraja-core/phone-number
```

O descargue el paquete [Descargar en GitHub](https://github.com/baraja-core/phone-number).

Cómo utilizar la biblioteca
----------

El principio de esta herramienta se basa en formatear y validar los números de teléfono del usuario, o de fuentes sobre las que no se tiene control.

El uso más común es corregir el formato del número de teléfono:

```php
$original = '+420 777123456';
$formatted = \Baraja\PhoneNumber\PhoneNumberFormatter::fix($original);

echo $original . '<br>';
echo $formatted;
```

La función corrige el formato del número y devuelve la cadena `+420 777 123 456`.

Si el usuario no especifica ningún prefijo, se asume el prefijo `+420`. Puedes cambiar la preferencia por defecto con el segundo parámetro:

```php
// devuelve: +421 777 123 456
\Baraja\PhoneNumber\PhoneNumberFormatter::fix('+420 777123456', 421);
```

El código telefónico se sobrescribe sólo si el usuario no lo introduce y no se detecta automáticamente.

Formato de entrada y salida
----------------------------

La cadena de entrada puede tener (casi) cualquier aspecto. El algoritmo incorporado puede eliminar automáticamente los caracteres no válidos (por ejemplo, algunos usuarios escriben una nota junto a un número de teléfono que se eliminará automáticamente). Así que no tiene que preocuparse de formatear la entrada en absoluto, pero la salida siempre será coherente.

La salida siempre tiene el mismo aspecto (está normalizada al formato canónico).

El formato general es:

```txt
   +420 777 123 456
     |  \_________/
  Prefix     |
      National number
```

Si envía una entrada no válida (o una entrada que no puede ser corregida automáticamente), se lanzará una excepción.

Trampa de errores
----------------

Si un número no puede ser normalizado con seguridad a una forma base, o no existe, lanza una excepción `InvalidArgumentException`.

Si desea convertir la excepción en un booleano, utilice el validador de activos incorporado:

```php
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('123'); // falso
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('777123456'); // verdadero
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('+420 777123456'); // verdadero
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('+420 777 123 456'); // verdadero
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('+420 77 712 34 56'); // verdadero
```
