El principio de la encapsulación en el EPI
==========================================

> id: '54968a42-b678-4385-91ac-c13ba96c9b34'
> slug:
> 	cs: zapouzdreni
> 	es: el-principio-de-la-encapsulacion-en-el-epi
> 
> publicationDate: '2020-02-16 21:21:35'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '3fbce6cdcbf5f274312496604569d9c2'

Uno de los principios fundamentales de la POO es el **principio de encapsulación**, que dice que los problemas complejos deben descomponerse en muchos problemas pequeños que podamos resolver de forma independiente y simultánea. Al mismo tiempo, a nosotros como usuarios no nos importa cómo sucede y los datos (estado interno) permanecen aislados.

Por ejemplo, si estamos resolviendo el problema de cómo devolver el resultado `1,6` en base a una consulta del usuario con la expresión `(5+3)*(2/(7+3))`, probablemente ninguno de nosotros pueda escribir una sola función o método que resuelva este problema de una vez.

**TIP:** Una solución preparada para este tipo de ejemplo está en el artículo <a href="/pokrocila-kalkulacka">Procesamiento de una expresión matemática como cadena</a>, pero prepárate para que no sea fácil.

La encapsulación aporta abstracción sobre los objetos
-----------------------------------------

Con la encapsulación, podrás utilizar los objetos "como un usuario", es decir, llamar a sus métodos y no preocuparte en absoluto de cómo funcionan internamente.

Supongamos que se trata de calcular el salario de un empleado y queremos utilizar una clase existente de otro programador para hacerlo. Sólo necesitamos conocer los parámetros obligatorios del constructor y podemos "simplemente usar" la clase:

```php
$mzda = new MzdaZamestnance(
    25000, // salario bruto
    6,     // número de años en la empresa
    10,    // número de años de experiencia
    true   // ¿es un hombre?
);

echo $mzda->getHruba(); // 25000

echo $mzda->getCista(); // 17800
```

Los parámetros del objeto son ficticios y no se corresponden con la realidad del cálculo del salario. En particular, el principio se ilustra por el hecho de que **sólo necesitamos conocer la interfaz pública general** y ni siquiera tenemos que ocuparnos del **estado interno del objeto**, ni siquiera de la **implementación interna**, y desde luego no de **por qué funciona como lo hace**. Simplemente llamamos al método `getCista()` y obtenemos el pago neto.

La encapsulación es una cuestión de diseño
----------------------------

Es importante señalar que **la encapsulación en sí misma no es una característica o sintaxis del lenguaje**. El hecho de que una clase y una aplicación estén encapsuladas es sólo cuestión de que el programador diseñe la aplicación y piense en el código.

Piensa siempre en el diseño de la clase de esta manera:

- KISS (keep it simple), mantén una interfaz sencilla y no obligues al usuario a pensar innecesariamente. Resuelve la lógica compleja para el usuario y éste lo agradecerá.
- El usuario de la clase (otro programador o tú del futuro) no necesita conocer la lógica interna en absoluto y sólo los nombres de los métodos y sus parámetros deberían ser suficientes.
- Si necesito cálculos auxiliares para el cálculo que no son de interés para el usuario y son sólo técnicos, no tiene sentido crear un getter para ellos en absoluto y sólo deben ser calculados internamente.
- La clase debe satisfacer las propiedades básicas del algoritmo, en particular que funcione en general para cualquier dato.
- Los métodos disponibles públicamente deben estar diseñados para proporcionar suficiente información para ampliar fácilmente el objeto con nuevas características en el futuro, de modo que podamos calcular fácilmente nuevos datos a partir de lo que ya conocemos.

Mantener los datos internos no públicos
-------------------------------

Para las propiedades y métodos que manejan la lógica interna, tiene sentido establecer la visibilidad como `privada`. La principal ventaja de esto es que no serán llamados desde fuera y el usuario se verá obligado a utilizar su interfaz diseñada, protegiendo así los datos y el estado interno del objeto.

Por ejemplo, tengamos un objeto que represente una cuenta bancaria en la que queremos contabilizar los pagos y tratar el saldo actual:

```php
class BankAccount
{
    private int $sum;

    public function __construct(int $startSum)
    {
        $this->sum = $startSum >= 0 ? $startSum : 0;
    }

    public function getSum(): int
    {
        return $this->sum;
    }

    public function pay(int $price): void
    {
        $newSum = $this->sum - $price;
        if ($newSum < 0) {
            throw new \Exception('¡No tienes esa cantidad de dinero!');
        }

        $this->sum = $newSum;
    }

    public function addMoney(int $money): void
    {
        $this->sum += $money;
    }
}
```

Tenga en cuenta que la clase sólo contiene una única propiedad `privada` `suma`, que contiene el saldo actual.

Si queremos obtener el saldo actual, hay un método `getSum()` para ello, pero no tenemos forma de cambiar el nuevo valor del saldo. Sólo podemos quitar dinero con el método `pay()` o añadir dinero con el método `addMoney()`.

Gracias a este principio, siempre sabemos con seguridad que nadie puede romper el objeto.

Si el usuario intenta pagar más dinero del que realmente hay en la cuenta, el método `pay()` no lo permitirá porque realiza un cálculo de comprobación antes de sobrescribir la propiedad `$sum` y si el saldo debe ser negativo (menor que cero), se lanza una excepción de error y la operación se detiene.

Conclusión
-----

Hemos demostrado el principio básico de la encapsulación, que nos permite pensar mejor en la abstracción de objetos y aporta una perspectiva totalmente nueva.

Una vez que entiendas bien este principio, verás que <a href="/proc-use-frameworks">los frameworks empiezan a tener mucho sentido</a>, porque encapsulan internamente un montón de ingenio que puedes utilizar sin más.

La próxima vez veremos <a href="/dedicatoria-y-visibilidad">dedicatoria y visibilidad</a>.
