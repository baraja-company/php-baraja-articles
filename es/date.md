Función PHP date(), fecha y hora
================================

> id: '9b0ec1c7-3431-4d7d-9bcc-6093285f14f1'
> slug:
> 	cs: date
> 	es: funcion-php-date-fecha-y-hora
> 
> perex:
> 	- 'Zjištění data a času, aktuální datum, formátování data a času a převod tvaru.'
> 	- 'Detección de fecha y hora, fecha actual, formato de fecha y hora y conversión de formas.'
> 
> publicationDate: '2019-09-11 10:14:16'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: d0323ba88fcdba84ef45439991301f6c

La función `date()` es una herramienta para trabajar con la fecha y la hora. Se utiliza en dos casos:

- **Encontrar el estado actual**, es decir, la fecha actual, la hora, ... y la salida en un formato específico,
- **Convertir** una fecha específica en otro formato (por ejemplo, número de mes a nombre, formato de notación de año, sistema de 12 y 24 horas, ...).

Muestra
------

```html
Já jsem speciální stránka. Vím, že právě je
<?php
    echo date('H:i'); // Hodina:minuta
?>
```

> ADVERTENCIA: PHP no imprime su hora, sino la del servidor. Por lo tanto, es posible que obtenga una hora diferente a la establecida en su ordenador.

Sintaxis de entrada
--------------------------

La función se llama de forma normal y las solicitudes individuales se introducen como argumentos de la función.

```php
echo date('marcas de formato', atribut konkrétního času);
```

Las marcas de formato indican en qué formato se imprimirá la fecha. Las marcas pueden incluir espacios, puntos, dos puntos, guiones, rayas y otros caracteres que no son en sí mismos marcas de formato (si desea utilizar una marca de formato, debe <a href="/carriage-notes">escaparla</a>). A continuación se ofrece un resumen de cada etiqueta.

El segundo atributo (opcional) indica la fecha u hora introducida manualmente, que será convertida y emitida en el formato según el primer parámetro. Debe especificarse como una *firma de tiempo* (puede obtenerse mediante la etiqueta de formato "U").

Ejemplo:

```php
echo date('d. m. Y', 1405856605); // antes del 20. 07. 2014
```

Tabla de marcas de formato permitidas
--------------------------

| Descripción
|------|---------------------
| Año con cuatro dígitos (por ejemplo, 1998)
| Año en forma de dos dígitos (por ejemplo, 98)
| `M` | Abreviatura en inglés del nombre del mes (por ejemplo, Jan)
| `m` | Número de mes (01-12)
| `F` | Nombre del mes en inglés (p. ej. enero)
| `D` | Abreviatura inglesa del día de la semana (por ejemplo, Fri)
| `l` | Nombre en inglés del día de la semana (por ejemplo, viernes)
| `N` | Número del día de la semana (1 - lunes, 7 - domingo)
| `w` | Número del día de la semana (0 - domingo, 1 - lunes, 6 - sábado)
| `d` | Día del mes (01-31)
| `j` | Número del día del mes (1-31)
| `z` | Día del año (001-365)
| Hora (00-23)
| Hora (01-12)
| `i` | Minuto (00-59)
| `s` | Segundo (00-59)
| `U` | *Timestamp:* Número de segundos desde el inicio del tiempo (desde el 1 de enero de 1970)
| `S` | Terminación inglesa del número ordinal del día del mes
| Indicador AM/PM
| Indicador de mañana/tarde (am/pm)
| `P` | Diferencia con la hora de Greenwich (GMT) con separador entre horas y minutos (añadido en PHP 5.1.3), por ejemplo: `+02:00`.
| `g` | Hora en formato de 12 horas (1-12)
| G` | Hora en formato de 24 horas (0-23)

Formato de tiempo para el mapa del sitio
---------------------------------

Muy a menudo es necesario formatear la hora del archivo `sitemap.xml`, que contiene la fecha y la hora del último cambio en la etiqueta `<lastmod>`.

A partir de PHP 5.1.3, se puede utilizar la siguiente sintaxis para hacerlo:

```php
date('Y-m-d\TH:i:sP');
```

La fecha y la hora se mostrarán con una precisión de segundos y también incluirán información sobre la zona horaria en la que se encuentra el servidor (la zona horaria la determina el sistema operativo del servidor).

Obtener los nombres checos de los días y los meses
----------------------------------

No es posible obtener los nombres checos de los días y los meses en PHP de la manera normal, por lo que tenemos que escribir dichos valores nosotros mismos. La mejor manera es almacenar las entradas en un array y recuperarlas mediante una llamada a un índice.

```php
$mesice = [
    1 => 'Enero', 'Febrero', 'Marzo', 'Abril', 'Mayo',
    'Junio', 'Julio', 'Agosto', 'Septiembre', 'Octubre',
    'Noviembre', 'Diciembre'
];

$dny = ['Domingo', 'Lunes', 'Martes', 'Miércoles', 'Jueves', 'Viernes', 'Sábado'];
```

Este ejemplo es un ejemplo simplificado de <a href="https://php.vrana.cz">**Jakub Vrana**</a> del artículo <a href="https://php.vrana.cz/ceske-nazvy-mesicu-a-dnu-v-tydnu.php">Nombres checos de los meses y días de la semana</a>.

Traducción de fechas del inglés al inglés
--------------------------------------

A menudo tenemos una fecha con el formato `Viernes, 13 de septiembre` y queremos traducirla al inglés, pero ¿cómo hacerlo? La mejor manera es llamar a alguna función, por ejemplo `datumcesky()`, a la que le pasamos la fecha en inglés y la traduce.

```php
function datumCesky(string $date): string
{
    $men = [
        'Enero', 'Febrero', 'Marzo', 'Abril', 'Mayo',
        'Junio', 'Julio', 'Agosto', 'Septiembre', 'Octubre',
        'Noviembre', 'Diciembre'
    ];

    $mcz = [
        'Enero', 'Febrero', 'Marzo', 'Abril', 'Mayo',
        'Junio', 'Julio', 'Agosto', 'Septiembre', 'Octubre',
        'Noviembre', 'Diciembre'
    ];

    $date = str_replace($men, $mcz, $date);

    $den = [
        'Lunes', 'Martes', 'Miércoles', 'Jueves',
        'Viernes', 'Sábado', 'Domingo'
    ];

    $dcz = [
        'Lunes', 'Martes', 'Miércoles', 'Jueves',
        'Viernes', 'Sábado', 'Domingo'
    ];

    return str_replace($den, $dcz, $date);
}
```

Ejemplo de uso:

```php
echo datumCesky('Viernes, 13 de septiembre'); // Viernes, 13 de diciembre
```

Esta función no es ideal para la traducción, ya que sólo sustituye las palabras inglesas por las checas, pero puede ser suficiente para muchas implantaciones. En el caso de traducciones más avanzadas, siempre hay que garantizar la sintaxis exacta para traducir en un estilo uniforme, por ejemplo, `Viernes, 13 de diciembre`.

Encontrar el primer día del mes
-----------------------------

El primer día de abril de 2018 fue un domingo, pero ¿cómo averiguarlo fácilmente?

A partir de PHP 5.1.0, existe una solución sencilla:

```php
echo date('N', strtotime('2018-04-01')); // 1 (lunes), 7 (domingo)
```

En las versiones anteriores tenemos que implementar la función nosotros mismos:

```php
/**
 * @autor Jan Barášek
 */
function getFirstDayPosition(?int $year = null, ?int $month = null): int
{
    $day = (int) date('w', strtotime($year . '-' . $month . '-1')) - 1;

    return $day < 0 ? 7 : $day + 1;
}
```

Como el modificador `w` devuelve la salida en formato US, sigo modificando el número del día mediante un simple cálculo. La función devuelve un número entero entre 1 (lunes) y 7 (domingo).

Desplazamientos de tiempo / conversión de formatos y validación de fechas
--------------------------------------------------

A menudo necesitamos saltar por un tiempo relativo (digamos +5 días), o sacar una fecha de la entrada de texto de un usuario para que sea válida. Para ello, la función <a href="https://www.php.net/manual/en/function.strtotime.php">strtotime()</a> permite la siguiente sintaxis:

```php
echo strtotime('ahora');
echo strtotime('10 de septiembre de 2000');
echo strtotime('+1 día');
echo strtotime('+1 semana');
echo strtotime('+1 semana 2 días 4 horas 2 segundos');
echo strtotime('el próximo jueves');
echo strtotime('el pasado lunes');
```

La salida de la función es el **timestamp** *(tiempo universal)* de la hora especificada como un entero.

> En general, recomiendo desplegar esta función en todos los formularios donde trabajemos con el tiempo del usuario de alguna manera. Ciertamente, no es una buena idea obligar al usuario a escribir la fecha en un formato determinado, sino que siempre se crea dicho formato automáticamente para que el usuario pueda escribir lo que quiera. Porque a menudo el texto, por ejemplo, se copia de algún sitio y es demasiado trabajo reformularlo manualmente cuando se puede hacer automáticamente.

Número de segundos desde el inicio del tiempo
--------------------------

Desde el 1 de enero de 1970, cada segundo se suma a uno para obtener un enorme número entero que indica el tiempo transcurrido desde entonces. Esto es especialmente útil para calcular simplemente las diferencias de tiempo. Es una solución bastante fiable, pero tiene sus riesgos (el problema de 2038).

**Propiedades generales de esta notación:**
- Es un número entero, así que es fácil trabajar con él,
- Está en el sistema decimal, por lo que es fácil de leer para los humanos (excepto que no se puede saber rápidamente cuál es la hora exacta),
- No se puede utilizar para representar el periodo anterior al 1 de enero de 1970, porque no puede ser negativo *(algunas versiones nuevas del algoritmo implementan tiempos negativos, pero no siempre se puede confiar en ello)*,
- En 2038 dejará de funcionar en ordenadores de 32 bits y provocará el bloqueo del programa (debido al desbordamiento de la pila y al tamaño máximo de los enteros).

El problema de 2038
--------------------------

<h2 id="universalTime" style="background: #222; margin-top: 32px; padding: 32px; color: white; text-align: center; border-radius: 3px;">No estoy leyendo...</h1>

<script>
	setInterval(function() {
		window.document.getElementById('universalTime').innerHTML = Math.floor(Date.now() / 1000);
	}, 250);
</script>

> Sobre este texto se muestra el valor de la hora actual, que se incrementa en +1 cada segundo. El valor actual es cargado por javascript, por lo que puede no ser siempre preciso (indica la hora del sistema de su ordenador).

Los ordenadores almacenan los números enteros en forma binaria, y si es un número entero, suele tener 32 bits (32 unos y ceros). El mayor número posible de 32 bits es: `011 111 111 111 111 111 111 11`.

Sin embargo, si sumamos +1 a esta constante cada segundo, un día obtendremos tal número *(será el 19 de enero de 2038 a las 03:14:07)*. Sin embargo, cuando intentemos añadir otro 1, el rango de bits ya no será suficiente (el número sería más grande de lo que se puede almacenar en 32 bits), por lo que se producirá un **desbordamiento de la pila**, es decir, se añadirá un 1 al principio del número, lo que en forma binaria significa un **número negativo**, (por lo que esto probablemente hará que el programa se bloquee), lo que nos llevará a algún lugar del año 1901 (¡ugh!).

Hay dos soluciones a este problema:

- Ampliando el rango de los enteros de 32 a 64 bits, lo que nos da un margen de varios miles de años
- Utilice el tipo de datos `\DateTime` (comúnmente disponible en PHP), que proporciona un enfoque orientado a objetos para la gestión de la fecha, es bien compatible con la base de datos, y ofrece un rango de años de `0001` a `9999`, que es suficiente para las necesidades de la mayoría de las aplicaciones del mundo real.

Personalmente, soy partidario de utilizar el tipo de datos `\DateTime` y no utilizar el almacenamiento de enteros en absoluto.

Diferentes zonas horarias
-----------------

PHP es inteligente, por lo que siempre intenta utilizar la mejor zona horaria actual donde se encuentra el servidor. Sin embargo, a veces puede ocurrir que la zona horaria esté mal especificada, o que estemos programando una aplicación global a la que acceden usuarios de todo el mundo y, por tanto, tengamos que cambiar manualmente la zona horaria.

Esto puede hacerse globalmente para todo el script PHP llamando a una función:

```php
date_default_timezone_set('UTC');
```

Ocasionalmente, también se puede encontrar esta solución (detectar que el servidor tiene una zona horaria distinta de UTC):

```php
$defaultTimeZone = 'UTC';

if (date_default_timezone_get() != $defaultTimeZone) {
    date_default_timezone_set($defaultTimeZone);
}
```

Cambiar la fecha y la hora
--------------------------

No es el propio PHP el responsable de obtener la fecha y hora actuales, sino el sistema operativo en el que se ejecuta. Así que si necesitas cambiar la hora manualmente, cámbiala ahí.
