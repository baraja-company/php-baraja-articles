Elaborazione di immagini in miniatura e meta informazioni da Vimeo
==================================================================

> id: '8b8cbef0-210a-4883-be07-5004e8f43739'
> slug:
> 	cs: zpracovani-nahledovych-obrazku-z-vimea
> 	it: elaborazione-di-immagini-in-miniatura-e-meta-informazioni-da-vimeo
> 
> publicationDate: '2020-09-19 17:32:31'
> mainCategoryId: a0143f3c-ac75-46dc-a514-d3c9417ded4e
> sourceContentHash: '93c29218f162fb5dc8d0f265968b0f8d'

Quando si incorporano video da Vimeo in una pagina (come un embed HTML), spesso si vuole ottenere anche l'immagine e altre informazioni utili come la lunghezza del video, il titolo completo, l'autore e così via.

Fortunatamente, Vimeo fornisce una semplice API HTTP da cui possiamo leggere tutti i dati basati sul token del video.

Per evitare di dover scrivere l'API da soli, basta usare [ready package](https://github.com/baraja-core/vimeo-video-api), che integra completamente l'API.

Si installa il pacchetto con il comando:

```shell
composer require baraja-core/vimeo-video-api
```

È facile da usare. Si crea un'istanza del servizio `\Baraja\VimeoAPI\VimeoVideoAPI` per comunicare con Vimeo secondo la documentazione, si chiama il metodo `getInfo()`, si passa il token del video e si ottengono informazioni dettagliate come entità `VideoInfo` da cui si possono leggere tutte le informazioni disponibili (non tutte le informazioni sono sempre disponibili per ogni video).

In questo modo è possibile interrogare anche i video privati e quelli non disponibili pubblicamente. Ma bisogna sempre conoscere il loro gettone.

Elencare tutte le informazioni disponibili
---------

Un modo di base per usare la libreria è questo:

```php
$api = new \Baraja\VimeoAPI\VimeoVideoAPI;

$token = 0; // token video come intero
$info = $api->getInfo($token);

echo var_dump($info); // elenca tutto

// Stampa la lunghezza del video in secondi:
echo 'La lunghezza del video è:' . $info->getDuration();
```

La variabile `$info` memorizza tutte le informazioni descrittive di un particolare video. Una panoramica di tutti i metodi disponibili [può essere trovata nell'implementazione] (https://github.com/baraja-core/vimeo-video-api/blob/master/src/VideoInfo.php).
