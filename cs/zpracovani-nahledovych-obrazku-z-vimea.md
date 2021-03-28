Zpracování náhledových obrázků a meta informací z Vimea
================================

> id: "8b8cbef0-210a-4883-be07-5004e8f43739"
> slugCS: zpracovani-nahledovych-obrazku-z-vimea
> publicationDate: "2020-09-19 17:32:31"
> mainCategoryId: a0143f3c-ac75-46dc-a514-d3c9417ded4e

Při vkládání videí z Vimea do stránky (jako HTML embed) můžeme často chtít získat také obrázek a další užitečné informace, jako je například délka videa, celý název, autor a podobně.

Naštěstí Vimeo poskytuje jednoduché HTTP API, z kterého můžeme všechna data přečíst na základě tokenu videa.

Abyste si nemuseli psát rozhraní pro API sami, stačí použít [hotový balíček](https://github.com/baraja-core/vimeo-video-api), které API kompletně integruje.

Balík nainstalujete příkazem:

```shell
composer require baraja-core/vimeo-video-api
```

Použití je jednoduché. Vytvoříte instanci služby `\Baraja\VimeoAPI\VimeoVideoAPI` pro komunikaci s Vimeem podle dokumentace, zavoláte metodu `getInfo()`, předáte token videa a dozvíte se podrobné informace jako entitu `VideoInfo`, z které lze přečíst všechny dostupné informace (ne vždy jsou dostupné všechny informace u každého videa).

Tímto způsobem se můžete zeptat i na soukromá a veřejně nedostupná videa. Vždy ale musíte znát jejich token.

Vypsání všech dostupných informací
---------

Základní způsob použití knihovny vypadá třeba takto:

```php
$api = new \Baraja\VimeoAPI\VimeoVideoAPI;

$token = 0; // Token video jako integer
$info = $api->getInfo($token);

echo var_dump($info); // vypíše vše

// Vypíše délku videa v sekundách:
echo 'Délka videa je: ' . $info->getDuration();
```

V proměnné `$info` jsou uloženy všechny popisné informace o konkrétním videu. Přehled všech dostupných metod [najdete v implementaci](https://github.com/baraja-core/vimeo-video-api/blob/master/src/VideoInfo.php).
