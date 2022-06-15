Tipo de datos objeto Enum en PHP
================================

> id: '5b96765c-16c2-4f76-8e31-6de6e9f59365'
> slug:
> 	cs: datovy-typ-enum
> 	es: tipo-de-datos-objeto-enum-en-php
> 
> publicationDate: '2022-05-29 15:30:00'
> mainCategoryId: e45491db-b548-471d-97b4-3e23610c5da9
> sourceContentHash: '43735e926db87d2cc6c538ddb67e0a69'

A partir de PHP 8.1, el tipo de datos Enum puede ser usado para definir valores de enumeración exactos para una lista. Esto es útil para los casos en los que sabemos que el valor de una variable sólo puede tomar unos pocos valores específicos.

Por ejemplo, así es como almaceno los tipos de notificación:

```php
enum OrderNotificationType: string
{
    case Email = 'correo electrónico';
    case Sms = 'texto';
}
```

En PHP, el tipo de datos Enum es un objeto clásico que se comporta como un tipo especial de constante, pero también tiene una instancia que puede ser pasada. Sin embargo, a diferencia de un objeto normal, está sujeto a una serie de restricciones.

Diferencias entre Enum y objetos
-----------------------

Aunque los enums están construidos sobre clases y objetos, no soportan toda la funcionalidad relacionada con los objetos. En particular, se prohíbe que los objetos enum tengan estado interno (deben ser siempre una clase estática).

Una lista específica de diferencias:

- Los constructores y destructores están prohibidos.
- No se admite la herencia. Los Enums no pueden ser extendidos o heredados por otra clase.
- No se permiten las propiedades estáticas ni las de los objetos.
- No se admite la clonación de valores específicos de Enum (instancias), cada instancia individual debe ser una instancia única.
- Están prohibidos los métodos mágicos, excepto los indicados a continuación.

Las siguientes características del objeto están disponibles y se comportan como cualquier otro objeto:

- Métodos públicos, privados y protegidos.
- Métodos estáticos públicos, privados y protegidos.
- Constantes públicas, privadas y protegidas.
- Los Enums pueden implementar cualquier número de interfaces.
- Los atributos se pueden adjuntar a los enums y a los casos. El filtro de destino `TARGET_CLASS` incluye los propios Enums. El filtro de destino `TARGET_CLASS_CONST` incluye casos Enum.
- Métodos mágicos `__call`, `__callStatic` y `__invoke`.
- Las constantes `__CLASS__` y `__FUNCTION__` se comportan como constantes normales
- La constante mágica `::class` en el tipo Enum se evalúa como el nombre completo del tipo de datos, incluyendo cualquier espacio de nombres, exactamente como para un objeto. La constante mágica `::class` en una instancia de tipo `Case` también se evalúa como tipo Enum, ya que es una instancia de un tipo diferente.

Uso de Enum como tipo de datos
-----------------------------

Imagina que tenemos un enum que representa los tipos de trajes. En este caso, sólo tenemos que definir el tipo `Suit` y almacenar los valores válidos individuales.

Entonces obtenemos una instancia de la opción particular clásicamente a través de una cuadrática, como cuando se trabaja con una constante estática.

Ejemplo de definir un Enum, invocarlo por un tipo específico y pasarlo a una función:

```php
enum Suit
{
	case Hearts;
	case Diamonds;
	case Clubs;
	case Spades;
}

function doStuff(Suit $s)
{
	// ...
}

doStuff(Suit::Spades);
```

Comparación de dos valores
---------------------

La ventaja fundamental de los enums sobre los objetos y las constantes es que es fácil comparar sus valores.

La comparación básica con la que trabajamos con un valor específico se puede hacer de la siguiente manera:

```php
$a = Suit::Spades;
$b = Suit::Spades;

$a === $b; // verdadero
```

Muy a menudo también tenemos que decidir que un valor concreto pertenece a una enumeración de valores Enum válida. Esto se puede comprobar fácilmente de la siguiente manera:

```php
$a = Suit::Spades;

$a instanceof Suit;  // verdadero
```

Lectura del valor del tipo
---------------------

Podemos leer un valor de tipo específico como nombre de una constante de llamada o directamente como un valor real definido (si existe):

```php
enum Colors
{
	case Red;
	case Blue;
	case Green;

	public function getColor(): string
	{
	    return $this->name;
	}
}

function paintColor(Colors $colors): void
{
	echo "Pintura:" . $colors->getColor();
}
```

El valor de la constante de llamada se lee a través de la propiedad `nombre`. También es importante que se pueda implementar una función personalizada directamente en el tipo de datos Enum, que pueda ser llamada sobre cada Enum.

Si un Enum en particular también implementa valores reales (que están ocultos bajo cada constante), su valor también puede ser leído:

```php
enum OrderNotificationType: string
{
    case Email = 'correo electrónico';
    case Sms = 'texto';
}

$type = OrderNotificationType::Email;

echo $type->value;
```

Todos los valores Enum válidos
-----------------------------

A menudo necesitamos listar (por ejemplo, al usuario en un mensaje de error) todos los posibles valores que puede tomar el Enum. Cuando se usaban constantes esto no era posible, Enum lo permite fácilmente:

```php
Suit::cases();
```

Devuelve `[Traje::Corazones, Traje::Diamantes, Traje::Tréboles, Traje::Picas]`.

Compruebe que la variable es de tipo Enum
---------------------------------

Podemos verificar fácilmente que una determinada variable desconocida contiene un Enum mediante una condición:

```php
if ($haystack instanceof \BackedEnum) {
```

Cada objeto Enum es automáticamente un hijo de la interfaz genérica `\BackedEnum`.

Para más información, consulte la discusión en el GitHub de PhpStan: https://github.com/phpstan/phpstan/issues/7304
