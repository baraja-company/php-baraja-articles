Funkcia PHP date(), dátum a čas
===============================

> id: '9b0ec1c7-3431-4d7d-9bcc-6093285f14f1'
> slug:
> 	cs: date
> 	sk: funkcia-php-date-datum-a-cas
> 
> perex: 'Zjištění data a času, aktuální datum, formátování data a času a převod tvaru.'
> publicationDate: '2019-09-11 10:14:16'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: d0323ba88fcdba84ef45439991301f6c

Funkcia `date()` je nástroj na prácu s dátumom a časom. Používa sa v dvoch prípadoch:

- **Zistenie aktuálneho stavu**, t. j. aktuálneho dátumu, času, ... a jeho výstup v určitom formáte,
- **Konverzia** konkrétneho dátumu do iného formátu (napríklad číslo mesiaca na meno, formát zápisu roka, 12- a 24-hodinový systém, ...).

Vzorka
------

Já jsem speciální stránka. Vím, že právě je
<?php
    echo date('H:i'); // Hodina:minuta
?>

> VAROVANIE: PHP nevypisuje váš čas, ale čas na serveri. Preto sa môže stať, že sa zobrazí iný čas, ako je nastavený v počítači.

Vstupná syntax
--------------------------

Funkcia sa volá bežným spôsobom a jednotlivé požiadavky sa zadávajú ako argumenty funkcie.

```php
echo date('formátovacie značky', atribut konkrétního času);
```

Formátovacie značky označujú, v akom formáte sa dátum vytlačí. Značky môžu obsahovať medzery, bodky, dvojbodky, pomlčky, spojovníky a iné znaky, ktoré samy o sebe nie sú formátovacími značkami (ak chcete použiť formátovaciu značku, musíte ju <a href="/carriage-notes">vypísať</a>). Prehľad jednotlivých značiek je uvedený nižšie.

Druhý (nepovinný) atribút označuje ručne zadaný dátum alebo čas, ktorý sa prevedie a vypíše vo formáte podľa prvého parametra. Musí byť zadaná ako *časová pečiatka* (možno ju získať pomocou formátovacej značky "U").

Príklad:

```php
echo date("d. m. Y', 1405856605); // do 20. 07. 2014
```

Tabuľka povolených formátovacích značiek
--------------------------

| Znak | Popis
|------|---------------------
| `Y` | Rok ako štyri číslice (napr. 1998)
| `y` | Rok ako dvojmiestne číslo (napr. 98)
| `M` | anglická skratka názvu mesiaca (napr. Jan)
| `m` | Číslo mesiaca (01-12)
| `F` | Názov mesiaca v angličtine (napr. January)
| `D` | anglická skratka dňa v týždni (napr. Fri)
| `l` | Anglický názov dňa v týždni (napr. Friday)
| `N` | Číslo dňa v týždni (1 - pondelok, 7 - nedeľa)
| `w` | Číslo dňa v týždni (0 - nedeľa, 1 - pondelok, 6 - sobota)
| `d` | Deň v mesiaci (01-31)
| `j` | Číslo dňa v mesiaci (1-31)
| `z` | Deň v roku (001-365)
| `H` | Hodina (00-23)
| `h` | Hodina (01-12)
| `i` | Minúta (00-59)
| `s` | Second (00-59)
| `U` | *Timestamp:* Počet sekúnd od začiatku času (od 1. januára 1970)
| `S` | Anglická koncovka poradového čísla dňa v mesiaci
| `A` | Indikátor AM/PM
| `a` | Indikátor ráno/odpoludnie (am/pm)
| `P` | Rozdiel oproti greenwichskému času (GMT) s oddeľovačom medzi hodinami a minútami (pridané v PHP 5.1.3), napríklad: `+02:00`
| `g` | Hodiny v 12-hodinovom formáte (1-12)
| `G` | Hodiny v 24-hodinovom formáte (0-23)

Formátovanie času pre mapu stránky
---------------------------------

Veľmi často potrebujete formátovať čas pre súbor `sitemap.xml`, ktorý obsahuje dátum a čas poslednej zmeny v tagu `<lastmod>`.

Od verzie PHP 5.1.3 možno na tento účel použiť nasledujúcu syntax:

```php
date("Y-m-d\TH:i:sP);
```

Dátum a čas sa zobrazí s presnosťou na sekundy a bude obsahovať aj informácie o časovom pásme, v ktorom sa server nachádza (časové pásmo určuje operačný systém servera).

Získanie českých názvov dní a mesiacov
----------------------------------

V PHP nie je možné bežným spôsobom získať české názvy dní a mesiacov, preto si tieto hodnoty musíme napísať sami. Najlepší spôsob je uložiť položky do poľa a načítať ich pomocou volania indexu.

```php
$mesice = [
    1 => "január, "február, "Pochod, "apríl, "May,
    "June, "júl, "August, 'September', "október,
    "November, "december
];

$dny = ["nedeľa, "pondelok, "Utorok, "Streda, "štvrtok, "Piatok, "Sobota];
```

Tento príklad je zjednodušený príklad od <a href="https://php.vrana.cz">**Jakuba Vránu**</a> z článku <a href="https://php.vrana.cz/ceske-nazvy-mesicu-a-dnu-v-tydnu.php">České názvy mesiacov a dní v týždni</a>.

Preklad dátumov z angličtiny do slovenčiny
--------------------------------------

Často máme dátum vo formáte `Friday, 13 September` a chceme ho preložiť do slovenčiny, ale ako to urobiť? Najlepšie je zavolať nejakú funkciu, napríklad `datumcesky()`, ktorej odovzdáme anglický dátum a ona ho preloží.

```php
function datumCesky(string $date): string
{
    $men = [
        "január, "február, "March, "apríl, "May,
        "June, "júl, "August, 'September', "október,
        "November, "december
    ];

    $mcz = [
        "január, "február, "Pochod, "apríl, 'may',
        "June, "júl, "August, 'September', "október,
        "November, "december
    ];

    $date = str_replace($men, $mcz, $date);

    $den = [
        "pondelok, "Utorok, "Streda, "štvrtok,
        "Piatok, "Sobota, "nedeľa
    ];

    $dcz = [
        "pondelok, "Utorok, "Streda, "štvrtok,
        "Piatok, "Sobota, "nedeľa
    ];

    return str_replace($den, $dcz, $date);
}
```

Príklad použitia:

```php
echo datumCesky("Piatok, 13. septembra); // piatok, 13. decembra
```

Táto funkcia určite nie je ideálna na preklad, pretože len nahrádza anglické slová českými, ale pre mnohé nasadenia môže postačovať. Pri pokročilejších prekladoch by ste mali vždy zaručiť presnú syntax prekladu do jednotného štýlu, napr. `Piatok, 13. decembra`.

Vyhľadanie prvého dňa v mesiaci
-----------------------------

Prvý deň v apríli 2018 bola nedeľa, ale ako to ľahko zistiť?

Od verzie PHP 5.1.0 existuje jednoduché riešenie:

```php
echo date('N', strtotime('2018-04-01')); // 1 (pondelok), 7 (nedeľa)
```

V starších verziách musíme túto funkciu implementovať sami:

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

Keďže modifikátor `w` vracia výstup v americkom formáte, stále upravujem číslo dňa jednoduchým výpočtom. Funkcia vráti celé číslo v rozsahu od 1 (pondelok) do 7 (nedeľa).

Prevod časových posunov / formátovania a overovanie dátumu
--------------------------------------------------

Často potrebujeme preskočiť relatívny čas (napríklad +5 dní) alebo vytiahnuť dátum z textového vstupu používateľa, aby bol platný. Na to slúži funkcia <a href="https://www.php.net/manual/en/function.strtotime.php">strtotime()</a>, ktorá umožňuje nasledujúcu syntax:

```php
echo strtotime("teraz);
echo strtotime("10. septembra 2000);
echo strtotime("+1 deň);
echo strtotime("+1 týždeň);
echo strtotime("+1 týždeň 2 dni 4 hodiny 2 sekundy);
echo strtotime("budúci štvrtok);
echo strtotime("minulý pondelok);
```

Výstupom funkcie je **časová pečiatka** *(univerzálny čas)* zadaného času ako celé číslo.

> Vo všeobecnosti odporúčam nasadiť túto funkciu na všetky formuláre, kde nejakým spôsobom pracujeme s časom používateľa. Určite nie je dobré nútiť používateľa písať dátum v nejakom konkrétnom formáte, ale vždy takýto formát automaticky vytvoriť, aby si používateľ mohol napísať, čo chce. Často sa totiž stáva, že text je napríklad odniekiaľ skopírovaný a je príliš pracné ho ručne preformátovať, keď sa to dá urobiť automaticky.

Počet sekúnd od začiatku času
--------------------------

Od 1. januára 1970 sa každá sekunda pripočíta k jednej a vznikne obrovské celé číslo, ktoré udáva čas, ktorý odvtedy uplynul. To je užitočné najmä na jednoduchý výpočet časových rozdielov. Je to pomerne spoľahlivé riešenie, ale má svoje riziká (problém 2038).

**Všeobecné vlastnosti tohto zápisu:**
- Je to celé číslo, takže sa s ním ľahko pracuje,
- Je v desiatkovej sústave, takže sa ľuďom ľahko číta (okrem toho, že nemôžete rýchlo zistiť, aký je presný čas),
- Nemožno ho použiť na reprezentáciu obdobia pred 1. januárom 1970, pretože nemôže byť záporný *(niektoré nové verzie algoritmu implementujú záporné časy, ale nemožno sa na to vždy spoliehať)*,
- V roku 2038 prestane fungovať na 32-bitových počítačoch a spôsobí pád programu (kvôli pretečeniu zásobníka a maximálnej veľkosti celého čísla).

Problém roku 2038
--------------------------

<h2 id="universalTime" style="background: #222; margin-top: 32px; padding: 32px; color: white; text-align: center; border-radius: 3px;">Nečítam...</h1>

<script>
	setInterval(function() {
		window.document.getElementById('universalTime').innerHTML = Math.floor(Date.now() / 1000);
	}, 250);
</script>

> Nad týmto textom sa zobrazuje aktuálna hodnota času, ktorá sa každú sekundu zvyšuje o +1. Aktuálna hodnota sa načíta pomocou javascriptu, takže nemusí byť vždy presná (udáva systémový čas vášho počítača).

Počítače ukladajú celé čísla v binárnom tvare, a ak ide o celé číslo, často má 32 bitov (32 jednotiek a núl). Najvyššie možné 32-bitové číslo je: `011 111 111 111 111 111 111 111 111 11`.

Ak však k tejto konštante každú sekundu pripočítame +1, jedného dňa dostaneme takéto číslo *(bude to 19. januára 2038 o 03:14:07)*. Keď sa však pokúsime pridať ďalšiu 1, rozsah bitov už nebude stačiť (číslo by bolo väčšie, ako sa dá uložiť do 32 bitov), takže dôjde k **preplneniu zásobníka**, t. j. na začiatok čísla sa pridá 1, čo v binárnom tvare znamená **záporné číslo**, (takže to pravdepodobne spôsobí pád programu), čím sa dostaneme niekam do roku 1901 (uff!).

Tento problém má dve riešenia:

- Rozšírenie rozsahu celých čísel z 32 bitov na 64 bitov, čo nám dáva rozpätie niekoľkých tisíc rokov
- Použite dátový typ `\DateTime` (bežne dostupný v PHP), ktorý poskytuje objektovo orientovaný prístup k správe dátumu, je dobre kompatibilný s databázou a ponúka rozsah rokov od `0001` do `9999`, čo je dostatočné pre potreby väčšiny reálnych aplikácií.

Osobne som zástancom používania dátového typu `\DateTime` a nepoužívania celočíselného ukladania.

Rôzne časové pásma
-----------------

PHP je inteligentné, takže sa vždy snaží použiť aktuálne najlepšie časové pásmo, v ktorom sa nachádza server. Niekedy sa však môže stať, že časové pásmo je zadané nesprávne alebo programujeme globálnu aplikáciu, ku ktorej pristupujú používatelia z celého sveta, a preto musíme časové pásmo zmeniť ručne.

Toto je možné vykonať globálne pre celý skript PHP zavolaním funkcie:

```php
date_default_timezone_set('UTC');
```

Občas sa môžete stretnúť aj s týmto riešením (zistenie, že server má iné časové pásmo ako UTC):

```php
$defaultTimeZone = 'UTC';

if (date_default_timezone_get() != $defaultTimeZone) {
    date_default_timezone_set($defaultTimeZone);
}
```

Zmena dátumu a času
--------------------------

Za zistenie aktuálneho dátumu a času nie je zodpovedné samotné PHP, ale operačný systém, v ktorom je spustené. Ak teda potrebujete manuálne zmeniť čas, zmeňte ho tam.
