Variables globales en PHP
=========================

> id: '1cdfc51a-31f4-4a5d-81f6-cfd92f86a9d4'
> slug:
> 	cs: globalni-promenna
> 	es: variables-globales-en-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '31e78a9abf19252ef62315230a3731b5'

Las variables globales están disponibles en cualquier momento en cualquier parte de la aplicación y no necesitan ser pasadas.

> **Atención:** Una aplicación bien diseñada no debería usar variables globales porque violan el principio de encapsulación y pueden causar errores difíciles de detectar si se manejan sin cuidado.

Ejemplo de uso:

```php
$a = 1;
$b = 2;

function suma(): void
{
	global $a, $b;
	$b = $a + $b;
}

suma();

echo $b; // imprime el número 3 porque la variable $b es global
```

Nótese que hemos obtenido las variables `$a` y `$b` fuera de su contexto natural. Este comportamiento se denomina "mágico" porque si otra función anula las variables actualmente en uso, la aplicación experimentará una condición inesperada.

De forma correcta, la aplicación debería **encapsular** y pasar las variables cada vez:

```php
$a = 1;
$b = 2;

function suma(int $a, int $b): int
{
	return $a + $b;
}

echo suma($a, $b); // imprime 3
```

Gracias a esto, podemos llamar a la función dinámicamente con diferentes parámetros de entrada y su salida dependerá sólo de las entradas, no del entorno.

Obtención de parámetros de entrada desde la URL
---------------------------------

Tal vez el único uso razonable de las variables globales sea en el análisis de la entrada del usuario, en cuyo caso estamos hablando de <a href="/superglobal-variable">variables superglobales</a>.

En este caso, es un diseño limpio porque la variable debe ser de sólo lectura, no de sólo escritura, y además es la misma en toda la aplicación:

```php
function getNameFromUrl(): string
{
    return isset($_GET['nombre'])
    	? htmlspecialchars($_GET['nombre'])
    	: '';
}

echo getNameFromUrl();
```
