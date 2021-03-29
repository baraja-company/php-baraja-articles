Echo - výstup do zdrojového kódu
================================

> id: d6a1a7bf-03bf-49ea-9947-d4d6753ca6e4
> slugCS: echo
> perex: Echo - výpis řetězce do stránky a na standardní výstup. Možnosti escapování a HTTP hlaviček.
> publicationDate: "2020-02-16 18:40:31"
> mainCategoryId: "6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070"

Kontrukt `echo` slouží pro výpis proměnné nebo řetězce do zdrojového kódu.

| Podpora:       | Všechny verze
|----------------|------
| Stručný popis: | Výstup jednoho nebo více řetězců
| Typ:           | <a href="/prikazy-a-funkce">Příkaz, konstrukt</a> (není funkce)

Popis
-----

```php
echo 'ahoj světe';
```

Vypíše "ahoj světe".

```php
$var = 'Text';
echo $var;
```

Vypíše hodnotu proměnné `$var`, tedy "Text".

**Echo** není funkce (je to <a href="/prikazy-a-funkce">příkaz</a>), takže můžete ale nemusíte použít závorku. Tedy zápis `echo ('ahoj světe');` je taky správně.

> **Odborná vsuvka:** PHP zpracovává **Echo** jako příkaz (konstrukt) a pracuje s ním tedy jako s výrazem. Závorka v tomto případě není povinná. Pokud uvedeme zápis: `echo ('něco');`, tak se z příkazu **Echo** nestane funkce a ani se s ní tak nepracuje. Závorka v tomto případě znamená uzavření přesné hodnoty výrazu podobně, jako to funguje v matematice.

Uvozovky
--------

Řetězce lze uzavírat do uvozovek i apostrofů.

Tedy toto:

```php
echo "ahoj";
```


Je to samé jako toto:

```php
echo 'ahoj';
```


Ale pozor na to, že každý řetězec musí začínat a končit stejným typem uvozovacího znaku a v řetězci nesmí být tato uvozovka použita.

Pokud chcete vypsat třeba HTML odkaz (nebo jakýkoli HTML kód), musíte před uvozovku napsat lomítko. Lomítko znamená "přesně tento znak", takže není chápán jako výraz v jazyce.

```php
echo "<a href=\"index.php\">text odkazu</a>";
```


Technická vsuvka: Uvozovky mají v PHP <a href="/uvozovky-vyznam">speciální význam</a>.

Parametry
---------

- `arg` Parametr výstupu.

Návratové hodnoty
-----------------

Nevrací se žádná hodnota.

Nelze použít jako proměnnou.

Poznámka
--------

Poznámka: Protože se jedná o **jazykový konstrukt** (konstrukt = příkaz) (není to funkce), tak nelze načíst do proměnné.

Příklad
-------

```php
echo "ahoj světe";

echo "echo umí vypsati více řádkový text.
Ale pozor na HTML značku <br>, ta se totiž nevypíše. Od toho je funkce nl2br().";

$a = "php";				// definice proměnné

echo "Mám rád " . $a;	// Vypíše: Mám rád php
```

**Echo** má také zkrácenou syntaxi, kdy je možné následně za otvíracím php tagem použít jen znak rovnítka.

```php
Ahoj <?=$jmeno;?>!
```

To se hodí, pokud potřebujeme do stránky vypsat nějakou rychlou informaci. Třeba aktuální rok:

```php
Píše Jan Barášek © <?=date('Y');?>
```

> Tato zkrácená syntaxe bude fungovat pouze jsou-li povoleny zkrácené otvírací php tagy, tj. direktiva `short_open_tag` je nastavena na `on`.

Operace
-------

V rámci příkazu **echo** je možné provádět všechny běžné matematické operace.

Podrobně o matematice pojednává <a href="/matematika">samostatný článek</a>.

```php
echo 5 + 3 * 2; // vypíše 11
```
