Proměnné v PHP
================================

> id: b89774e7-143c-4a8c-8dc6-3b3d2c78d5b7
> slugCS: promenna
> publicationDate: "2019-08-22 20:48:46"
> mainCategoryId: "0e39aee9-2818-480c-8081-e0c2d039bb24"

Tato stránka je kompletní shrnutí fungování proměnných v PHP. Text je psán mírně odborným stylem a začátečníci mu nemusí plně rozumět. Pokud vás zajímají úplné základy, tak si přečtěte <a href="/prvni-script">tutoriál pro začátečníky</a> a <a href="/zasady-promennych">zásady zápisu proměnných</a>.

Popis
-----

Proměnná je virtuální místo v operační paměti, je definována `pomocí názvu` a `datového typu`. V rámci datového typu poté proměnná má nějaký `obsah`.

PHP proměnné interně reprezentuje jako tvz. hashovací tabulku, tj. všechny proměnné jsou uloženy v tabulce, která se velice rychle prohledává, proto čas potřebný k přístupu ke každé promněnné je *téměř* konstantní.

Příklady zápisu:

```php
$a = 10;
$b = 'kočka';
$c = true;
```

Každý řádek v ukázce značí definici jedné proměnné. Název každé proměnné začíná znakem dolaru `$` následovaný samotným názvem. Znakem rovnítka můžeme přiřadit do proměnné hodnotu.

Interně budou proměnné uloženy do paměti jako hashovací tabulka:

| Název | Typ     | Zkratka | Hodnota |
|-------|---------|---------|---------|
| `$a`  | integer | int     | `10`    |
| `$b`  | string  | string  | `kočka` |
| `$c`  | boolean | bool    | `true`  |

Typy proměnných
---------------

Proměnné se rozdělují podle přístupových práv a způsobu použití:

- <a href="/lokalni-promenna">Lokální proměnné</a> (platná v rámci kontextu, tj. v rámci funkce a metody),
- <a href="/globalni-promenna">Globální proměnné</a> (platná v rámci celého scriptu),
- <a href="/superglobalni-promenna">Superglobální proměnné</a> (platná v rámci celého prostředí serveru - typicky data od uživatele),
- <a href="/promenna-promenna">Proměnné proměnné</a> (speciální proměnná, která obsahuje referenci (odkaz) na jinou proměnnou).

*`Globální proměnné` a `proměnné proměnné` by se neměly používat, protože přispívají k nečitelnosti kódu a "magickému" (nečekanému) chování aplikace.*

Povolený obsah proměnných
--------------------------

Proměnné mohou obsahovat cokoli, co dovoluje jejich aktuální datový typ. Pokud datový typ neuvedeme, bude určen automaticky, podle obsahu (tento postup se příliš nedoporučuje, protože může vést k chybám).

Datové typ fungují samostatně, proto můžeme používat téměř jakékoli typy. Pokud však chceme provést nějakou slučovací operaci, tak musíme vždy zajistit převod na jeden formát.

Dobrým příkladem je třeba sčítání a násobení čísel:

```php
$x = 5;       // integer
$y = 3;       // integer
$z = $y + $y; // proměnná $z bude složena na základě více proměnných
```


V tomto případě stojí PHP před otázkou, jaký datový typ bude mít nově vzniklá proměnná `$z`. Pokud se jedná o stejné datové typy a operace je možná, tak se datový typ zdědí.

Někdy můžeme ovšem provádět operaci s více datovými typy:

```php
$x = 1;       // integer
$y = 3.14159; // float
$z = $y + $y; // float
```


V tomto případě slučujeme integer a float. Výstupem bude desetinné číslo, proto se použije float. V tomto případě PHP provede něco, čemu se říká **dynamické přetypování**.

Na toto chování se ovšem nemůžeme vždy spolehnout. Jak byste například chtěli sloučit číslo a string?

```php
$x = 256;     // integer
$y = 'Ahoj!'; // float
$z = $y + $y; // ???
```

Datové typy (přehled nejdůležitějších)
--------------------------------------

PHP je jazyk interpretovaný, proto má některé zvláštnosti oproti jiným programovacím jazykům, jedna z nich je například ta, že u proměnných nemusíme uvádět datové typy, tj. každá proměnná svůj datový typ dynamicky mění podle svého obsahu (pokud není řečeno jinak). Přesto je dobré znát alespoň základní datové typy, hodí se to zejména u optimalizace aplikací nebo při práci s databází.

Zápis pak může vypadat následovně:

```php
$x = (int) 25; // vytvoří proměnnou typu integer
```

<a href="/datove-typy">Přehled datových typů</a>.

Dědičnost datového typu
-----------------------

Jaký datový typ bude mít proměnná `$x`, pokud známe jen tento kousek zdrojového kódu?

```php
$x = $y;
```

Záleží na datovém typu proměnné `$y`, z kterého se bude dědit jak hodnota, tak i její datový typ. V tomto případě tedy neznáme proměnnou `$x`, proto nemůžeme pokračovat ve vyhodnocování kódu a vyhodilo by se chybové hlášení.

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

Reprezentace řetězců jako pole
------------------------------

Všechny řetězce jsou interně uloženy jako pole znaků. Tj. každý znak má svůj vlastní **index** a lze se na něj odkázat. Pokud index neuvádíme, tak se pracuje s celým řetězcem.

```php
$x = 'Programujme v PHP!';
$n = 3;

echo $x;		// toto vypíše celý obsah proměnné $x
echo $x[0];		// toto vypíše nultý znak proměnné $x
echo $x[$n];	// toto vypíše n-tý znak proměnné $x
```

> **Poznámka:** PHP čísluje od nuly, tj. nultý znak je v tomto případě 'P' a první znak je 'r'.
>
> Znaky se navíc prochází po bajtech. Například znak "č" v kódování UTF-8 má 2 bajty, proto při procházení nebude sedit index znaku v řetězci vůči reálné pozici a pro uložení znaku se použijí 2 indexy.

Existenci prvku pole bychom měli vždy ověřit funkcí `isset()`:

```php
if (isset($x[$n])) {
	echo $x[$n];
}
```

Případně to lze hezky zapsat ternárním operátorem:

```php
echo $x[$n] ?? '';
```

Kopírování proměnných
---------------------

Mějme následující proměnnou:

```php
$q = 'Lorem ipsum, ...';
```

A následně její hodnotu překopírujeme do další proměnné:

```php
$qi = $q;
```

Naštěstí se žádné kopírování provádět nebude a PHP si jen uloží odkaz na hodnotu do **hashovací tabulky**. Hodnota se reálně překopíruje, až když by se měla hodnota některé z proměnných změnit. Toto chování zabezpečuje komponenta, které se obecně říká **Garbridge collector**.
