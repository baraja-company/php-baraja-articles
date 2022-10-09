Behandling af miniaturebilleder og metaoplysninger fra Vimeo
============================================================

> id: '8b8cbef0-210a-4883-be07-5004e8f43739'
> slug:
> 	cs: zpracovani-nahledovych-obrazku-z-vimea
> 	da: behandling-af-miniaturebilleder-og-metaoplysninger-fra-vimeo
> 
> publicationDate: '2020-09-19 17:32:31'
> mainCategoryId: a0143f3c-ac75-46dc-a514-d3c9417ded4e
> sourceContentHash: '93c29218f162fb5dc8d0f265968b0f8d'

Når vi indlejrer videoer fra Vimeo på en side (som en HTML-indlejring), vil vi ofte også gerne have billedet og andre nyttige oplysninger som f.eks. videolængde, fuld titel, forfatter og så videre.

Heldigvis tilbyder Vimeo et simpelt HTTP API, hvorfra vi kan læse alle data baseret på videotokenet.

Hvis du vil undgå at skulle skrive API'et selv, skal du blot bruge [ready package] (https://github.com/baraja-core/vimeo-video-api), som integrerer API'et fuldstændigt.

Du installerer pakken med kommandoen:

```shell
composer require baraja-core/vimeo-video-api
```

Den er nem at bruge. Du opretter en instans af tjenesten `\Baraja\VimeoAPI\VimeoVideoAPI` for at kommunikere med Vimeo i henhold til dokumentationen, kalder metoden `getInfo()`, overfører videotokenet og får detaljerede oplysninger som en `VideoInfo`-enhed, hvorfra alle tilgængelige oplysninger kan læses (ikke alle oplysninger er altid tilgængelige for hver video).

På denne måde kan du søge efter private og offentligt utilgængelige videoer. Men du skal altid kende deres symbol.

Oplysning om alle tilgængelige oplysninger
---------

En grundlæggende måde at bruge biblioteket på ser således ud:

```php
$api = new \Baraja\VimeoAPI\VimeoVideoAPI;

$token = 0; // Video token som heltal
$info = $api->getInfo($token);

echo var_dump($info); // opregner alt

// Udskriv længden af videoen i sekunder:
echo 'Videoens længde er:' . $info->getDuration();
```

Variablen `$info` gemmer alle beskrivende oplysninger om en bestemt video. En oversigt over alle tilgængelige metoder [findes i implementeringen] (https://github.com/baraja-core/vimeo-video-api/blob/master/src/VideoInfo.php).
