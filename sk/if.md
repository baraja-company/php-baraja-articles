Podmienky v PHP - IF() {...} - možnosti vetvenia
================================================

> id: '3e4b81bb-a8bb-4e99-8842-e76f1658a371'
> slug:
> 	cs: if
> 	sk: podmienky-v-php---if-...---moznosti-vetvenia
> 
> perex: 'Podmínky, logické výrazy, booleovská logika a možnosti větvení PHP scriptů. Vyhodnocování logických výrazů a operátory. Možnosti skládání výrazu.'
> publicationDate: '2019-11-26 11:55:09'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '759a244b4fe2dc119c475aee42259578'

> **Poznámka:** Tento článok môže byť pre niektorých začiatočníkov trochu neprehľadný, pretože predpokladá základné znalosti jazyka PHP. Ak vás zaujíma, ako podmienky fungujú, prečítajte si <a href="/podmienky">o podmienkach v kurze pre začiatočníkov</a>.

| Podpora: | Všetky verzie: PHP 4, PHP 5, PHP 7
|-------------------|----------|
| Stručný opis: | Overenie jedného alebo viacerých výrokov
| Typ: | <a href="/výroky-a-funkcie">Výrok, konštrukcia</a> (nie funkcia)

Popis
-----

Často potrebujete určiť, či platí rovnosť alebo či je výrok pravdivý, na to slúžia podmienky. PHP používa nasledujúcu syntax ako mnohé iné jazyky (najmä C):

```php
if (logický výrok) {
    // konštruovať
}
```

Každý logický výraz má hodnotu `TRUE` (pravda) alebo `FALSE` (nepravda), iné možnosti neexistujú.

Príklad porovnania, či je premenná `$x` väčšia ako premenná `$y`:

```php
$x = 10;
$y = 5;

if ($x > $y) {
    // Táto časť skriptu sa vykoná, ak je podmienka pravdivá
} else {
    // Táto časť skriptu sa spustí, ak sa podmienka neuplatní
}
```

Konštrukcia podmienky má povinný obsah v okrúhlych zátvorkách, v ktorom je uvedený výraz, ktorý sa má testovať, zložený z operátorov (prehľad nižšie), viaceré výrazy možno spájať pomocou logických operátorov (prehľad nižšie).

Okrem toho podmienka obsahuje dva voliteľné bloky príkazov.

- Ak táto podmienka platí
- Ak táto podmienka neplatí

Z praktických dôvodov vždy zahrňte aspoň prvý blok príkazov, keď je podmienka pravdivá, inak by testovanie výrazu nemalo zmysel.

Používanie stredníkov a zátvoriek
--------------------------

Vo všeobecnosti:
- **Krúhle** zátvorky sa používajú na oddelenie logických výrazov (na dosiahnutie zložitejších výrazov možno použiť iné okrúhle zátvorky).
- Zložená zátvorka** sa používa na ohraničenie bloku príkazov a funkcií.
- Znak **meddle** sa nepoužíva na označenie podmienky (blok príkazov je ohraničený zloženou zátvorkou), ale na oddelenie jednotlivých príkazov v rámci podmienky).

Jediný možný zápis so stredníkom (okrem použitia konštrukcie **endif**):

```php
if ($x > $y);
```

Takáto podmienka však nemá zmysel, pretože v oboch prípadoch sa výsledok porovnania zahodí a nevykoná sa žiadny príkaz patriaci do podmienky.

Alternatívny zápis
--------------------------

Za určitých okolností možno konštrukciu `if` použiť s vynechaním zložených zátvoriek. To sa dá dosiahnuť len v nasledujúcich prípadoch:

- Ak v podmienke vykonáme len jeden príkaz.
- Ak namiesto zložených zátvoriek použijeme dvojbodku a endif;
- Ak použijeme "in-line" zápis.

Podrobnejšie informácie nájdete v nasledujúcich 3 kapitolách.

> **`1. Len jeden príkaz ~ skrátená syntax`**

Ak vytvárate podmienku, v ktorej chcete vykonať len jednu konštrukciu (príkaz), môžete použiť buď klasický zápis v zložených zátvorkách:

```php
if ($x > 10) { $y = $x; }
```

Alebo môžeme zátvorky vynechať:

```php
if ($x > 10) $y = $x;
```

Toto správanie sa však vzťahuje len na jeden príkaz v bezprostrednej blízkosti podmienky.

Lepší príklad (podmienečne sa vykoná len konštrukcia `$y = $x`, ostatné sa vykonajú vždy):

```php
$x = 5;
$y = 3;
$z = 10;
if ($x > $y)
$y = $x;
$x = 3;
```

> **`2. Dvojbodka a endif;`**

```php
if (výraz):
    konstrukt;
    konstrukt;
    konstrukt;
endif;
```

Tento zápis sa však už dlho považuje za zastaraný, pretože sa znižuje jeho orientácia, keď sa do seba ponorí viacero podmienok.

> **Poznámka:** Rád by som poznamenal, že tento štýl sa páči aj niektorým ľuďom, napríklad Yuhovi (<a href="https://www.jakpsatweb.cz/php/php-tahak.html#vetveni">viď jeho článok</a>). Chráň Boh, aby ste to niekde použili.

> **`3. Ternárny výraz ~ jednoriadkový "in-line" zápis`**

Občas je užitočné vykonať jednoduché in-line porovnanie s nejakou inou akciou (napríklad spolu s definovaním novej premennej). Ak chceme vykonať len jeden príkaz, celý postup môžeme zredukovať na jeden riadok aj pri zachovaní čo najväčšej jednoduchosti.

```php
$x = 5;
$xJeVetsiNezDva = ($x > 2 ? TRUE : FALSE);

// alebo ešte kratšie:

$xJeVetsiNezDva = ($x > 2);
```

Tabuľky operátorov
-----------------

V rámci podmienky sa používajú dva typy operátorov:

- **Komparatívne** ~ porovnávajú konkrétny vzťah,
- **Logické** ~ kombinujte viacero výrazov na vytvorenie komplexných podmienok.

Porovnávacie subjekty
-----------------------

| Operátor | Význam |
|----------|----------|
| `==` | Rovná sa
| `===` | Rovná sa a má rovnaký dátový typ
| `!=` | Nerovná sa
| `>=` | Rovná sa alebo je väčšia
| `<=` | Rovná sa alebo je menšia
| `>` | Väčší
| `<` | Menej

Príklad (platí, keď `$x nie je 5`):

```php
if ($x != 5) { ... }
```

Logické operátory
--------------------

| Operátor | Alternatíva | Význam | Pravda, keď:
|-----------|--------------|----------------|--------------------------------------------
| `&&` | AND | a zároveň | obe hodnoty sú pravdivé
| `||` | OR | alebo | aspoň jedna hodnota je pravdivá
| `^^` | XOR | exkluzívne OR | aspoň jedno je pravda alebo nepravda, ale nikdy nie oboje
| `!` | *doesn't* | negácia výrazu | `true` keď `false` a naopak
| `()` | *neguje* | negácia výrazu| závisí od okolností

Zložitejší príklad:

```php
$x = 5;
$y = 3;
$z = 8;
if ($x > 0 && !($y != 2 && $z == $x) || $z > $y) { ... }
```

Vynechanie logických a porovnávacích operátorov
---------------------------------------------

Často si môžeme dovoliť vynechať niektorý z operátorov (alebo dokonca oba), ale nikdy nesmieme zabudnúť na pravidlá správneho používania, aby výsledný výraz fungoval.

Vo všeobecnosti pri testovaní výrazu bez operátora testujeme, či je jeho hodnota `TRUE` alebo či nie je prázdna (napríklad obsahuje nenulové číslo, neprázdny reťazec, ...).

Príklady:

```php
$x = 5;
$y = 3;
$z = 8;

if ($x) { ... }         // prechádza, pretože $x nie je prázdny
if ($x && $y) { ... }   // prechádza, pretože $x a $y nie sú prázdne
if (!$x) { ... }        // zlyhá, pretože TRUE je negované
if (isset($z)) { ... }  // prechádza, pretože premenná $z existuje
```

Môžu však nastať zložité situácie, najmä keď:
- Pýtam sa na `if ($x)` a premenná `$x` obsahuje nulu (`0`), potom podmienka nie je splnená.
- Alebo premenná `$x` obsahuje reťazec ``0`` (číslo nula), pretože sa prelieva do nuly, a preto výraz nie je pravdivý.
- V podmienke je funkcia, ktorá vždy vracia nejaký neprázdny reťazec, pretože potom je podmienka vždy pravdivá.
- Alebo ak kontrolujeme vstup od používateľa a ten vráti `'false'` ako reťazec, potom je podmienka opäť pravdivá, pretože reťazec nie je prázdny.

Odporúčam jedno jednoduché a účinné riešenie - opýtajte sa na počet vrátených znakov. Ak je reťazec prázdny (alebo premenná neexistuje), potom sa vráti nula znakov a podmienka nie je splnená. Jednoduchý príklad:

```php
$x = '0';
if ($x) { ... }			// podmienka sa zvyčajne neuplatňuje
if (strlen($x)) { ... }	// podmienka je platná, pretože $x obsahuje 1 znak
```

Ďalej môžeme testovať existenciu premennej pomocou funkcie `isset()`.

Porovnanie reťazcov
-----------------

Zistiť, že struny sú identické, je jednoduché:

```php
$a = "Mačka;
$b = 'cat';

if ($a === $b) {
    // Ak sú reťazce rovnaké
} else {
    // Ak sa reťazce líšia
}
```

Je dôležité riadne sledovať typy údajov pre prípad, že by položka mohla byť ekvivalentná inej položke.

Napríklad prázdny reťazec `$a = '';` sa líši od reťazca `NULL`: `$b = NULL;`. Toto rozlíšenie potrebujeme napríklad kvôli databázam, kde je rozdiel medzi tým, či hodnota neexistuje alebo je prázdna.

```php
$a = '';
$b = null;

if ($a == $b) {
    // Vyhodnotí sa ako TRUE, pretože
    // dátový typ sa konvertuje.
}

if ($a === $b) {
    // Vykonáva oveľa prísnejšiu validáciu
    // a to neprejde, pretože je to iný
    // obsah a iný typ údajov, preto
    // tento kód sa nikdy nespustí.
}
```

Pri porovnávaní reťazcov je tiež dobré ignorovať biele (neviditeľné) znaky, ako sú medzery, tabulátory a zalomenia riadkov. To je užitočné napríklad pri zadávaní hesla a jeho odovzdávaní hashovacej funkcii:

```php
$password = '81dc9bdb52d04dc20036dbd8313ed055'; // 1234
$userPassword = ' 1234 ';

if (md5(trim($userPassword)) === $password) {
    // Funkcia trim() automaticky odstraňuje medzery.
}
```

Neznáma (neexistujúca) hodnota?
--------------------------

Niekedy sa môže stať, že hodnota neexistuje (nie je ani `TRUE` ani `FALSE`), ide hlavne o hodnotu získanú z databázy (napríklad sa pýtame na stĺpec, ktorý neexistuje), v takom prípade sa vráti dátový typ `NULL`.

Vo všeobecnosti sa `NULL` vyhodnotí ako `FALSE`, t. j. podmienka neplatí. Toto správanie však nie je vždy vhodné, pretože neexistujúca hodnota nemusí nevyhnutne znamenať, že neexistuje žiadny záznam.

> Príklad z praxe: Máme profil používateľa a dopytujeme sa na jeho webovú stránku. Nie všetci používatelia musia mať webovú stránku, takže v tomto prípade sa vráti `NULL`, ale používateľ stále existuje. V tomto prípade by sme teda mali radšej použiť funkciu `isset()` na testovanie (ne)existencie premennej a nerobiť závery na základe konkrétnej hodnoty.
