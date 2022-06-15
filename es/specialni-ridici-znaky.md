Caracteres de control especiales en PHP
=======================================

> id: '888e6ee4-2ff3-4f0d-8f5a-4741e62e11d0'
> slug:
> 	cs: specialni-ridici-znaky
> 	es: caracteres-de-control-especiales-en-php
> 
> publicationDate: '2021-11-24 14:05:00'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '2711968227a253040a0a6ed1464aa447'

Las cadenas de PHP pueden contener caracteres de control especiales que tienen diferentes significados en un contexto particular y no necesariamente se comportan como caracteres regulares.

Muchos de ellos ya le resultarán intuitivamente familiares. Algunos se reservan para usos especiales y otros se reservan para los caracteres del teclado, por ejemplo.

Escribir caracteres especiales
-----------------------

Los caracteres especiales se escriben entre comillas dobles.

Así que es muy sencillo:

```php
$message = "Hello\nworld.";
```

El código anterior contiene un salto de línea entre "Hola" y "Mundo".

Tabla de caracteres especiales
-------------------------

Si la cadena está encerrada entre comillas dobles ("), PHP interpretará las siguientes secuencias de escape como caracteres especiales:

| Secuencia | Significado |
|----------|--------|
| `\n` | salto de línea (`LF` o `0x0A (10)` en ASCII) |
| `\r` | retorno de carro (`CR` o `0x0D (13)` en ASCII) |
| ``t` | tabulación horizontal (`HT` o `0x09 (9)` en ASCII) |
| ``v` | tabulador vertical (`VT` o `0x0B (11)` en ASCII) |
| `\e` | escape (`ESC` o `0x1B (27)` en ASCII) |
| `\f` | form feed (`FF` o `0x0C (12)` en ASCII) |
||| barra invertida ||
| signo de dólar |
| `"` | comillas dobles |
| `[0-7]{1,3}` | Una secuencia de caracteres que coincide con una expresión regular es un carácter en notación octal que se desborda silenciosamente en un byte (por ejemplo, `"\400" === "\000"`) |
| `\x[0-9A-Fa-f]{1,2}` | La secuencia de caracteres correspondiente a una expresión regular es un carácter en notación hexadecimal. |
| `\u{[0-9A-Fa-f]+}` | la secuencia de caracteres que coincida con la expresión regular es un punto de código Unicode, que se emitirá en la cadena como una representación UTF-8 de ese punto de código.

Al igual que con las cadenas entre comillas simples, se emitirá una barra invertida cuando se escape cualquier otro carácter.

Al delimitar las cadenas con comillas, tenga en cuenta que las variables contenidas se expandirán (los valores de las variables se escribirán directamente en la cadena). Este comportamiento puede ser extremadamente peligroso.
