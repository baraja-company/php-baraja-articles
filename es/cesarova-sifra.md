Cifrado del César: cómo funciona
================================

> id: '662dd627-c106-406e-ad6a-7ae9a492ad92'
> slug:
> 	cs: cesarova-sifra
> 	es: cifrado-del-cesar-como-funciona
> 
> publicationDate: '2019-08-23 15:43:02'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '892ad0746be469b5cdbb4de72d8e85b9'

> **Atención:** Este artículo fue escrito hace muchos años y parte de la información puede ser obsoleta o incorrecta. Téngalo en cuenta al leerlo.

El cifrado César es una de las funciones hash más sencillas. En su día era prácticamente irrompible, pero en la era de los ordenadores modernos sólo se necesitan unas decenas de segundos, incluso unos minutos, para romperlo. Se basa en una clave, según la cual se encripta el mensaje y según la cual se puede volver a expandir. Por lo tanto, la clave es secreta. En el momento de la encriptación, el mensaje puede ser visto y no significará nada (sólo un amasijo de caracteres). La única manera de romper el cifrado es adivinar la clave.

La clave
--------------------------

La clave puede ser cualquier número entero que tenga menos dígitos que el propio mensaje. Normalmente se dan 3 dígitos válidos (por lo que hay 999 combinaciones). Cada dígito adicional aumenta la seguridad. Para que dos partes se comuniquen, ambas necesitan conocer esta clave secreta (por lo que deben pasársela de alguna manera a la otra de forma segura). Si la clave es conocida por ellos y no por el otro, entonces el mensaje puede difundirse incluso de forma insegura y potencialmente no comprometer el contenido, porque el potencial atacante no conoce el procedimiento para recuperar el mensaje.

Para fines de demostración, utilizaré la clave **123**, normalmente se utiliza algún otro número aleatorio que se supone no es tan fácil de adivinar.

Procedimiento de encriptación
--------------------------

El principio se basa en la idea de sustituir los caracteres de un mensaje por otros mediante una clave. Yo lo llamo "cambio de carácter".

Por ejemplo, tengamos este mensaje que queremos cifrar:

```php
TAJNA ZPRAVA
```

Ahora asigna un número a cada personaje. Normalmente por el alfabeto (su número de serie). Utilizaré el alfabeto inglés, así que esta serie de caracteres:

```php
A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
```

Si asignamos un número a cada carácter, obtenemos algo así:

```php
20-1-10-14-1   26-16-18-1-22-1
```

Ahora viene la clave. Tomamos cada número individual y le añadimos la clave. Resalto en color lo que he añadido donde:

`1 2 3`

```php
21 3 13 15 3   3 17 20 4 23 3
```

Obsérvese que cuando cifro el carácter **Z**, escribo el dígito 3. Esto se debe a que **Z** es el último carácter del alfabeto, por lo que cuando llega al final, vuelve a contar desde el principio de la línea.

Transmisión del mensaje
--------------------------

Ahora podemos pasar el mensaje como queramos. A veces incluso públicamente. Otros sólo verán una serie ilógica de números, por lo que probablemente ni siquiera sabrán que se trata de algún tipo de cifrado. La seguridad también se ve reforzada por la propia clave, que es secreta. A veces conviene transmitir el mensaje como una serie de números, otras veces conviene convertir esos números en caracteres (de nuevo, siguiendo la misma serie alfabética) y luego transmitir una secuencia de caracteres. Depende mucho de las circunstancias. Sin embargo, en general es mejor una secuencia numérica, porque pocas personas sospecharán que se trata de un mensaje codificado.

Desencriptación en el destinatario
--------------------------

El destinatario descifra utilizando el mismo procedimiento. Toma cada carácter individual y resta los números según la clave y luego convierte los valores resultantes de nuevo en caracteres utilizando el alfabeto. Es sólo un procedimiento de encriptación inversa. Lo importante es conocer la **clave** y el **conjunto de caracteres**, es decir, cómo van los caracteres en la secuencia.

Cracking y descifrado
--------------------------

El único procedimiento posible para descifrar es probar todas las combinaciones imaginables de todas las claves potenciales. Si no conocemos la longitud de la llave, todo el proceso se complica aún más. Pero en general, los ordenadores actuales pueden probar algo así como 100 claves en 1 segundo, por lo que una clave aleatoria de 3 caracteres tarda algo así como un minuto en descifrarse.

Sin embargo, si la clave es tan larga o más que el mensaje original, no se puede romper porque cada carácter individual tiene su propia clave, por lo que hay que probar todas las combinaciones para cada carácter.

Si tuviera un mensaje "**Toma Mensaje**", tendría 11 caracteres (los espacios no cuentan). Si quisiera una clave que también tuviera 11 caracteres, y utilizara una cadena de 26 caracteres del alfabeto inglés, entonces hay **11^26** = 1,191817654*10²⁷ combinaciones, el ordenador medio descifraría esa clave en 1,310999419×10²⁶ segundos = 10^20 días :)
