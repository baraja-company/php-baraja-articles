Uso inseguro de constantes en PHP
=================================

> id: b4d0f45f-c059-4a47-9b63-8980d0d465ed
> slug:
> 	cs: nebezpecne-konstanty
> 	es: uso-inseguro-de-constantes-en-php
> 
> publicationDate: '2021-06-22 10:49:46'
> mainCategoryId: '561b0614-e152-4a62-a634-1f2d605d39d9'
> sourceContentHash: '58d4ea2e4bf106402386506b023ba4d2'

Hay dos cosas difíciles de tener en cuenta cuando se utilizan constantes en PHP.

Constantes dinámicas y estáticas
------------------------------

Una constante puede ser definida en PHP directamente en la clase (la mejor solución), por ejemplo de la siguiente manera:

```php
class Region
{
	public const PREFIX = 420;
}
```

Y el uso es bastante claro. En tiempo de compilación de la clase, se decide el valor de la constante y podemos acceder a ella llamando al nombre de la clase y a la propia constante. La mayoría de las veces escribiendo `Región::PREFIX`.

La otra forma (mucho peor) es definir la constante dinámicamente en tiempo de ejecución (casi siempre en algún lugar de un script de configuración), donde es entonces algo como:

```php
define('BASE_DIR', __DIR__ . '/../');
```

La mayor desventaja de definir una constante a través de la función `define` es que el script que define la constante puede no haber sido llamado, por lo que la constante no existirá cuando intentes leerla.

Cuando se combina con el uso de una constante dinámica dentro de la definición de una estática en una clase, esto puede incluso dar lugar a un error de reflexión fatal:

```php
class InvoiceGenerator
{
	// ¡Esto está completamente mal!
	public const DATA_DIR = BASE_DIR . '/datos/factura';
}
```

> **Explicación:**
>
> El uso de una constante dinámica dentro de una constante estática tiene la gran desventaja de que el valor de la constante dinámica no se puede leer en tiempo de compilación. Por lo tanto, esta secuencia de comandos debe ser procesada de nuevo en cada solicitud (es decir, no puede ser almacenada en el OPCache para la optimización de la velocidad), y si la constante no existe en absoluto, se lanza un error de compilación fatal y la aplicación no puede ejecutarse en absoluto.

Si utiliza PhpStan, puede advertirle automáticamente de este problema:

```txt
Reflection error: Could not locate constant "BASE_DIR" while
evaluating expression in InvoiceGenerator at line 6
```

> **Aprendizaje:**
>
> El valor de todas las constantes debe ser siempre constante.


Herencia de constantes cuando se utiliza la estática
-------------------------------------

En algunos casos tiene sentido utilizar la herencia para anular el valor de una constante. Pero en ese caso, el ancestro no puede leer el valor del descendiente (o no debería).

Un ejemplo es la definición de países y regiones:

```php
abstract class Region
{
	public function getPrefix(): int
	{
		// ¡Error fatal!
		return static::REGION;
	}
}

final class CzechRepublic extends Region
{
	public const REGION = 420;
}
```

Lo paradójico es que el código anterior no necesariamente arroja un error, pero sí puede ser arrojado por un uso inadecuado de la herencia.

Si llamamos al método `getPrefix()` en un descendiente de `CzechRepublic`, todo será correcto porque el valor de la constante se leerá correctamente. Sin embargo, si el descendiente no establece el valor de la constante, se lanzará un error fatal de constante inexistente. Lo peor de todo es que se trata de una **dependencia oculta** que se crea en la implementación del método, y el desarrollador que hereda la clase puede ni siquiera conocer esta dependencia.

La mejor solución en este caso es definir una constante directamente en el ancestro con un valor por defecto (para que la lógica siempre pase), o al menos lanzar una excepción en el getter.

```php
abstract class Region
{
	public const REGION = null;

	public function getPrefix(): int
	{
		if (static::REGION === null) {
			throw new \LogicException('No se ha definido la región.');
		}
		return static::REGION;
	}
}

final class CzechRepublic extends Region
{
	public const REGION = 420;
}
```

PhpStan responde a este error de la siguiente manera:

```txt
Access to undefined constant static(Region):REGION.
```
