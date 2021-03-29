Superglobální proměnné
======================

> id: bc2107ee-6551-4559-8c02-9cecdf98d33b
> slugCS: superglobalni-promenna
> publicationDate: "2019-11-01 09:29:46"
> mainCategoryId: "0e39aee9-2818-480c-8081-e0c2d039bb24"

Superglobální proměnné slouží k předávání globálního stavu aplikace a HTTP komunikace.

Výhoda těchto proměnných je hlavně ta, že jsou k dispozici vždy a všude. V praxi se jedná o pole hodnot, kde přistupujeme ke konkrétní informaci podle indexu. V různých kontextech se může dostupnost klíčů lišit (vysvětleno níže).

Druhy superglobálních proměnných
--------------------------------

Všechny superglobální pole v PHP jsou pole a značí se dolarem následovaný podtržítkem (kromě `$GLOBALS`) a velkými znaky.

V `PHP 7` existují zejména tyto:

| Proměnná    | Popis |
|-------------|-------|
| `$_GET`     | URL parametry <a href="/metody-odesilani-dat">odeslané metodou GET</a>
| `$_POST`    | Data z formuláře <a href="/metody-odesilani-dat">odeslaná metodou POST</a>. Pozor, <a href="/ajax-post">při ajaxu se může chovat jinak</a>.
| `$_REQUEST` | Data z formuláře odeslaná libovolnou metodou (`$_GET`, `$_POST` a `$_REQUEST`).
| `$_FILES`   | Technické informace o právě nahranávaných souborech, například přes konstrukci `<input type="file">`
| `$_SERVER`  | <a href="/info">Nastavení webového serveru</a>, IP adresa, konfigurace... liší se podle prostředí (při zavolání PHP scriptu z Terminálu bude obsahovat jiné hodnoty a například bude chybět informace o aktuálním requestu).
| `$_COOKIE`  | Nastavené <a href="/cookies">cookies</a>.
| `$_SESSION` | Data relace (<a href="/sessions">session</a>), pokud existuje a byla v minulosti nastavena.
| `$GLOBALS ` | **Pozor, neobsahuje v názvu podtržítko!** Jde o tzv. <a href="globalni-promenna">Globální proměnnou</a> a alternativní zápis pro klíčové slovo `global`. Pokud máte v aplikaci globální proměnnou `$promenna`, lze k ní přistupovat taky konstrukcí `$GLOBALS["promenna"]`. Použití globálních proměnných je ale návrhově špatné a nečisté řešení, proto to raději nedělejte.
| `$_ENV`     | Informace o aktuálním prostředí, kde PHP běží.

Vypsání všech existujících hodnot uděláme jednoduše:

```php
foreach ($_SERVER as $key => $value {
	echo $key . ': ' . $value . '<br>';
}
```

> Pozor: Ne všechny indexy musí vždy existovat (například pokud script spustí cron v CLI režimu, tak nebude existovat index s URL adresou stránky nebo IP adresa requestu).

Přístup k proměnným
-------------------

Doporučuji veškeré globální proměnné (kromě `$_SESSION`) používat jen a pouze pro čtení. Obsahují totiž globální data aplikace a jiný kód s tím může počítat (například jiná nainstalovaná knihovna).

Nevýhoda globálního stavu je také v tom, že se nelze na přesné hodnoty vždy spolehnout a to dokonce ani na jejich existenci, proto jejich klíče vždy raději kontrolujte konstrukcí `isset()`.

Pro uložení nové cookies použijte funkci `setcookie()` a nevkládejte hodnotu přímo. Ta je totiž pouze pro čtení.

Poučení
-------

Nikdy slepě nevěřte hodnotám superglobálních proměnných!

Uživatel může pomocí URL a odeslaných hlaviček ovlivnit, jak budou hodnoty nastaveny. Veškeré vstupy je potřeba vždy pečlivě validovat.

Register globals - průšvih staré verze PHP
------------------------------------------

Ve staré verzi PHP (do verze `5.4.0`) existovala speciální direktiva `register-globals` (konfigurovatelné v `php.ini`), která způsobovala, že se veškeré předané parametry v URL automaticky zaregistrovaly jako proměnné.

Například:

Uživatel přišel na URL: `https://example.com/script.php?var=24`

A PHP v rámci scriptu automaticky vytvořilo proměnnou `$var` s hodnotou `24`.

Takže fungovalo klasicky:

```php
<?php

echo $var;
```

Kdokoli tedy mohl do scriptu podstrčit libovolnou proměnnou a měnit její obsah. Bezpečnost evidentně nebyla vždy priorita. Notykrávo.

Další zdroje
------------

Podrobnější popis najdete v <a href="https://www.php.net/manual/en/language.variables.superglobals.php">oficiálním manuálu</a>.
