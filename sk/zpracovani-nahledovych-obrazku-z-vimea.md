Spracovanie miniatúrnych obrázkov a metainformácií zo služby Vimeo
==================================================================

> id: '8b8cbef0-210a-4883-be07-5004e8f43739'
> slug:
> 	cs: zpracovani-nahledovych-obrazku-z-vimea
> 	sk: spracovanie-miniaturnych-obrazkov-a-metainformacii-zo-sluzby-vimeo
> 
> publicationDate: '2020-09-19 17:32:31'
> mainCategoryId: a0143f3c-ac75-46dc-a514-d3c9417ded4e
> sourceContentHash: '93c29218f162fb5dc8d0f265968b0f8d'

Pri vkladaní videí zo služby Vimeo do stránky (ako vložené HTML) môžeme často chcieť získať aj obrázok a ďalšie užitočné informácie, ako je dĺžka videa, úplný názov, autor atď.

Našťastie Vimeo poskytuje jednoduché rozhranie HTTP API, z ktorého môžeme načítať všetky údaje na základe tokenu videa.

Ak sa chcete vyhnúť tomu, aby ste museli API písať sami, stačí použiť [ready package](https://github.com/baraja-core/vimeo-video-api), ktorý API úplne integruje.

Balík nainštalujete pomocou príkazu:

composer require baraja-core/vimeo-video-api

Ľahko sa používa. Vytvoríte inštanciu služby `\Baraja\VimeoAPI\VimeoVideoAPI` na komunikáciu so službou Vimeo podľa dokumentácie, zavoláte metódu `getInfo()`, odovzdáte token videa a získate podrobné informácie ako entitu `VideoInfo`, z ktorej možno vyčítať všetky dostupné informácie (nie vždy sú všetky informácie dostupné pre každé video).

Takto môžete vyhľadávať aj súkromné a verejne nedostupné videá. Vždy však musíte poznať ich token.

Uvedenie všetkých dostupných informácií
---------

Základný spôsob používania knižnice vyzerá takto:

```php
$api = new \Baraja\VimeoAPI\VimeoVideoAPI;

$token = 0; // Video token ako celé číslo
$info = $api->getInfo($token);

echo var_dump($info); // zoznamy všetkého

// Vypíšte dĺžku videa v sekundách:
echo "Dĺžka videa je: . $info->getDuration();
```

V premennej `$info` sú uložené všetky popisné informácie o konkrétnom videu. Prehľad všetkých dostupných metód [nájdete v implementácii](https://github.com/baraja-core/vimeo-video-api/blob/master/src/VideoInfo.php).
