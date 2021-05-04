Datové typy v PHP
=================

> id: cd63818c-259a-484e-b006-86c35b362019
> slug:
> 	cs: datove-typy
> 
> perex: "Datové typy v PHP: String, integer, float, boolean, pole (array), objekt a další."
> publicationDate: "2019-08-23 15:44:28"
> mainCategoryId: "0e39aee9-2818-480c-8081-e0c2d039bb24"

Veškerá zpracovávaná data v PHP jsou určitého typu. Například celé číslo, řetězec nebo boolean (pravda / nepravda).

Základní datové typy
--------------------

Základním typů se říká také **primitivní datové typy**, nebo také <a href="/funkce-is-scalar">skalární typy</a>.

| Typ     | Název           | Popis |
|---------|-----------------|-------|
| `int` | Celé číslo      | (`integer`) Obsahuje pouze celé číslo. Maximální hodnota je určena podle počtu bitů. Na 32-bitovém PHP je rozsah od `-2,147,483,648` do `2,147,483,647` (~ ± 2 miliardy), 64-bitové PHP má rozsah od `-9,223,372,036,854,775,808` do `9,223,372,036,854,775,807` (~ ± 9 quintillionů). Maximální hodnotu můžeme vždy získat zavoláním konstanty `PHP_INT_MAX`. Pokud maximální hodnotu integeru přesáhneme, bude PHP číslo reprezentovat jako `float` a automaticky ho přetypuje.
| `float`   | Desetinné číslo s pohyblivou řádovou čárkou | Jde o variantu čísla s plovoucí řádovou čárkou, pro které platí pravidlo *"čím menší, tím přesnější"*. Číslo se interně uloží jako tzv. **mantisa** a **exponent**, takže se vlastně ukládají 2 čísla, mezi kterými se provádí operace: `mantisa * (2 ^ exponent)`, čímž je možné uložit opravdu obří rozsah čísel. Využívá se principu, že u velkých čísel nepotřebujeme vždy znát přesně jejich hodnotu, zato chceme ušetřit co nejvíce paměti. Čísla typu `float` nemusí být uloženy přesně a neměly by se používat pro výpočet peněz.
| `string`  | Řetězec         | Obsahuje posloupnost znaků, které se ohraničují pomocí uvozovek nebo apostrofů. Maximální délku omezuje jen kapacita operační paměti. Řetězec může být uložen v libovolném kódování, obsahovat emoji nebo binární data.
| `bool` | Logická hodnota | (`boolean`) Logická hodnota z booleovy algebry, může obsahovat pouze `true` (pravda) nebo `false` (nepravda).
| `null`    | Prázdná hodnota | Prázdná hodnota `null` se hodí pro případy, kdy chceme vyjádřit, že něco neexistuje. Například článek nemá kategorii. Někdy se `null` nesprávně nahrazuje za nulu (`0`) nebo prázdný řetězce (`''`), nicméně to není pro vyjádření neexistence vhodné řešení.

**Pozor:** Typ `null` není skalární.

Nula (0) vs. null
----------------

Řada vývojářů má při začátcích vývoje problém pochopit rozdíl mezi hodnotou `0` (nula) a `null` (neexistující hodnota).

Tento rozdlíl lze velmi dobře a vtipně vysvětlit následujícím obrázkem:

<img src="{$baseUrl}/images/0-vs-null.jpg" alt="0 vs. null" class="w-100 mb-3">

Manuální přetypování
--------------------

Některé typy lze mezi sebou převádět. Cílový datový typ se uvádí pomocí kulaté závorky a lze uvést kdekoli v aplikaci, například při výpisu hodnoty.

Například:

```php
$pi = 3.14;

echo $pi;       // vypíše 3.14

echo (int) $pi; // vypíše 3
```

Dynamické přetypování
---------------------

Mějme následující 2 proměnné:

```php
$x = 10;
$y = '10';
```

Jaký je rozdíl mezi obsahy proměnných `$x` a `$y`?

Proměnná `$x` je číslo, `$y` je řetězec (obsahující znak "1" a "0"), tedy v případě, že proměnnou pouze uložíme do paměti a nebudeme provádět nějakou operaci, která bude mít vliv na hodnotu. Například následující 2 zápisy budou vracet stejný výsledek:

```php
echo $x + 5;	// vypíše 15
echo $y + 5;	// vypíše 15
```

V druhém případě dojde k takzvanému **dynamickému přetypování**, tj. proměnná převede svůj datový typ tak, aby bylo možné s ní provést výpočetní operaci. Na toto chování se nelze vždy spolehnout a jedná se spíše o korektivní chování, co má za úkol opravit špatně napsané scripty začátečníků. Pokud to je možné, tak čísla vždy zapisujte s datovým typem pro uložení čísel, protože se tím zvyšuje jejich přesnost a usnadňuje se budoucí použití.

> **Poznámka:** Je potřeba si uvědomit, že nemůžeme převádět datové typy úplně libovolně a ne vždy je to tedy možné. Pokud budete přetypovávat datový typ na nějaký jiný (nekompatibilní) tak nemusí buď dojít k převodu vůbec, nebo se může původní obsah poškodit, či zcela zničit a nahradit za jiný. Pokud například budete přetypovávat řetězec na celé číslo (a v proměnné bude uložen nějaký text, který není číslem), tak bude místo číselné hodnoty do proměnné uložena hodnota `1`.

Porovnávání typů
----------------

Při porovnávání hodnot je potřeba brát v potaz různé datové typy.

Obecně operátor `==` slouží pro obecné porovnání dvou hodnot (bez ohledu na typy), zatímco `===` porovnává hodnoty i datový typ.

Například:

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
