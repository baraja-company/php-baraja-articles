Cadenas y contraseñas de Hashing
================================

> id: '7978bee8-62cc-4770-b15b-a8d08d1dcf34'
> slug:
> 	cs: hashovani
> 	es: cadenas-y-contrasenas-de-hashing
> 
> perex:
> 	- 'Hash není šifra! Metody hashování dat a hesel. MD5, SHA1, Bcrypt. Ověření hesla.'
> 	- 'El hash no es una cifra. Métodos de hashing de datos y contraseñas. MD5, SHA1, Bcrypt. Verificación de la contraseña.'
> 
> publicationDate: '2019-09-11 10:13:30'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '5d1e289fd93e18ad73eb23ee1bbba8ee'

El proceso de hashing (a diferencia de la encriptación) produce una salida a partir de la entrada de la que ya no se puede derivar la cadena original.

Por lo tanto, es muy adecuado para proteger cadenas sensibles, contraseñas y sumas de comprobación.

Otra buena característica de las funciones hashing es que siempre generan salidas de la misma longitud, y un pequeño cambio en la entrada siempre cambia por completo la salida.

Funciones de hashing
----------------

Hay muchas funciones hash en PHP, las más importantes son:

- **Bcrypt: password_hash()** - El hash más seguro para contraseñas, computacionalmente lento, usa la sal interna y hace el hash de forma iterativa.
- **md5()** - Función muy rápida adecuada para el hash de archivos. La salida es siempre de 32 caracteres.
- **sha1()** - Función hash rápida para el hash de archivos, utilizada internamente por Git para el hash de confirmaciones. La salida es siempre de 40 caracteres.

Hashing
-----------

```php
$password = 'contraseña secreta';

echo password_hash($password); // Bcrypt
echo md5($password);
echo sha1($password);
```

> **Atención:** Ni `md5()` ni `sha1()` son adecuados para el hash de contraseñas, porque es computacionalmente fácil descubrir la contraseña original, o al menos precalcular las contraseñas. Es mucho mejor utilizar `bcrypt`, que fue desarrollado para el hash de contraseñas.
>
> El sitio web <a href="https://www.md5cracker.com/">md5cracker.com</a> contiene una base de datos de sumas de comprobación (hashes), prueba a buscar el hash: `79c2b46ce2594ecbcb5b73e928345492`, como puedes ver, así que el `md5()` puro no es tan seguro para palabras y contraseñas comunes.

La única solución correcta: `Bcrypt + salt`.
--------------------------------------

En la charla <a href="https://www.youtube.com/watch?v=F58_A5TM-Sc">Cómo no meter la pata en el plano de destino</a>, David Grudl abordó las formas de hashear y almacenar correctamente las contraseñas.

La única solución correcta es: `Bcrypt + salt`.

Específicamente:

```php
$password = 'hash';

// Genera un hash seguro
echo password_hash($password, PASSWORD_BCRYPT);

// Alternativamente con mayor complejidad (por defecto es 10):
echo password_hash($password, PASSWORD_BCRYPT, ['costo' => 12]);
```

La ventaja de Bcrip radica principalmente en su rapidez y en el salado automático.

El hecho de que se tarde **mucho** en generar, digamos 100 ms, hace que sea muy costoso para un atacante probar muchas contraseñas.

Además, el hash de salida se trata automáticamente con **sal aleatoria**, lo que significa que cuando se hace hash de la misma contraseña repetidamente, la salida es siempre un hash diferente. Por lo tanto, un atacante no podrá utilizar una tabla hash precalculada.

Por lo tanto, no podremos verificar la exactitud de la contraseña mediante el hash repetido, sino que tendremos que llamar a una función especializada:

```php
if (password_verify($password, $hash)) {
    // La contraseña es correcta
} else {
    // La contraseña es incorrecta
}
```

Salado de contraseñas
------------

Para dificultar el descifrado del hash, es una buena idea insertar alguna cadena adicional en la entrada original. Lo ideal sería uno al azar. Este proceso se denomina **Salting de la contraseña**.

La seguridad se basa en la idea de que un atacante no podrá utilizar una tabla precalculada de contraseñas y hashes, porque no conocerá la sal y tendrá que romper las contraseñas individualmente.

Por ejemplo:

```php
$password = 'pasaporte_secreto';
$salt = 'fghjgtzjhg';

$hash = md5($password . $salt);

echo $password; // imprime la contraseña original
echo $hash;     // imprime el hash de la contraseña incluyendo la sal
```

Funciones hash compuestas
------------------------

Podrías pensar que sería una buena idea realizar la función hash repetidamente, aumentando así la complejidad de su crackeo, ya que la contraseña original necesitará ser hashada repetidamente.

Por ejemplo:

```php
$password = 'contraseña';

for ($i = 0; $i <= 1000; $i++) {
    $password = md5($password);
}

echo $password; // 1000x hashed via md5()
```

Paradójicamente, la dificultad para abrirse paso se reduce o permanece casi igual.

La razón es que la función `md5()` es extremadamente rápida y puede calcular más de un millón de hashes por segundo en un ordenador normal, por lo que probar las contraseñas una por una no ralentiza mucho.

La segunda razón es más bien una teoría, a saber, la posibilidad de encontrarse con una supuesta colisión. Si hacemos un hash de una contraseña repetidamente, con el tiempo puede ocurrir que demos con un hash que el atacante ya conoce, y esto le permitirá hacer un hash de la contraseña utilizando la base de datos.

Por lo tanto, es mejor utilizar una función de hashing segura y lenta y realizar el hashing una sola vez, sin dejar de tratar la salida final con salting.
