Podmínky v PHP - IF() {...} - možnosti větvení
==============================================

> id: "3e4b81bb-a8bb-4e99-8842-e76f1658a371"
> slug:
> 	cs: if
> 
> perex: "Podmínky, logické výrazy, booleovská logika a možnosti větvení PHP scriptů. Vyhodnocování logických výrazů a operátory. Možnosti skládání výrazu."
> publicationDate: "2019-11-26 11:55:09"
> mainCategoryId: "6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070"

> **Poznámka:** Tento článek může být pro některé začátečníky mírně chaotický, protože předpokládá základní znalosti o PHP. Pokud vás zajímá, jak podmínky fungují, přečtěte si <a href="/podminky">o podmínkách v začátečnickém kurzu</a>.

| Podpora: 			| Všechny verze: PHP 4, PHP 5, PHP 7
|-------------------|----------|
| Stručný popis:	| Kontrola platnosti jednoho nebo více výroků
| Typ: 				| <a href="/prikazy-a-funkce">Příkaz, konstrukt</a> (není funkce)

Popis
-----

Často je potřeba zjistit, zda platí nějaká rovnost nebo zda je výrok pravdivý, k tomu slouží podmínky. PHP používá jako mnoho jiných jazyků (zejména jazyk C) následující syntaxi:

```php
if (logický výrok) {
    // konstrukt
}
```

Každý logický výraz má hodnotu `TRUE` (pravda) nebo `FALSE` (nepravda), jiné možnosti nejsou.

Příklad porovnání, zda je proměnná `$x` větší než proměnná `$y`:

```php
$x = 10;
$y = 5;

if ($x > $y) {
    // Tato část scriptu se spustí, pokud podmínka platí
} else {
    // Tato část scriptu se spustí, pokud podmínka neplatí
}
```

Konstrukt podmínky má povinný obsah v kulaté závorce, ve které se uvádí testovaný výraz, složený z operátorů (přehled níže), více výrazů lze propojit pomocí logických operátorů (přehled níže).

Dále podmínka obsahuje dva nepovinné bloky příkazů.

- Pokud podmínka platí
- Pokud podmínka neplatí

Z praktických důvodů se vždy uvádí minimálně první blok příkazů, kdy podmínka platí, jinak by totiž testování výrazu nedávalo smysl.

Použití středníku a závorek
--------------------------

Všeobecně platí, že:
- **Kulatá** závorka se používá pro oddělení logického výrazu (další kulaté závorky je možné zanořit a docílit tak složitějších výrazů).
- **Složená** závorka slouží pro vymezení bloku příkazů a funkcí.
- **Středník** se nepoužívá pro označení podmínky (vymezení bloku příkazů se provádí složenou závorkou), ale pro oddělení jednotlivých příkazů uvnitř podmínky).

Jediný možný zápis se středníkem (vyjma použití konstruktu **endif**):

```php
if ($x > $y);
```


Taková podmínka však nemá smysl, protože v obou případech bude výsledek porovnávání zahozen a nespustí se žádný příkaz patřící k podmínce.

Alternativní zápis
--------------------------

Za určitých okolností lze využít konstrukt `if` s vynecháním složených závorek. Toho můžeme dosáhnout jen v těchto případech:

- Pokud v podmínce provádíme jen jeden příkaz.
- Pokud místo složených závorek použijeme dvojtečku a endif;
- Pokud používáme "in-line" zápis.

Pro podrobnější informace vizte následující 3 kapitoly.

> **`1. Jen jeden příkaz ~ zkrácená syntaxe`**

Pokud vytváříte podmínku, ve které chceme provést jen jeden konstrukt (příkaz), tak můžeme použít buď klasický zápis se složenou závorkou:

```php
if ($x > 10) { $y = $x; }
```


Nebo můžeme závorky vynechat:

```php
if ($x > 10) $y = $x;
```


Toto chování se však týká jen jednoho příkazu v bezprostřední blízkosti podmínky.

Lepší příklad (podmíněně se provede jen konstrukt `$y = $x`, zbytek vždy):

```php
$x = 5;
$y = 3;
$z = 10;
if ($x > $y)
$y = $x;
$x = 3;
```

> **`2. Dvojtečka a endif;`**

```php
if (výraz):
    konstrukt;
    konstrukt;
    konstrukt;
endif;
```

Tento zápis je však dlouhodobě považován za zastaralý, protože snižuje v orientaci při zanoření více podmínek do sebe samotných.

> **Poznámka:** Rád bych poznamenal, že tento styl mají rádi také někteří lidé, například Yuhů (<a href="https://www.jakpsatweb.cz/php/php-tahak.html#vetveni">více v jeho článku</a>). Chraň vás ruka páně tohle někde použít.

> **`3. Ternární výraz ~ jednořádkový "in-line" zápis`**

Občas se hodí provést jednoduché porovnání v rámci jednoho řádku s nějakou jinou akcí (například společně s definováním nové proměnné). Pokud chceme provést jen jeden příkaz, tak lze celý postup zkrátit na jeden jediný řádek i při zachování maximální jednoduchosti.

```php
$x = 5;
$xJeVetsiNezDva = ($x > 2 ? TRUE : FALSE);

// nebo ještě kratší:

$xJeVetsiNezDva = ($x > 2);
```

Tabulky operátorů
-----------------

V rámci podmínky se používají dva typy operátorů:

- **Srovnávací** ~ porovnávají nějaký konkrétní vztah,
- **Logické** ~ spojují více výrazů a vytváří komplexní podmínky.

Srovnávací operátory
-----------------------

| Operátor | Význam   |
|----------|----------|
| `==` 	   | Rovná se
| `===`    | Rovná se a má stejný datový typ
| `!=` 	   | Nerovná se
| `>=` 	   | Rovná se nebo je větší
| `<=` 	   | Rovná se nebo je menší
| `>` 	   | Větší
| `<` 	   | Menší

Příklad (platí, když `$x není 5`):

```php
if ($x != 5) { ... }
```

Logické operátory
--------------------

| Operátor 	| Alternativa  | Význam 		| Pravda, když:
|-----------|--------------|----------------|--------------------------------------------
| `&&`      | AND 		   | a zároveň 		| jsou obě hodnoty pravdivé
| `||`      | OR 		   | nebo 			| je alespoň jedna hodnota pravdivá
| `^^`      | XOR 		   | exlusive OR 	| je aspoň jedno pravda nebo nepravda, ale nikdy ne oboje
| `!`       | *nemá*	   | negace výrazu	| `true` když bylo `false` a naopak
| `()`      | *nemá*	   | zanoření výrazů| záleží na okolnostech

Složitější příklad:

```php
$x = 5;
$y = 3;
$z = 8;
if ($x > 0 && !($y != 2 && $z == $x) || $z > $y) { ... }
```


Vynechání logických a porovnávacích operátorů
---------------------------------------------

Často si můžeme dovolit vynechat některý z operátorů (nebo dokonce i oba), nikdy ale nesmíme zapomenout na pravidla správného použití, aby výsledný výraz fungoval.

Všeobecně platí, že při testování výrazu bez operátoru se zkouší, zda je jeho hodnota `TRUE`, nebo je neprázdná (například obsahuje nenulové číslo, neprázdný řetězec, ...).

Příklady:

```php
$x = 5;
$y = 3;
$z = 8;

if ($x) { ... }         // vyhoví, protože $x není prázdné
if ($x && $y) { ... }   // vyhoví, protože $x a $y není prázdné
if (!$x) { ... }        // nevyhoví, protože TRUE je negováno
if (isset($z)) { ... }  // vyhoví, protože proměnná $z existuje
```

Mohou ale nastat zrádné situace, zejména když:
- Se ptám na `if ($x)` a proměnná `$x` obsahuje nulu (`0`), pak podmínka není splněna.
- Nebo proměnná `$x` obsahuje řetězec `'0'` (číslo nula), protože se přetypuje na nulu a výraz tedy není pravda.
- V podmínce je funkce, která vždy vrací nějaký neprázdný řetězec, pak totiž podmínka vždy platí.
- Nebo když kontrolujeme vstup od uživatele a on vyplatí `'false'` jako string, pak opět podmínka platí, protože řetězec není prázdný.

Na toto doporučuji jedno jednoduché a účelné řešení - ptát se na počet znaků, které se vrací. Pokud je řetězec prázdný (nebo proměnná neexistuje), tak se vrátí nulový počet znaků a podmínka není splněna. Jednoduchý příklad:

```php
$x = '0';
if ($x) { ... }			// podmínka za běžných okolností neplatí
if (strlen($x)) { ... }	// podmínka platí, protože $x obsahuje 1 znak
```

Dále můžeme testovat existenci proměnné funkcí `isset()`.

Porovnání řetězců
-----------------

Zjistit, že jsou řetězce shodné, je jednoduché:

```php
$a = 'Kočka';
$b = 'kočka';

if ($a === $b) {
    // Pokud jsou řetězce stejné
} else {
    // Pokud jsou řetězce různé
}
```


Důležité je správně ohlídat datové typy v případě, že by mohl být zápis ekvivaletní k nějakému jinému.

Například prázdný řetězec `$a = '';` je něco jiného, než řetězec `NULL`: `$b = NULL;`. Rozlišovat to musíme například kvůli databázím, kde je rozdíl mezi tím, že hodnota neexistuje, nebo je prázdná.

```php
$a = '';
$b = null;

if ($a == $b) {
    // Bude vyhodnoceno jako TRUE, protože
    // dojde k převodu datového typu.
}

if ($a === $b) {
    // Provede mnohem přísnější validaci
    // a neprojde, protože jde o jiný
    // obsah a jiný datový typ, proto se
    // tento kód nikdy nespustí.
}
```

Dále je při porovnávání řetězců dobré zanedbat bílé (neviditelné) znaky, jako jsou mezery, tabulátory a odřádkování. To se hodí například při zadávání hesla a předání do hashovací funkce:

```php
$password = '81dc9bdb52d04dc20036dbd8313ed055'; // 1234
$userPassword = ' 1234   ';

if (md5(trim($userPassword)) === $password) {
    // Funkce trim() provede automatické odmazání mezer.
}
```

Neznámá (neexistující) hodnota?
--------------------------

Někdy se může stát, že hodnota neexistuje (není tedy `TRUE` ani `FALSE`), jde zejména o hodnotu získanou z databáze (například se ptáme na sloupec, který neexistuje), v takovém případě bude vrácen datový typ `NULL`.

Obecně se `NULL` vyhodnocuje jako `FALSE`, tj. podmínka neplatí. Toto chování ovšem nemusí být vždy výhodné, protože neexistující hodnota ještě nemusí nutně znamenat, že neexistuje záznam.

> Příklad z praxe: Máme uživatelský profil a ptáme se na webovou stránku uživatele. Ne všichni uživatelé musí mít webovou stránku, v takovém případě se tedy vrátí `NULL`, ale uživatel i přesto existuje. V tomto případě bychom tedy měli spíše použít funkci `isset()`, abychom otestovali (ne)existenci proměnné a nedělali závěr na základě konkrétní hodnoty.
