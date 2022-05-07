Ako napísať svoj prvý skript PHP
================================

> id: '2d6986dc-f24b-4ae5-b24c-d5bb9bf94512'
> slug:
> 	cs: prvni-script
> 	sk: ako-napisat-svoj-prvy-skript-php
> 
> perex: Jak napsat první PHP script? Úvod do PHP pro začátečníky.
> publicationDate: '2020-02-16 16:26:08'
> mainCategoryId: f4a34087-1b51-4761-8128-4459dfe83d8a
> sourceContentHash: a5fab0e5edf99323140e497a6fe52322

Skôr ako napíšeme náš prvý skript PHP, musíme si najprv teoreticky vysvetliť, ako načítať stránku pomocou PHP.

1. Najprv používateľ vo svojom webovom prehliadači zavolá konkrétnu adresu URL, napríklad `https://baraja.cz`.
2. Potom webový prehliadač vytvorí `požiadavku`, čo je špeciálna požiadavka na webový server, ktorá sa odošle na internet. Obsahuje informácie o požadovanej stránke, základnú identifikáciu a nastavenia prehliadača, informácie o súboroch cookie atď.
3. Táto `žiadosť` putuje cez internet na webový server (najčastejšie `Apache`), ktorý ju prečíta a začne zostavovať odpoveď.
4. Keďže volaná adresa URL je skript PHP a požiadavka je na súbor s názvom `index.php`, `Apache` prečíta súbor `index.php` z koreňového adresára na disku a odovzdá ho `PHP interpretu`, čo je program, ktorý dokáže spracovať samotný kód PHP a na jeho základe vytvoriť kód `HTML`, ktorý sa odošle späť používateľovi.
5. Po skompilovaní kódu HTML sa odpoveď odošle späť používateľovi (nazýva sa `odpoveď`) a webový prehliadač stránku vykreslí bežným spôsobom, ako keby to bol čistý HTML.

> Všimnite si, že webový prehliadač sa nedozvie nič o obsahu skriptu PHP, ale spracuje iba vygenerovaný HTML, takže vaše skripty a obsah servera zostanú v bezpečí.

Vytvorme prvý skript
----------------------------

> Pri písaní prvého skriptu sa predpokladá, že máte v počítači spustený webový server. Pre Windows je najlepší <a href="https://www.apachefriends.org/index.html">XAMPP</a> (stiahnite si PHP vo verzii 7.0 alebo novšej) a <a href="https://www.apachefriends.org/index.html">XAMPP</a> funguje na Macu rovnako ako na Windows. Pre Linux odporúčam <a href="https://wiki.ubuntu.cz/servery/apache_s_mysql_a_php">LAMP server</a> (aj táto stránka beží na Lamp serveri).

Názov súboru skriptu PHP musí končiť príponou `.php`, aby webový server vedel, že ho chceme spracovať podľa pravidiel PHP. Vytvorme si teda príklad súboru `index.php`, ktorý bude obsahovať kód hlavnej stránky našej webovej stránky.

Otvorenie súboru
--------------------------

Otvorte tento súbor vo vhodnom textovom editore na písanie zdrojového kódu.

V systéme Windows je napríklad <a href="https://www.sublimetext.com">Sublime Text</a> dobrým začiatkom, pretože pekne vyfarbuje syntax (pravidlá jazyka) a uľahčuje čítanie kódu. Neskôr odporúčam zakúpiť <a href="https://www.jetbrains.com/phpstorm/">PhpStorm</a>, ktorý sa vo firmách často používa a ponúka možnosť programovať vo viacerých ľuďoch.

Napíšte základnú štruktúru stránky HTML
--------------------------

Základnú štruktúru stránky HTML už pravdepodobne poznáte:

<!DOCTYPE HTML>
<html>
    <head>
    <title>Můj první PHP script</title>
    <meta charset="UTF-8">
    </head>
    <body>

    </body>
</html>

Všetok kód HTML bude spracovaný bežným spôsobom a bude veľkou pomocou pri navrhovaní stránky. PHP do veľkej miery využíva princípy HTML a CSS.

Oddelenie skriptu PHP od kódu HTML
--------------------------

PHP je hlavne šablónovací jazyk, ktorý na vhodných miestach kódu generuje vlastný obsah. Aby sme mohli jasne rozlíšiť, čo je HTML a čo PHP, musíme použiť oddeľovaciu značku.

V súčasnosti je najvhodnejšie používať zápis pomocou `<?php` a `?>`.

```php
// tu je kód PHP
?>
```

> Terminátor `?>` používame, ak chceme použiť iný kód HTML. Ak sa na konci skriptu PHP nenachádza žiadny ďalší kód HTML, je vhodnejšie neuvádzať značku `?>`, aby sa na konci stránky nenachádzali zbytočné biele miesta a zlomy riadkov, ktoré môže vložiť textový editor.

V minulosti sa namiesto tagu `<?php` často používal tag `<?php`, ktorý však nemusí byť vždy podporovaný.

Obalové značky možno umiestniť kdekoľvek v kóde HTML, napríklad v tele stránky:

<!DOCTYPE HTML>
<html>
    <head>
    <title>Můj první PHP script</title>
    <meta charset="UTF-8">
    </head>
    <body>
    <?php
        // tady bude PHP kód
    ?>
    </body>
</html>

Základné konštrukcie
--------------------------

Medzi základné stavebné prvky patria:

- <a href="/echo">Uvádzanie obsahu v zdrojovom kóde</a>
- <a href="/premenná">Premenné</a>
- <a href="/if">Podmienky</a>

V tejto časti si ukážeme jednoduchý výpis obsahu do zdrojového kódu pomocou premennej.

Princíp konštrukcie zdrojového kódu
--------------------------------

Všetky konštrukcie (jazykové výrazy), príkazy a funkcie sú oddelené stredníkmi, aby bolo jednoznačné, odkiaľ a kam aktuálna konštrukcia platí.

Za bodkočiarkou zvyčajne nasleduje zalomenie riadku.

Symbolicky napísané:

```php
příkaz;
další příkaz;
proměnná x = její hodnota;

vypsat proměnnou x;

uložit do souboru;
```

Výpis do zdrojového kódu
--------------------------

Konštrukcia <a href="/echo">echo</a> sa používa na vypísanie obsahu.

Jeho používanie je veľmi jednoduché:

```php
echo 'Ahoj, svet!';
```

Potom vypíše text "Hello world!" do kódu HTML. Vyskúšajte vzorku.

> Všetky ostatné ukážky budú obsahovať iba vnútro kódu PHP. Okolitý kód HTML je voľne dostupný, môžete si ho vytvoriť podľa vlastného uváženia (napríklad použiť ukážku na začiatku tohto článku).

Premenné
--------------------------

Premenné sú virtuálne pamäťové miesta, ktoré uchovávajú údaje a používajú sa na ich presun. Názov premennej vždy začína znakom `dollar`, za ktorým nasleduje samotný názov `name` a potom jeho hodnota `value`.

> Podrobný popis fungovania premenných som zhrnul v <a href="/premenné">samostatnom článku o premenných</a>.

```php
$oblibeneCislo = 1024;
$jmenoAutora = 'Jan Barášek';

echo $oblibeneCislo;
echo '<br>';
echo $jmenoAutora;
```

> Názov premennej by mal vyjadrovať, čo premenná skutočne obsahuje, aby bol kód prehľadnejší. Všimnite si tiež vloženie značky HTML `<br>` na odsadenie textu. Tento tag by ste už mali poznať z jazyka HTML.

To, čo sa vypíše v konštrukcii `echo`, sa nazýva reťazec (postupnosť znakov). Jednotlivé reťazce možno spojiť bodkou (`.`), aby sa výstup zmenšil na jeden riadok:

```php
$oblibeneCislo = 1024;
$jmenoAutora = 'Jan Barášek';

echo $oblibeneCislo . '<br>' . $jmenoAutora;
```

> Po spojení reťazcov bodkou sa celok bude javiť ako jeden veľký reťazec.

Premenlivé operácie
--------------------------

Medzi premennými fungujú všetky základné <a href="/matematika">matematické operácie</a> úplne intuitívne, ako sa očakáva.

Definujme 2 premenné a vložme do nich čísla:

```php
$x = 5; // definuje premennú x s hodnotou 5
$y = 3; // definuje premennú y s hodnotou 3

echo $x + $y; // pridá premenné a vypíše 8
```

> Všimnite si, že znak rovnosti (`=`) sa nepoužíva na vykonávanie matematických operácií, takže nemôžete písať napríklad rovnice. PHP sa v tomto ohľade správa ako kalkulačka.

Ak nechceme používať premenné, môžeme operácie vykonávať priamo. Na umiestnení operácií teda nezáleží a vyhodnotia sa kdekoľvek.

```php
echo 5 + 3; // vytlačí 8
```

Prípadne môžeme premenné sčítať a výsledok uložiť do inej premennej:

```php
$x = 5;
$y = 3;
$z = $x + $y; // premenná $z obsahuje 8

echo $z; // vytlačí 8
```

V ďalšej časti sa naučíme úplné základy <a href="/principles-of-variables">definície a použitie premenných</a>.
