Dátové typy v PHP
=================

> id: cd63818c-259a-484e-b006-86c35b362019
> slug:
> 	cs: datove-typy
> 	sk: datove-typy-v-php
> 
> perex: 'Datové typy v PHP: String, integer, float, boolean, pole (array), objekt a další.'
> publicationDate: '2019-08-23 15:44:28'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: fcb2c647973c5f94e601c789b0a882f1

Všetky údaje spracúvané v jazyku PHP sú určitého typu. Napríklad celé číslo, reťazec alebo logické číslo (true/false).

Základné typy údajov
--------------------

Základné typy sa nazývajú aj **primitívne dátové typy** alebo <a href="/function-is-scalar">skalárne typy</a>.

| Typ | Názov | Popis |
|---------|-----------------|-------|
| `int` | Integer | (`integer`) Obsahuje iba celé číslo. Maximálna hodnota je určená počtom bitov. V 32-bitovom PHP je rozsah od `-2 147 483 648` do `-2 147 483 647` (~ ± 2 miliardy), v 64-bitovom PHP je rozsah od `-9 223 372 036 854 775 808` do `-9 223 372 036 854 775 807` (~ ± 9 kvintiliónov). Maximálnu hodnotu možno vždy získať volaním konštanty `PHP_INT_MAX`. Ak je prekročená maximálna hodnota celého čísla, PHP bude číslo reprezentovať ako `float` a automaticky ho prepíše.
| `float` | Desatinné číslo s pohyblivou desatinnou čiarkou | Ide o variant čísla s pohyblivou desatinnou čiarkou, pre ktorý platí pravidlo *"čím menšie, tým presnejšie "*. Číslo je interne uložené ako takzvaná **mantisa** a **exponent**, takže vlastne ukladáte 2 čísla, medzi ktorými sa vykoná operácia `mantisa * (2^exponent)`, čo umožňuje uložiť naozaj veľký rozsah čísel. Využíva princíp, že pri veľkých číslach nepotrebujeme vždy poznať ich presnú hodnotu, ale chceme ušetriť čo najviac pamäte. Čísla typu `float` nemusia byť uložené presne a nemali by sa používať na výpočet peňazí.
| `string` | String | Obsahuje postupnosť znakov, ktoré sú ohraničené úvodzovkami alebo apostrofmi. Maximálna dĺžka je obmedzená len kapacitou operačnej pamäte. Reťazec môže byť uložený v ľubovoľnom kódovaní, môže obsahovať emoji alebo binárne údaje.
| `bool` | Boolean | (`boolean`) Logická hodnota z logickej algebry, môže obsahovať len `true` alebo `false`.
| `null` | Prázdna hodnota | Prázdna hodnota `null` je užitočná v prípadoch, keď chceme vyjadriť, že niečo neexistuje. Napríklad článok nemá žiadnu kategóriu. Niekedy sa `null` nesprávne nahrádza nulovým (`0`) alebo prázdnym reťazcom (`''`), čo však nie je dobré riešenie na vyjadrenie neexistencie.

**Upozornenie:** Typ `null` nie je skalárny.

Nula (0) vs. nula
----------------

Mnohí vývojári majú pri začiatku vývoja problém pochopiť rozdiel medzi `0` (nula) a `null` (neexistujúca hodnota).

Tento rozdiel sa dá veľmi dobre a vtipne vysvetliť na nasledujúcom obrázku:

<img src="{$baseUrl}/images/0-vs-null.jpg" alt="0 vs. null" class="w-100 mb-3">

Ručné prebalenie
--------------------

Niektoré typy sa dajú navzájom konvertovať. Cieľový dátový typ sa uvádza pomocou okrúhlych zátvoriek a možno ho uviesť kdekoľvek v aplikácii, napríklad pri výpise hodnoty.

Napríklad:

```php
$pi = 3.14;

echo $pi;       // výtlačky 3.14

echo (int) $pi; // vytlačí 3
```

Dynamické dopĺňanie
---------------------

Uvažujme nasledujúce 2 premenné:

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

Porovnanie typov
----------------

Pri porovnávaní hodnôt je potrebné zohľadniť rôzne typy údajov.

Vo všeobecnosti sa operátor `==` používa na všeobecné porovnanie dvoch hodnôt (bez ohľadu na typ), zatiaľ čo operátor `===` porovnáva hodnoty aj dátový typ.

Napríklad:

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
