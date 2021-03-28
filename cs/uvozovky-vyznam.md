Speciální význam uvozovek
================================

> id: "2649942d-94d6-48c3-9c78-5a303165bf72"
> slugCS: uvozovky-vyznam
> publicationDate: "2019-08-22 20:48:46"
> mainCategoryId: "6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070"

V PHP lze často zaměnit uvozovky za apostrofy a docílíme stejného efektu. Někdy se dokonce používá kombinace uvozovek a apostrofů záměrně, abychom docílili různých výstupů a nemuseli jsme používat tzv. escapování.

**TLDR: Použití uvozovek může být v některých kontextech nebezpečné, proto používejte raději všude apostrofy. Uvozovky se hodí jen pro speciální případy.**

| Znak | Název    |
|------|-----------
| `"`  | Uvozovka |
| `'`  | Apostrof |

Příklad první - stejný výstup
-----------------------------

```php
echo "ahoj světe!"; // toto je to samé
echo 'ahoj světe!'; // jako toto
```

V tomto případě nezáleží na použití uvozovek nebo apostrofů. Zdánlivě to tedy může vypadat, že mezi uvozovkami a apostrofy rozdíl není.

Kombinace uvozovky a apostrofů
------------------------------

Mějme následující kód:

```php
echo 'Moje oblíbená barva je "červená"!';
```

Kdybych k ohraničení vypisovaného řetězce použil `uvozovky`, tak by **bylo nejednoznačné**, kde řetězec **začíná a kde končí**, proto jsem použil k ohraničení `apostrofy`, které tento problém řeší.

Vepsání uvozovek do řetězce ohraničeného uvozovkami
---------------------------------------------------

Občas může nastat situace, kdy potřebuji vypsat jak `uvozovku`, tak `apostrof` v rámci jednoho řetězce. Použít lze nejen propojení dvou řetězců, ale i tzv. **escapování** znaků.

```php
echo "Tento řetězec \"obsahuje\" uvozovky.";
```

Zpětné lomítko má v tomto případě význam "přesně tento znak" a proto nebude uvozovka chápána jako konec řetězce (nebo něco jiného) a proto to bude vždy uvozovka. Takto lze označit i další podivné znaky, u kterých nelze zaručit jejich trvanlivost a mohlo by dojít k nepochopení správného významu.

Proměnná uvnitř řetězce
-----------------------

Uvozovky umí být extrémně zrádné, protože umožňují vkládat proměnné přímo do řetězce:

```php
$color = 'červená';

echo "Moje oblíbená barva je {$color}, a tvoje?";
```

Mnohem bezpečnější jsou apostrofy, které toto neumožňují a je potřeba řetězec složit:

```php
$color = 'červená';

echo 'Moje oblíbená barva je ' . $color . ', a tvoje?';
```

Dobré návyky
--------------------------

Obecně doporučuji k ohraničení čehokoli používat apostrofy (pokud je to možné), protože jejich výskyt v řetězcích je daleko menší, než právě uvozovek.

PHP je navíc jazykem webovým, tj. používá se k generování HTML dokumentů, kde se uvozovky vyskytují hodně často právě z toho důvodu, že se generují i části HTML kódů. Osobně doporučuji, zvyknout si na striktní použití apostrofů úplně všude, protože pak si nemusíte pamatovat, do čeho zrovna uzavíráte. 

Chování uvozovek
--------------------------

Bacha! Ještě uvozovky úplně nezahazujte! Mají totiž některé speciální výhody, které se při pokročilé práci s PHP mohou hodit - nicméně začátečníci je považují naopak za chyby a nechápou je.

```php
$x = 10; // nastavím si proměnnou
echo "Hodnota proměnné je: $x, přesně."; // a výpis
```

V rámci uvozovek lze rovnou vypisovat hodnoty proměnných, resp. znak dolaru způsobí, že vše za ním je proměnná. Pokud tedy nechcete vypsat hodnotu proměnné, ale znak dolaru, tak musíte escapovat.

```php
$price = 25; // cena v dolarech
echo "Cena produktu: $price\$"; // vypíše: "Cena produktu: 25$"
```

Výhoda uvozovek je v tomto případě sporná a možná je lepší použít apostrofy a řetězce jednoduše propojit.

```php
$price = 25;
echo 'Cena produktu: ' . $price . '$'; // vypíše to samé jako předchozí příklad
```

> **Poznámka:** Cena v dolarech se zapisuje správně ve formátu `$25`, což by způsobilo ještě větší nepřehlednost, protože zápis `$$price` ve skutečnosti volá něco, čemu se říká `proměnná proměnná` (v názvu proměnné říkáme názvev proměnné, kterou budeme volat - prostě zmatek).

**TIP:** Možná by vás mohla zajímat <a href="/promenna-promenna">Proměnná proměnná</a>.

Označení řetězce
--------------------------

Všeobecně platí, že vše, co je v uvozovkách nebo apostrofech, se chápe jako řetězec. Tedy:

```php
$x = 5;   // toto je něco jiného,
$x = "5"; // než toto.
```

V prvním případě se do proměnné **$x** uloží **číslo** 5, v druhém případě se do té samé proměnné uloží **řetězec** "5". Naštěstí to v PHP nevadí a s jakoukoli variantou můžete pracovat (téměř) stejně, protože PHP umí proměnné automaticky **přetypovat** podle jejich obsahu. Nicméně se obecně doporučuje čísla nepsat do uvozovek, zejména kvůli výpočetním operacím, kde pak může vznikat zaokrouhlovací chyba.

Někdy můžu chtít do proměnné uložit výstup nějaké funkce:

```php
$pi = 3.14159;
$zaokrouhleni = round($pi); // toto je správně
$zaokrouhleni = "round($pi)"; // toto je "špatně" (otázkou je, jaký výstup očekávám).
```

Chyba v druhém případě je právě v uvozovkách, kdy se do proměnné **$zaokrouhleni** neuloží výstup funkce **round()**, ale řetězec volající tuto funkci.
> **TIP:** Hodnotu `$pi` nemusíme zadávat takto přímo do scriptu a můžeme použít funkci `pi()`, která bývá přesnější při složitějších výpočtech.

Speciální význam escapování
--------------------------

> **Pozor:** Následující ukázky fungují pouze v uvozovkách, pokud je uzavřete do apostrofů, tak se budou chovat jako klasické znaky bez speciálního významu (kromě `\'`, které escapuje apostrof).
Escapování slouží pro případ, kdy chci vypsat nějaký speciální znak uvnitř uvozovek nebo apostrofu, který by mohl být chápán jako výraz jazyka a proto by se mohl zpracovat, i když to programátor nezamýšlel. Příklad jsme si už ukazovali, tato kapitola popisuje možné výjimky chování.

Někdy totiž i samotné escapování nese speciální význam. Příklad:

```php
echo "Dlouhý text, rozdělený na\ndva řádky.";
```

Předchozí příklad vypíše:

```php
Dlouhý text, rozdělený na
dva řádky.
```

Pokud tedy chceme vypsat lomítko, tak jej musíme také escapovat (escapování znaku `n` není v tomto případě nutné, protože by bylo opět pochopeno jako zalomení, resp. v tomto případě dokonce escapovat vůbec nesmíme):

```php
echo "Dlouhý text, rozdělený na\\ndva řádky.";
```

Podobných speciálních znaků existuje více, například `\t` udělá tabulátor. Celý seznam je v oficiální dokumentaci.

Reálné použití speciálních znaků při escapování
-----------------------------------------------

Na začátek bych rád podotknul, že se v PHP dá téměř cokoli udělat více způsoby a zdejší uváděné příklady slouží pouze pro ilustraci toho, jak lze k problému přistupovat.

Pokud chceme například rozparsovat text po řádcích, můžeme použít funkci <a href="/explode">explode</a>.

```php
$parser = explode("\n", $retezec); // rozparsuje text po řádku
```

V tomto případě dává použití speciálního znaku `\n` smysl, protože velice efektivně řekneme, že chceme parsovat podle zalomení řádku.

```php
$parser = explode('
', $retezec);
```

> **Pozor:** V Unixových a Windows systémech dochází ke zmatení znaků pro označení konce řádků. Například Windows používá znak `CRLF` (dvojice znaků `\r\n`), zatímco Linux jen `LF` (jediný znak `\n`). Při parsování podle řádků je potřeba na toto pamatovat. Obvykle se problém řeší normalizací znaků jen na `LF`.

Shrnutí
-------

Pokud můžete, použijte všude **apostrofy**.

Je dobré znát použití uvozovek a používat je jen tam, kde to je nutné (nebo obecně dobré). Pokud vypisujete text, který může obsahovat uvozovky, tak jej uzavírejte do apostrofů (které se pak chovají více předvídatelně). Já osobně uvozovky používám pro vyjádření různých speciálních znaků, které se v apostrofech zadávají zbytečně složitě a muselo by dojít ke složitému escapování.
