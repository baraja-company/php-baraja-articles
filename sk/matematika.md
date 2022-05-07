Matematika v PHP
================

> id: '69a432ee-7635-46e2-bb21-8492cb62c4e6'
> slug:
> 	cs: matematika
> 	sk: matematika-v-php
> 
> perex: 'Matematika a matematické funkce v PHP. Definice čísel, operátorů a vhodné typy pro výpočty a ukládání čísel.'
> publicationDate: '2020-02-16 18:24:10'
> mainCategoryId: c2134b23-9b10-46b3-aa54-e3996707255e
> sourceContentHash: '1070f11c80eff1faf3388e47721b01a2'

Podobne ako v iných jazykoch, aj v PHP sú čísla reprezentované v binárnej sústave (sústava jednotiek a núl), takže niektoré matematické operácie môžu priniesť trochu iné výsledky, ako by sme očakávali. Táto stránka poskytuje prehľad správania sa výpočtov v rôznych kontextoch. Na pochopenie sú potrebné aspoň základné znalosti **číselných techník** a **číselných sústav**.

Reprezentácia čísel v pamäti
---------------------------

Spôsob reprezentácie čísel je charakterizovaný dátovým typom, napríklad celé číslo sa priamo zo zdrojového kódu prevádza z desiatkovej sústavy na dvojkovú. O prevod čísel sa nemusíme vôbec starať, všetko sa vykonáva automaticky.

Dôsledkom tohto prevodu je, že maximálna hodnota celého čísla je určená počtom bitov, ktoré sú k dispozícii v pamäti. V 32-bitovom PHP je rozsah od `-2 147 483 648` do `-2 147 483 647` (~ ± 2 miliardy), v 64-bitovom PHP je rozsah od `-9 223 372 036 854 775 808` do `-9 223 372 036 854 775 807` (~ ± 9 kvintiliónov). Maximálnu hodnotu možno vždy získať volaním konštanty `PHP_INT_MAX`.

Desatinné čísla sú uložené v dátovom type **float**, ktorý má obmedzenú presnosť, takže je interne uložený ako dvojica čísel, na ktoré sa vzťahuje matematická operácia `mantisa * (2^exponent)`. Praktickým dôsledkom je, že sme schopní uložiť relatívne veľké číslo (rádovo miliardy) do malého počtu bitov, len nie veľmi presne.

Dôsledkom použitia dátového typu **float** je, že výsledok výpočtu `1 - 0,9` nie je presne `0,1`.

Príklad z <a href="https://diskuse.jakpsatweb.cz/?action=vthread&forum=9&topic=97912">webových fór</a>:

```php
$x = 0.3;
$y = 0.4;
echo 0.7 - $x - $y; // vytlačí -5,5511151231258E-17
```

Ak čísla mierne upravíme:

```php
$x = 6.5;
$y = 7.5;
echo 14.0 - $x - $y; // vytlačí 0
```

Vo všeobecnosti sa o tejto problematike <a href="https://vtm.zive.cz/clanky/proc-pocitacum-delaji-problemy-desetinna-cisla/sc-870-a-185672/default.aspx">rozpráva v samostatnom článku na VTM.e15.cz: Prečo majú počítače problémy s desatinnými číslami</a>, alebo všeobecne o <a href="https://cs.wikipedia.org/wiki/Pohyblivá_řádová_čárka">plávajúcej bodke na Wikipédii</a>.

> Vo všeobecnosti je dobré výsledok po výpočte zaokrúhliť alebo sa desatinným číslam úplne vyhnúť. Napríklad cenu neukladajte v korunách (napríklad 14,90), ale v halieroch (napríklad 1490) a potom s ňou presne pracujte. Zaokrúhľovaniu sa venujeme v ďalšej časti tohto článku.

Matematické operácie
-------------------

```php
$a = 5;
$b = 3;

echo $a + $b;
```

Pre operácie možno použiť bežné matematické konvencie, ktoré rešpektujú prioritu operátorov podľa pravidiel matematiky (násobenie má prednosť pred sčítaním atď.). Hodnoty možno zapisovať priamo alebo čítať prostredníctvom premenných.

> Možno to nie je na prvý pohľad zrejmé, ale v matematickom príklade nie je napísané znamienko rovnosti (`=`). Z toho vyplýva, že sa dajú riešiť len jednoduché `binárne operácie`, ktoré vždy pracujú len s `dvomi prvkami` (nepočítajte rovnice, na to musíte použiť špecializovanú knižnicu).

Prehľad základných operátorov
----------------------------

> V stĺpci `výsledok` uvádzam, čo sa vypíše za predpokladu:

```php
$a = 5;
$b = 3;
$c = 10;
```

| Operácia | Označenie | Označenie slovami | Príklad písania | Výsledok
|-------------------|-----------|---------------|-----------------------|----------
| Sčítanie | `+` | Plus | `echo $a + $b;` | 8
| Odčítanie | `-` | Mínus | `echo $a - $b;` | 2
| Násobenie | `*` | Hviezdička | `echo $a * $b;` | 15
| delenie | `/` | lomítko | `echo $a / $b;` | 1,6666666666667
| zátvorky | `( )` | zátvorky | `echo $a + ($b * $c);`| 35
| Zlučovanie reťazcov | `.` | Obdoba | `echo $a . $b . $c;` | 5310
| Zvyšok po delení | `%` | Percentá | `echo $a % $b;` | 2
| Pridanie jedného | `++` | dvoch plusov | `echo $a++;` | 6
| Odpočítanie jednotky | `--` | dva mínusy | `echo $a--;` | 4
| Power | `**` | dve hviezdičky | `echo $a ** $b;` | 125

> Operátor power (dvojitá hviezdička) je k dispozícii až od verzie PHP 7.1. V ostatných verziách PHP musíte použiť univerzálnu funkciu `pow($a, $b)`.

Prehľad základných matematických funkcií
---------------------------------------

Ak riešite nejakú bežnú výpočtovú úlohu, s najväčšou pravdepodobnosťou už existuje funkcia priamo v PHP, tu je ich komentovaný prehľad.

Zaokrúhľovanie
--------------

Bežné zaokrúhľovanie sa vykonáva pomocou funkcie <a href="/function-round">round()</a>, ktorá má 3 parametre:

- Zaokrúhlené číslo
- Voliteľné: Presnosť (na koľko desatinných miest)
- Voliteľné: Polovičná hodnota (od akej hodnoty zaokrúhľovať nahor)

```php
round(5);               // 5
round(5.1);             // 5
round(5.4);             // 5
round(5.5);             // 6
round(5.8);             // 6
round(5483.47621, 2);   // 5483.47
round(5483.47621, -2);  // 5500
```

Existuje aj funkcia <a href="function-floor">floor()</a> na zaokrúhľovanie nadol a funkcia <a href="/function-ceil">ceil()</a> na zaokrúhľovanie nahor.

> **Pozor na dátové typy!**
>
> Všetky funkcie zaokrúhľovania vracajú výsledok ako `float`, aj keď je výsledkom celé číslo.
>
> > Ak chcete s výsledkom zaobchádzať ako s celým číslom, je dôležité prekryť typ výsledku. Napríklad: `echo (int) round(3.14);`

PHP pracuje s rôznymi dátovými typmi, napríklad pri type **float** sa môže ľahko stať, že výsledok funkcie `round()` nie je číslo s konečným desatinným rozšírením. Pre výpis odporúčam použiť funkciu `number_format()`, o ktorej hovorím nižšie.

Goniometrické funkcie
--------------------

Používajú sa na množstvo rôznych výpočtov a vychádzajú z jednotkového kruhu. Používajú sa napríklad pri vykresľovaní kružníc, elips, posunov atď.

- <a href="/funkcia-sin">sin()</a>
- <a href="/funkcia-cos">cos()</a>
- <a href="/funkcia-tan">tan()</a>

Cyklometrické funkcie
--------------------

Vypočítajte uhol z hodnôt `x` a `y`. Prevody kartézskych a polárnych súradníc.

- <a href="/funkcia-asin">asin()</a>
- <a href="/funkcia-acos">acos()</a>
- <a href="/funkcia-atan">atan()</a>

Hyperbolické funkcie
-------------------

Na základe jednotkovej hyperboly. Ich definície sa dajú ľahko opísať pomocou prirovnaní:

- Elipsa, kruh, kruh s polomerom 1
- Hyperbola, Rovnovážna hyperbola, Jednotková hyperbola

Používajú sa napríklad pri generovaní terénu a fyzikálnej simulácii.

- <a href="/funkcia-sinh">sinh()</a>
- <a href="/function-cosh">cosh()</a> (chainline)
- <a href="/funkcia-tanh">tanh()</a>

Hyperbolometrické funkcie
------------------------

Na základe jednotkovej hyperboly.

- <a href="/funkcia-asinh">asinh()</a>
- <a href="/funkcia-acosh">acosh()</a>
- <a href="/funkcia-atanh">atanh()</a>

Príklady použitia goniometrických funkcií
---------------------------------------

```php
echo sin(30);            // -0.98803162409286
echo sin(deg2rad(30));   // 0.5
echo cos(deg2rad(123));  // -0.54463903501503
echo 1/tan(deg2rad(45)); // 1 (onen kotangens)
echo sin(deg2rad(500));  // 0.64278760968654
echo atan2(deg2rad(50), deg2rad(23)); // 1.1396575860761
```

Kalkulačka - spracovanie matematických výrazov
--------------------------------------------

Niekedy sa môže stať, že potrebujeme spracovať matematický výraz ako užívateľský reťazec, napríklad `5+2^(1+3/2)`.

Neexistuje jednoduchý spôsob, ako spracovať takýto vstup v jazyku PHP, ale naprogramoval som sériu knižníc, ktoré tento problém riešia.

Podrobnosti nájdete v samostatnom článku <a href="/pokrocila-kalkulator">Kalkulátor v PHP: Spracovanie matematického výrazu ako reťazca</a>.

Operácie s veľkými číslami a presnosť výpočtov
------------------------------------------

Veľké čísla a desatinné čísla sa v PHP ukladajú ako plávajúce, navyše sa zaokrúhľujú na približne 15 desatinných miest, čo môže byť niekedy nepríjemné. Preto bola vytvorená knižnica **<a href="https://www.php.net/manual/en/book.bc.php">BCMath Arbitrary Precision Mathematics</a>**.

Čísla sú v tejto knižnici reprezentované ako **reťazce**, takže nie sú obmedzené nepresnosťou **float**, ale vykonávané operácie sú o niekoľko rádov pomalšie (ale stále rýchlejšie, ako keby sme takúto funkciu naprogramovali sami).

Ak napríklad chceme získať druhú odmocninu z 2 na 3 desatinné miesta, stačí zavolať:

```php
echo bcsqrt('2', 3); // 1.414
```
