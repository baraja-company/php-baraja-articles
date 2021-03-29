Příkazy, klíčová slova a funkce v PHP
=====================================

> id: "96a00928-4410-479d-ada0-612de21cdacd"
> slugCS: prikazy-a-funkce
> publicationDate: "2019-09-11 10:18:03"
> mainCategoryId: "6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070"

Každý PHP script se skládá z příkazů a funkcí, dohromady se tomu říká **konstrukty**. Když se script zpracovává, tak se tyto výrazy jazyka sledují (tomuto procesu se říká **tokenizace**) a ná základě *klíčových slov* se interpret rozhoduje, jak se má procesor zachovat.

Příkazy
--------------------------

Příkazy (<a href="https://php.net/manual/en/reserved.keywords.php">v angličtině se jim říká **keywords**</a>) jsou již předprogramované části jazyka, jsou rezervované přímo pro svůj účel, můžeme je ihned použít za libovolné situace (nevyžadují další speciální knihovny nebo instalaci) a skládají se z nich základy všech scriptů.

Například <a href="/echo">výpis řetězce do HTML kódu</a>:

```php
echo 'Echo je příkaz jazyka na výpis obsahu';
```

U příkazů je důležité si uvědomit, že nevrací žádnou hodnotu a nelze je tedy použít jako proměnnou. Příkazy lze také poznat podle toho, že neobsahují závorku.

Příklady:

```php
echo 'Ahoj světe!';
echo ('Ahoj světe!');
```

Pokud obsah příkazu obalím závorkami, tak se chování nijak nezmění a bude na něj nahlíženo jako na **výraz**. Je to to samé, jako kdybychom v matematice zapsali:

```php
5 + 3

// nebo:

(5 + 3)
```

Technicky vzato v tom rozdíl je, ale v praxi se nic nemění.

Funkce
--------------------------

Kdyby existovaly jen příkazy, tak by to byla celkem nuda. Funkce jsou souhrnem více příkazů.

Často chceme stejný kód provádět na více místech opakovaně. V takovém případě by bylo neustálé opisování příliš náročné a mohlo by vést k chybám, proto takový kód obalíme do funkce, kterou pojmenujeme a následně ji jen voláme podle názvu.

Při volání funkce ji předáme parametry jako proměnné, ona provede zadaný kód a vrátí výsledek, který můžeme dále použít.

```php
$text = 'PHP je můj oblíbený jazyk!';

echo 'Text "' . $text . '" je dlouhý ' . strlen($text) . ' znaků.';
```

PHP má mnoho funkcí přímo předdefinovaných (<a href="/dokumentace">kompletní seznam je v dokumentaci</a>), často je ovšem výhodné si definovat vlastní:

```php
function mojeFunkce(int $x, int $y): int
{
   $z = $x + $y;   // sečte vstupní parametry a uloží do proměnné

   return $z;      // vrátí proměnnou $z
}

echo mojeFunkce(5, 3); // vypíše číslo 8, protože čísla zpracovala funkce
```

Proměnné ve funkcích
--------------------------

V rámci funkcí se používají tzv. lokální proměnné, to znamená, že mohou být použity jen uvnitř funkce a nelze s nimi pracovat (ani definovat) mimo funkci. Počáteční hodnoty získají z parametrů funkce přímo v její definici.

Příklad:

```php
$z = 5;

function prvniFunkce(int $x, int $y): int
{
   return $x - $y;   // toto by vrátilo rozdíl čísel
}

function druhaFunkce() {
   return $z;        // toto vrátí chybu, protože proměnná $z
                     // není uvnitř funkce definována
}
```

Někdy se hodí některý z parametrů nastavit jako nepovinný, to se udělá tak, že se definuje náhradní (výchozí) hodnota:

```php
echo prictiCislo(5);    // vrátí 6
echo prictiCislo(5, 7); // vrátí 12

function prictiCislo(int $x, int $y = 1): int
{
   return $x + $y;
}
```

Funkce `prictiCislo()` přičítá k proměnné `$x` hodnotu z proměnné `$y`. Pokud není proměnná `$y` definována (uvedena jako parametr při volání funkce), bude použita její výchozí hodnota zapsaná v definici funkce. Druhý parametr funkce (proměnná `$y`) je tedy nepovinný.

> Každá funkce může mít jen jeden výstup (jeden return), pokud ve funkci uvedete více výstupů, tak bude vrácen ten, který je uveden jako první. Pokud chcete vrátit více hodnot, musíte použít pole.
>
> V PHP (na rozdíl od jiných jazyků) nemusíme uvádět return vůbec, v takovém případě pak funkce na libovolný vstup nevrací nic (void). Takové funkce slouží většinou k ukládání dat nebo výstupu do zdrojového kódu. Všeobecně se ale doporučuje vždy nějakou hodnotu vracet, minimálně potvrzení o tom, že vše proběhlo v pořádku.
