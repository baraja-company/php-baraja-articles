PHP funkce Explode - rozdělení řetězce podle oddělovače
================================

> id: 4dc7cec6-0e96-4a6b-aee8-32c8817ba11e
> slugCS: explode
> perex: Rozdělení řetězce na více částí podle oddělovače.
> publicationDate: 2019-11-26 11:39:36
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec

Explode slouží k snadnému rozdělení řetězce podle oddělovače.

- Jednotlivé výsledky vrací jako pole číslované od nuly,
- nelze vložit pole, vstupem je pouze řetězec,
- nelze oddělovač měnit během parsování, nelze zvolit více oddělovačů.

| Podpora       | PHP 4 a novější |
|---------------|-----------------|
| Stručný popis | Rozdělení řetězce do pole podle oddělovače.
| Požadavky     | Žádné
| Poznámka      | Nelze vložit pole, jen řetězec.

Často potřebujeme rozdělit řetězec podle nějakého jednoduchého pravidla. Třeba seznam čísel oddělených čárkou.

Na to se výborně hodí funkce explode(), která prvním parametrem přijímá separátor (rozdělující řetězec) a druhým parametrem samotná data:

```php
$cisla = '3, 1, 4, 1, 5, 9';
$parser = explode(', ', $cisla);

foreach ($parser as $cislo) {
	echo $cislo . '<br>';
	// Tady můžeme čísla dále zpracovat
}
```

Co když ale budou čísla oddělena čárkou, ale kolem budou různě mezery?

Řešení je parsování podle nejmenšího společného podřetězce a následné odstranění nechtěných znaků kolem (mezery a jiné bílé znaky):

```php
$cisla = '3,   1,4, 1  ,   5  ,9';
$parser = explode(',', $cisla);

foreach ($parser as $cislo) {
     echo trim($cislo) . '<br>';
     // Tady můžeme čísla dále zpracovat
}
```

Funkce `trim()` v tomto případě elegantně odstraní bílé znaky kolem (mezery, tabulátory, konce řádků, ...), čímž dostaneme jen čistá data.

Limit, omezení počtu vyparsovaných entit do pole
--------------------------

> TIP: Na mnoho příkladů se explode() nehodí a je mnohem lepší použít regulární výrazy.

Často ovšem chceme data parsovat jen do určité vzdálenosti, k tomu lze omezeně použít třetí (nepovinný) parametr limit.

Mějme například strukturovaná data oddělená dvojtečkou, u kterých chceme získat obsah za první dvojtečkou a další dvojtečky ignorovat.
Příklad:

```php
$cas = 'format: "j.n.Y - H:i"';
```

Kdybychom řetězec parsovali pouze jako:

```php
$parser = explode(':', $cas);
```

Získali bychom tyto 3 podřetězce (v jiných případech by jich mohlo být mnohem více):

```php
'format'
' "j.n.Y - H'
'i"'
```

Proto do parseru nastavíme limit, kolik prvků získat (a případně všechny další budou připojeny na konec posledního prvku):

```php
$parser = explode(':', $cas, 2);

// vrátí:
echo $parser[0]; // format
echo $parser[1]; // "j.n.Y - H:i"
```

> **Poznámka:** Nechtěné uvozovky lze z řetězce odmazat například funkcí `trim()`:

```php
echo trim($parser[1], '"'); // druhý parametr uvádí mapu znaků k odstranění
```

Between, získání řetězce mezi dvěma řetězci
--------------------------

Často potřebujeme získat řetězec, který je ohraničen dvěma jinými řetězci. Přímo v PHP na to žádná funkce neexistuje, ale můžeme si jí napsat sami:

```php
function between($left, $right, $data) {
   $l = explode($left, $data);
   $r = explode($right, $l[1]);
   return $r[0];
}
```

Regulární výrazy
--------------------------

Mnohem elegantnějšího rozdělování a práci s řetězci lze dosáhnout za pomoci <a href="/regex">regulárních výrazů</a>, které probírám na samostatné stránce.

Podobné funkce
--------------------------

- <a href="/funkce-implode">Implode()</a> - spojení pole do řetězce

Příklad parsování IP adresy
--------------------------

```php
$ip = '10.0.0.138'; 
$parser = explode('.', $ip); 
echo $parser[0]; // vypíše "10" 
echo $parser[1]; // vypíše "0"; 
```

V proměnné `$ip` je vstupní řetězec, který je parsován podle oddělovače `.`, návratem je pole. Parsování probíhá až do konce řetězce, pokud není stanoven limit.

Parametry
--------------------------

| # | Typ    | Popis
|---|--------|------|
| 1 | string | Oddělovací řetězec.
| 2 | string | Parsovaný řetězec.
| 3 | int    | Limit parsování. Jedná se o nepovinný parametr. Příklad:

```php
$text = 'PHP je můj nejoblíbenější jazyk!'; 
$parser = explode(' ', $text, 1);

echo $parser[0]; // vypíše první slovo 
echo $parser[1]; // vypíše zbytek textu 
echo $parser[2]; // nevypíše nic, protože byl nastaven limit! 
```

Návratové hodnoty
--------------------------

Návratem je pole s rozparsovaným řetězcem.

Indexy jsou číslovány od nuly až po `X`, pokud není určen limit.

Rozdíly oproti starším verzím
--------------------------

| Verze PHP | Popis |
|-----------|-------|
| 5.1.0     | Byla přidána podpora záporného limitu při pasování.
| 4.0.1     | Přidán nepovinný parametr **limit**.

Tipy a poznámky
--------------------------

Při použití záporného **limitu** se uvede počet prvků od konce řetězce.

Příklad:

```php
$str = 'one|two|three|four'; 
 
// kladný limit 
print_r(explode('|', $str, 2)); 
 
// záporný limit (od PHP 5.1) 
print_r(explode('|', $str, -1)); 
```

Návratem budou následující pole:

```php
Array 
( 
    [0] => one 
    [1] => two|three|four 
)

Array 
( 
    [0] => one 
    [1] => two 
    [2] => three 
)
```
