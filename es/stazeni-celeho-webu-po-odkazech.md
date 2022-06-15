Descarga de todo el sitio por enlaces en PHP
============================================

> id: dd4abb8e-8f9b-4867-b98f-ff1c859d387a
> slug:
> 	cs: stazeni-celeho-webu-po-odkazech
> 	es: descarga-de-todo-el-sitio-por-enlaces-en-php
> 
> publicationDate: '2019-11-06 17:41:30'
> mainCategoryId: f46a0d80-fbe4-4be8-a5e4-04a8d29b0afc
> sourceContentHash: e70451decff5c7bdf19bca8181c0dd5e

Con relativa frecuencia resuelvo la tarea de descargar todas las páginas de un sitio o dominio, porque luego realizo diversas mediciones con los resultados, o utilizo las páginas para la búsqueda de texto completo.

Una posible solución es utilizar la herramienta ya preparada [Xenu](http://home.snafu.de/tilman/xenulink.html), que es muy difícil de instalar en un servidor web (es un programa de Windows), o [Wget](https://www.gnu.org/software/wget/), que no es compatible en todas partes y crea otra dependencia innecesaria.

Si la tarea es sólo hacer una copia de la página para su posterior visualización, es muy útil el programa [HTTrack](https://www.httrack.com/), que es el que más me gusta, sólo que cuando estamos indagando URLs parametrizadas podemos perder precisión en algunos casos.

Así que empecé a buscar una herramienta que pueda indexar todas las páginas de forma automática directamente en PHP con una configuración avanzada. Finalmente, se convirtió en un proyecto de código abierto.

Baraja WebCrawler
-----------------

Precisamente para estas necesidades he implementado mi propio paquete Composer [WebCrawler](https://github.com/baraja-core/webcrawler), que puede manejar el proceso de indexación de páginas elegantemente por sí mismo, y si me encuentro con un nuevo caso, lo mejoro aún más.

Se instala con el comando Composer:

```shell
composer require baraja-core/webcrawler
```

Y es fácil de usar. Basta con crear una instancia y llamar al método `crawl()`:

```php
$crawler = new \Baraja\WebCrawler\Crawler;

$result = $crawler->crawl('https://example.com');
```

En la variable `$resultado`, el resultado completo estará disponible como una instancia de la entidad `CrawledResult`, que recomiendo estudiar porque contiene mucha información interesante sobre todo el sitio.

Configuración de la oruga
------------------

A menudo tenemos que limitar la descarga de páginas de alguna manera, porque de lo contrario probablemente descargaríamos todo Internet.

Para ello se utiliza la entidad `Config`, a la que se le pasa la configuración como un array (clave-valor) y luego se le pasa al Crawler mediante el constructor.

Por ejemplo:

```php
$crawler = new \Baraja\WebCrawler\Crawler(
    new \Baraja\WebCrawler\Config([
        // clave => valor
    ])
);
```

Opciones de configuración:

| Clave - Valor por defecto - Valores posibles
|-------------------------|---------------|-----------------|
| `followExternalLinks` | `false` | `Bool`: ¿Permanecer sólo dentro del mismo dominio? ¿Puede indexar también los enlaces externos?
| `sleepBetweenRequests` | `1000` | `Int`: Tiempo de espera entre la descarga de cada página en milisegundos.
| `maxHttpRequests` | `1000000` | `Int`: ¿Cuántas URLs descargadas como máximo?
| `maxCrawlTimeInSeconds` | `30` | `Int`: ¿Cuánto tiempo de descarga máxima en segundos?
| `allowedUrls` | `['.+']` | `String[]`: Conjunto de formatos de URL permitidos como expresiones regulares.
| `forbiddenUrls` | `[']` | `String[]`: Conjunto de formatos de URL prohibidos como expresiones regulares.

La expresión regular debe coincidir con toda la URL exactamente como una cadena.
