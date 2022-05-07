Behandling av miniatyrbilder och metainformation från Vimeo
===========================================================

> id: '8b8cbef0-210a-4883-be07-5004e8f43739'
> slug:
> 	cs: zpracovani-nahledovych-obrazku-z-vimea
> 	sv: behandling-av-miniatyrbilder-och-metainformation-fran-vimeo
> 
> publicationDate: '2020-09-19 17:32:31'
> mainCategoryId: a0143f3c-ac75-46dc-a514-d3c9417ded4e
> sourceContentHash: '93c29218f162fb5dc8d0f265968b0f8d'

När du bäddar in videor från Vimeo på en sida (som en HTML-inbäddning) vill du ofta också få fram bilden och annan användbar information, t.ex. videolängd, fullständig titel, författare och så vidare.

Som tur är tillhandahåller Vimeo ett enkelt HTTP API från vilket vi kan läsa alla data baserat på videotoken.

För att slippa skriva API:et själv kan du använda [ready package] (https://github.com/baraja-core/vimeo-video-api), som integrerar API:et helt och hållet.

Du installerar paketet med kommandot:

```shell
composer require baraja-core/vimeo-video-api
```

Den är lätt att använda. Du skapar en instans av tjänsten `\Baraja\VimeoAPI\VimeoVideoAPI` för att kommunicera med Vimeo enligt dokumentationen, anropar metoden `getInfo()`, skickar videotoken och får detaljerad information som en enhet `VideoInfo` från vilken all tillgänglig information kan läsas (all information är inte alltid tillgänglig för varje video).

På så sätt kan du söka efter privata och offentligt otillgängliga videor. Men du måste alltid känna till deras symboliska symbol.

Lista all tillgänglig information.
---------

Ett enkelt sätt att använda biblioteket ser ut så här:

```php
$api = new \Baraja\VimeoAPI\VimeoVideoAPI;

$token = 0; // Video token som heltal
$info = $api->getInfo($token);

echo var_dump($info); // listar allt

// Skriv ut videons längd i sekunder:
echo 'Längden på videon är:' . $info->getDuration();
```

Variabeln `$info` lagrar all beskrivande information om en viss video. En översikt över alla tillgängliga metoder [finns i genomförandet] (https://github.com/baraja-core/vimeo-video-api/blob/master/src/VideoInfo.php).
