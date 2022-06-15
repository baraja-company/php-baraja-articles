Htmlspecialchars
================

> id: '46f04729-3956-4889-bb40-58362cb46b2a'
> slug:
> 	cs: htmlspecialchars
> 	es: htmlspecialchars
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0743dd02-12d0-4766-ba42-8fd7e9c4ae8a'
> sourceContentHash: '566e10c57527a45f32906beb6c21430f'

Htmlspecialchars() es una función para convertir caracteres especiales en entidades HTML.

Descripción
-----

```php
$promenna = htmlspecialchars($text);
```

Algunos caracteres especiales tienen un significado especial para los navegadores, por lo que deben convertirse en entidades. Esto previene la seguridad general de los scripts y evita que la página se renderice incorrectamente.

Se utiliza sobre todo para proteger los formularios y cualquier lugar en el que el usuario inserte texto y se arriesgue a insertar etiquetas HTML.

| Carácter | Nota | Cambios en
|------|-------------------------|-----------
| `&` | ampersand | `&amp;`
| `"` | comillas dobles (cambia cuando se desactiva `ENT_NOQUOTES`) | `&quot;`
| `'` | apóstrofe (cambia cuando se activa `ENT_QUOTES`) | `&#039;`
| `<` | menos que, corchete HTML | `&lt;`
| `>` | mayor que, corchete HTML | `&gt;`

Parámetro
--------

**Cadena** a convertir

**banderas** Diferentes configuraciones de comportamiento

**charset** Especifica el conjunto de caracteres (codificación). El juego de caracteres por defecto es `ISO-8859-1`.

Puede utilizar `ISO-8859-1`, `ISO-8859-15`, `UTF-8`, `cp866`, `CP1251`, `CP1252` y `KOI8-R`.

> Nota: Soporte sólo a partir de PHP 4.3.0 y posterior. Cualquier otro conjunto de caracteres no es reconocido ni soportado.

**Double_encode** Cuando `double_encode` está deshabilitado, PHP no codificará las entidades HTML existentes, el valor por defecto es convertir todo.

Valores de retorno
-----------------

Convierte la cadena.

Si la cadena contiene unidades no válidas, dentro del conjunto de caracteres dado en `ENT_IGNORE` (no establecido), se devuelve una cadena vacía.

Cambios en las versiones
----------------

| Versión de la nota
|-------|---------
| 5.4.0 Adición de las constantes `ENT_SUBSTITUTE`, `ENT_DISALLOWED`, `ENT_HTML401`, `ENT_XML1`, `ENT_XHTML` y `ENT_HTML5`.
| 5.3.0. Adición de la constante `ENT_IGNORE`.
| 5.2.3. Adición del parámetro "double_encode".
| 4.1.0 | Añadir el parámetro `charset`.

Ejemplo
-------

```php
$new = htmlspecialchars(
	'<a href="prueba">Prueba</a>',
	ENT_QUOTES
);

echo $new; // <a href="test">Test</a>
```
