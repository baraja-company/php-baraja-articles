Premenné v jazyku PHP
=====================

> id: b89774e7-143c-4a8c-8dc6-3b3d2c78d5b7
> slug:
> 	cs: promenna
> 	sk: premenne-v-jazyku-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '23e4d537f18a3b2e1946c79be1aacf52'

Táto stránka je úplným zhrnutím fungovania premenných v jazyku PHP. Text je písaný mierne odborným štýlom a začiatočníci mu nemusia úplne porozumieť. Ak vás zaujímajú úplné základy, prečítajte si <a href="/prvý-skript">príručku pre začiatočníkov</a> a <a href="/principy-premena-skriptu">principy písania premenných</a>.

Popis
-----

Premenná je virtuálne miesto v operačnej pamäti, je definovaná pomocou `name` a `data type`. V rámci dátového typu má potom premenná určitý `obsah`.

PHP interne reprezentuje premenné ako takzvanú hashovaciu tabuľku, t. j. všetky premenné sú uložené v tabuľke, v ktorej sa dá veľmi rýchlo vyhľadávať, takže čas potrebný na prístup ku každej premennej je *takmer* konštantný.

Príklady písania:

```php
$a = 10;
$b = 'cat';
$c = true;
```

Každý riadok vo vzorke označuje definíciu jednej premennej. Názov každej premennej začína znakom dolára `$`, za ktorým nasleduje samotný názov. Znak rovnosti možno použiť na priradenie hodnoty premennej.

Interne sa premenné uložia do pamäte ako hašovacia tabuľka:

| Názov | Typ | Skratka | Hodnota |
|-------|---------|---------|---------|
| `$a` | integer | int | `10` |
| `$b` | string | string | `cats` |
| `$c` | boolean | bool | `true` |

Typy premenných
---------------

Premenné sú klasifikované podľa prístupových práv a použitia:

- <a href="/local-variable">lokálne premenné</a> (platné v kontexte, t. j. v rámci funkcií a metód),
- <a href="/global-variable">globálne premenné</a> (platné v rámci celého skriptu),
- <a href="/superglobal-variable">Superglobálne premenné</a> (platné v rámci celého prostredia servera - typicky údaje používateľa),
- <a href="/promenna-premenna">premenná premennej</a> (špeciálna premenná, ktorá obsahuje odkaz (link) na inú premennú).

* `Globálne premenné` a `premenné` by sa nemali používať, pretože prispievajú k nečitateľnosti kódu a "magickému" (neočakávanému) správaniu aplikácie.*

Povolený obsah premenných
--------------------------

Premenné môžu obsahovať čokoľvek, čo umožňuje ich aktuálny dátový typ. Ak typ údajov nie je zadaný, určí sa automaticky na základe obsahu (to sa neodporúča, pretože to môže viesť k chybám).

Dátové typy pracujú nezávisle, takže môžeme použiť takmer akýkoľvek typ. Ak však chceme vykonať nejakú operáciu zlúčenia, musíme vždy zabezpečiť prevod do jedného formátu.

Dobrým príkladom je napríklad sčítanie a násobenie čísel:

```php
$x = 5;       // celé číslo
$y = 3;       // celé číslo
$z = $y + $y; // premenná $z bude zložená na základe viacerých premenných
```

V tomto prípade PHP stojí pred otázkou, aký dátový typ bude mať novo vytvorená premenná `$z`. Ak ide o rovnaký dátový typ a operácia je možná, dátový typ sa dedí.

Niekedy však môžeme vykonať operáciu s viacerými dátovými typmi:

```php
$x = 1;       // celé číslo
$y = 3.14159; // float
$z = $y + $y; // float
```

V tomto prípade zlúčime celé číslo a float. Výstupom bude desatinné číslo, preto sa použije float. V tomto prípade PHP vykoná niečo, čo sa nazýva **dynamické rozdelenie**.

Na toto správanie sa však nemôžeme vždy spoliehať. Ako by ste napríklad chceli zlúčiť číslo a reťazec?

```php
$x = 256;     // celé číslo
$y = "Ahoj!; // float
$z = $y + $y; // ???
```

Dátové typy (prehľad najdôležitejších typov)
--------------------------------------

PHP je interpretovaný jazyk, takže má v porovnaní s inými programovacími jazykmi niektoré zvláštnosti, jednou z nich je, že nemusíme špecifikovať dátové typy premenných, t. j. každá premenná dynamicky mení svoj dátový typ podľa svojho obsahu (ak nie je uvedené inak). Napriek tomu je dobré poznať aspoň základné dátové typy, čo je užitočné najmä pri optimalizácii aplikácií alebo pri práci s databázami.

Zápis môže vyzerať takto:

```php
$x = (int) 25; // vytvorí premennú typu integer
```

<a href="/datove-typy">Prehľad typov údajov</a>.

Dedičnosť dátových typov
-----------------------

Aký dátový typ bude mať premenná `$x`, ak poznáme len tento kus zdrojového kódu?

```php
$x = $y;
```

Závisí to od dátového typu premennej `$y`, od ktorej sa zdedí hodnota aj jej dátový typ. V tomto prípade nepoznáme premennú `$x`, takže nemôžeme pokračovať vo vyhodnocovaní kódu a vyhodí sa chybové hlásenie.

Dynamické prepísanie
---------------------

Majme nasledujúce 2 premenné:

```php
$x = 10;
$y = '10';
```

Aký je rozdiel medzi obsahom premenných `$x` a `$y`?

Premenná `$x` je číslo, `$y` je reťazec (obsahuje "1" a "0"), takže ak premennú len uložíme do pamäte a nevykonáme žiadnu operáciu, ktorá by ovplyvnila jej hodnotu. Napríklad nasledujúce 2 položky vrátia rovnaký výsledok:

```php
echo $x + 5;	// vytlačí 15
echo $y + 5;	// vytlačí 15
```

V druhom prípade dochádza k takzvanému **dynamickému prepísaniu**, t. j. premenná konvertuje svoj dátový typ tak, aby sa s ňou mohla vykonať výpočtová operácia. Na toto správanie sa nedá vždy spoľahnúť a ide skôr o opravné správanie, ktoré slúži na opravu zle napísaných začiatočníckych skriptov. Ak je to možné, vždy zapisujte čísla s dátovým typom na ukladanie čísel, pretože to zvyšuje ich presnosť a uľahčuje ich použitie v budúcnosti.

> **Poznámka:** Je dôležité poznamenať, že dátové typy nemôžeme konvertovať úplne ľubovoľne, takže to nie je vždy možné. Ak prepíšete dátový typ na iný (nekompatibilný), buď sa konverzia vôbec nemusí uskutočniť, alebo sa pôvodný obsah môže poškodiť alebo úplne zničiť a nahradiť iným. Ak napríklad prepíšete reťazec na celé číslo (a do premennej uložíte nejaký text, ktorý nie je číslom), namiesto číselnej hodnoty sa do premennej uloží hodnota `1`.

Reprezentácia reťazcov ako polí
------------------------------

Všetky reťazce sú interne uložené ako pole znakov. To znamená, že každý znak má svoj vlastný **index** a možno sa naň odkazovať. Ak nezadáme index, pracuje sa s celým reťazcom.

```php
$x = "Programujme v PHP!;
$n = 3;

echo $x;		// vypíše sa celý obsah premennej $x
echo $x[0];		// vypíše nulový znak premennej $x
echo $x[$n];	// vypíše n-tý znak premennej $x
```

> **Poznámka:** Čísla PHP od nuly, t. j. nulový znak je 'P' a prvý znak je 'r'.
>
> > Okrem toho sa znaky prepínajú po bajtoch. Napríklad znak "no" v kódovaní UTF-8 je dlhý 2 bajty, takže index znaku v reťazci nebude pri posúvaní zodpovedať skutočnej pozícii a na uloženie znaku sa použijú 2 indexy.

Existencia prvku poľa by sa mala vždy overiť pomocou funkcie `isset()`:

```php
if (isset($x[$n])) {
    echo $x[$n];
}
```

Prípadne ho môžete pekne zapísať pomocou ternárneho operátora:

```php
echo $x[$n] ?? '';
```

Kopírovanie premenných
---------------------

Majme nasledujúcu premennú:

```php
$q = "Lorem ipsum, ...;
```

A potom skopírujte jej hodnotu do ďalšej premennej:

```php
$qi = $q;
```

Našťastie nedôjde k žiadnemu kopírovaniu a PHP len uloží odkaz na hodnotu do tabuľky **hash**. Hodnota sa skutočne skopíruje len vtedy, keď sa má zmeniť hodnota jednej z premenných. O toto správanie sa stará komponent, ktorý sa všeobecne nazýva **Garbridge collector**.
