Príkazy, kľúčové slová a funkcie v jazyku PHP
=============================================

> id: '96a00928-4410-479d-ada0-612de21cdacd'
> slug:
> 	cs: prikazy-a-funkce
> 	sk: prikazy-klucove-slova-a-funkcie-v-jazyku-php
> 
> publicationDate: '2019-09-11 10:18:03'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '4534cc03d2ae66454b9f398139f232a4'

Každý skript PHP sa skladá z príkazov a funkcií, ktoré sa spoločne nazývajú **konštrukcie**. Pri spracovaní skriptu sa tieto jazykové výrazy sledujú (tento proces sa nazýva **tokenizácia**) a na základe *kľúčových slov* sa interpret rozhoduje, ako sa má procesor správať.

Príkazy
--------------------------

Príkazy (<a href="https://www.php.net/manual/en/reserved.keywords.php">v angličtine sa nazývajú **keywords**</a>) sú už naprogramované časti jazyka, sú vyhradené špeciálne pre svoj účel, dajú sa použiť okamžite v akejkoľvek situácii (nevyžadujú ďalšie špeciálne knižnice ani inštaláciu) a tvoria základ všetkých skriptov.

Napríklad <a href="/echo">extrahovanie reťazca do kódu HTML</a>:

```php
echo 'Echo je jazykový príkaz na výpis obsahu';
```

Pri príkazoch je dôležité poznamenať, že nevracajú žiadnu hodnotu, a preto ich nemožno použiť ako premennú. Príkazy sa dajú identifikovať aj podľa toho, že neobsahujú zátvorky.

Príklady:

```php
echo "Ahoj, svet!;
echo ("Ahoj, svet!);
```

Ak obsah príkazu uzavriem do zátvoriek, správanie sa nezmení a bude sa považovať za **výraz**. Je to rovnaké, ako keby sme písali matematiku:

```php
5 + 3

// alebo:

(5 + 3)
```

Technicky je v tom rozdiel, ale v praxi sa nič nemení.

Funkcie
--------------------------

Ak by existovali len príkazy, bolo by to dosť nudné. Funkcie sú súborom viacerých príkazov.

Často chceme opakovane vykonávať ten istý kód na viacerých miestach. V takom prípade by bolo neustále kopírovanie príliš pracné a mohlo by viesť k chybám, preto takýto kód zabalíme do funkcie, ktorú pomenujeme, a potom ju len zavoláme podľa názvu.

Keď zavoláme funkciu, odovzdáme jej parametre ako premenné, ona vykoná kód a vráti výsledok, ktorý môžeme ďalej použiť.

```php
$text = "PHP je môj obľúbený jazyk!;

echo 'Text' . $text . '" je dlhá . strlen($text) . ' znaky.;
```

PHP má mnoho funkcií priamo preddefinovaných (<a href="/documentation">pre úplný zoznam pozri dokumentáciu</a>), ale často je vhodné definovať si vlastné:

```php
function mojeFunkce(int $x, int $y): int
{
   $z = $x + $y;   // pridá vstupné parametre a uloží ich do premennej

   return $z;      // vráti premennú $z
}

echo mojeFunkce(5, 3); // vypíše číslo 8, pretože čísla boli spracované funkciou
```

Premenné vo funkciách
--------------------------

Lokálne premenné sa používajú v rámci funkcií, čo znamená, že ich možno použiť len vo vnútri funkcie a nemožno s nimi manipulovať (ani ich definovať) mimo funkcie. Svoje počiatočné hodnoty získavajú z parametrov funkcie priamo v definícii funkcie.

Príklad:

```php
$z = 5;

function prvniFunkce(int $x, int $y): int
{
   return $x - $y; // toto by vrátilo rozdiel v číslach
}

function druhaFunkce(): mixed
{
   return $z; // toto vráti chybu, pretože premenná $z
              // nie je definovaný vo vnútri funkcie
}
```

Niekedy je užitočné nastaviť niektoré parametre ako voliteľné, čo sa vykoná definovaním alternatívnej (predvolenej) hodnoty:

```php
echo prictiCislo(5);    // vráti 6
echo prictiCislo(5, 7); // vráti 12

function prictiCislo(int $x, int $y = 1): int
{
   return $x + $y;
}
```

Funkcia `prictiCislo()` pridá hodnotu z premennej `$y` do premennej `$x`. Ak premenná `$y` nie je definovaná (zadaná ako parameter pri volaní funkcie), použije sa jej predvolená hodnota zapísaná v definícii funkcie. Druhý parameter funkcie (premenná `$y`) je preto nepovinný.

> Každá funkcia môže mať len jeden výstup (jeden návrat), ak vo funkcii zadáte viac výstupov, vráti sa ten, ktorý bol zadaný ako prvý. Ak chcete vrátiť viacero hodnôt, musíte použiť pole.
>
> V PHP (na rozdiel od iných jazykov) nemusíte vôbec zadávať návrat, v takom prípade funkcia nevráti nič (void) na žiadnom vstupe. Takéto funkcie sa väčšinou používajú na ukladanie údajov alebo výstup do zdrojového kódu. Vo všeobecnosti sa však odporúča vždy vrátiť nejakú hodnotu, aspoň potvrdenie, že všetko prebehlo v poriadku.
