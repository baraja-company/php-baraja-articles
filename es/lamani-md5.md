Cómo romper la función md5
==========================

> id: '9e678fcc-3d5e-46a3-9c3f-c1eb3d1ad367'
> slug:
> 	cs: lamani-md5
> 	es: como-romper-la-funcion-md5
> 
> publicationDate: '2019-08-23 15:33:10'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '4e176a09e43dbf12e131103be6a9cf4f'

MD5 es una función muy utilizada para calcular hashes.

Los principiantes suelen utilizarlo para <a href="/hashovani">hacer hash de la contraseña</a>, lo que no es una buena idea porque hay muchas formas de recuperar la contraseña original.

Este artículo describe métodos específicos para hacerlo.

Complejidad del tiempo
----------------

Toda la seguridad se basa en el hecho de que se necesita un tiempo desproporcionadamente largo para probar todas las contraseñas. Bueno, debería. El problema con el algoritmo `md5()` en particular es que es una función muy rápida. En un ordenador normal, no hay problema para calcular más de un millón de hashes por segundo.

Si rompemos la contraseña probando combinaciones una a una, es un **ataque de fuerza bruta**.

Métodos de craqueo
----------------

Hay varias estrategias:

- Prueba y error secuencial (ataque de fuerza bruta)
- Prueba de contraseñas de diccionario
- Tablas Rainbow (base de datos hash precalculada)
- Búsquedas en Google
- Colisiones en el algoritmo

Hay muchos más métodos, este artículo sólo describe los más comunes.

Estrategias de ruptura por fuerza bruta
-----------------------------

Todas las combinaciones de letras, números y otros caracteres se prueban de una en una.

Los intentos generados se someten a un hash uno por uno y se comparan con el hash original.

Así, por ejemplo:

```php
aaaaaaabaaacaaadaaaeaaaf...
```

El problema de este ataque está en el propio algoritmo `md5()`, si probáramos sólo las letras minúsculas del alfabeto inglés y los números, tardaría como mucho decenas de minutos en probar todas las combinaciones en un ordenador comúnmente disponible.

Por lo tanto, es importante elegir contraseñas largas, preferiblemente aleatorias y con caracteres especiales.

Estrategia de ataque del diccionario
----------------------------

La gente suele elegir contraseñas débiles que existen en el diccionario.

Si aprovechamos este hecho, podemos descartar rápidamente variantes poco probables como `6w1SCq5cs` y, en su lugar, adivinar las palabras existentes.

Además, sabemos por anteriores filtraciones de contraseñas de grandes empresas que los usuarios eligen una letra mayúscula al principio de la contraseña y un número al final. Veamos, ¿también tiene eso su contraseña? :)

Tablas Rainbow - base de datos precalculada
--------------------------------------

Dado que una contraseña corresponde siempre al mismo hash, es fácil recalcular una enorme base de datos en la que se buscarán primero las contraseñas.

De hecho, la búsqueda es siempre órdenes de magnitud más rápida que la búsqueda de hashes una y otra vez.

Además, en el caso de las filtraciones de datos más grandes, las contraseñas pueden ser sometidas a hash en paralelo de esta manera y, por ejemplo, el 10% de todas las contraseñas de los usuarios pueden ser recuperadas rápidamente.

Una buena base de datos de contraseñas es por ejemplo <a href="https://crackstation.net/">Crack Station</a>.

Búsqueda en Google
-------------------

Muchas contraseñas simples son conocidas directamente por Google porque indexa las páginas que contienen hashes.

Siempre uso Google como primera opción. :)

Encontrar colisiones en el algoritmo
--------------------------

El <a href="https://cs.wikipedia.org/wiki/Dirichlet%C5%AFv_princip">Principio de Dirichlet</a> describe que si tenemos un conjunto de hashes que siempre tienen 32 caracteres, entonces hay al menos 2 contraseñas diferentes de 33 caracteres (una más larga) que generan el mismo hash.

En la práctica, no tiene sentido buscar colisiones, pero a veces el propio autor de la aplicación facilita las conjeturas volviendo a calcular las colisiones.

Por ejemplo:

```php
$password = 'contraseña';

for ($i = 0; $i <= 1000; $i++) {
    $password = md5($password);
}

echo $password; // 1000x hashed via md5()
```

En este caso, tiene sentido adivinar la colisión en lugar del hash original.

¡Salud a los hilvanes!
