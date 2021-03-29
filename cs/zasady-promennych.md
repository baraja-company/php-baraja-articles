Zásady zápisu proměnných
========================

> id: "4c8e631b-b305-4169-8885-4f9506155999"
> slugCS: zasady-promennych
> publicationDate: "2020-02-16 16:26:08"
> mainCategoryId: "0e39aee9-2818-480c-8081-e0c2d039bb24"

Toto je druhý díl výukového seriálu o PHP. V tomto díle se podíváme na základní pravidla pro zápis proměnných.

Tato stránka slouží jen jako rychlý přehled. Pokud hledáte podrobný odborný popis všech vlastností, tak jsem napsal <a href="/promenna">samostatný článek</a>.

Základní syntaxe
--------------------------

Proměnné v PHP začínají znakem dolaru `$`, za kterým ihned následuje název.

```php
$zvire = 'kočka';
```

Řetězce (posloupnosti znaků) se uzavírají do uvozovek nebo apostrofů:

```php
$a = "uvozovky";
$b = 'apostrofy';
```

Číslice se nezavírají do uvozovek:

```php
$a = 5;
$b = 10;
$c = 3.14159;
```

Název promenné mohou tvořit jen znaky anglické abecedy a čísla. Název vždy začíná písmenem.

Pokud se název skládá z více slov, tak je zvyk používat `camelCase` syntaxi (první písmeno malé a každé další slovo začíná velkým):

```php
$kocka = 'koťáťko';
$rychlyPocitac = 'Jasně, že můj!';
$pocetRohuJednorozce = 1;
```


Název nesmí obsahovat mezery, pomlčky, háčky, čárky, uvozovky, závorky a další speciální znaky. Jediným povoleným speciálním znakem je `podtržítko`.

Desetinná čísla se zapisují s tečkou:

```php
$pi = 3.14159;
```


Dost často se může hodit provádět matematické operace přímo při definici proměnné:

```php
$a = 5;
$b = 3;
$c = $a + $b;	// sečte 5 + 3
echo $c;		// vypíše 8
```


Správné vložení uvozovky nebo apostrofu
--------------------------

Uvozovky a apostrofy nesmíme libovolně kombinovat. Pokud se rozhodneme použít například uvozovku, tak musím řetězec uvozovkou také ukončit a uvnitř ji nepoužít.

Toto je tedy špatně:

```php
echo "<img src="obrazek.gif">";
```


Protože není jednoznačné, kde začíná a končí řetězec. Uvozovky ani apostrofy proto není možné zanořovat.

Jednomu z možných řešení se říká <a href="/escapovani">escapování</a>, kdy se před problematický znak napíše zpětné lomítko (backslash).

```php
echo "<img src=\"obrazek.gif\">";
```


Backslash říká, že následující znak bude přesně ten, který chceme použít.

Pro výpis HTML kódu je ovšem výhodnější celý řetězec obalit do apostrofů a pak uvozovky používat běžným způsobem:

```php
echo '<img src="obrazek.gif">';
```


Případně to lze i otočit:

```php
echo "<img src='obrazek.gif'>";
```


Naplnění proměnné z url adresy nebo z formuláře
--------------------------

Adresy obsahující otazník nesou informace o vstupních proměnných, takže třeba `index.php?page=kontakty` označuje proměnnou `page` s hodnotou `kontakty`. Hodnota této proměnné se přečte jako `$_GET['page']`.

Znak otazníku nijak nesouvisí s názvem souboru na disku. Jedná se vždy o jeden a ten samý soubor, kterému v adrese předáváme parametry.

Podrobně tuto problematiku rozebírám v článku o <a href="/metody-odesilani-dat">metodách odesílání dat</a>.

Definice obsahu proměnné z adresy
--------------------------

Některé proměnné jsou dostupné už v okamžiku spuštění scriptu (a můžeme je proto používat rovnou), tím se říká **superglobální proměnné**. Pokud chceme například přečíst hodnotu z URL adresy, použijeme proměnnou `$_GET`.
Použití je následovné:

```php
$a = $_GET['a'];

echo $a;
```


Tento script vypíše do zdrojového kódu to, co má v URL adrese za otazníkem.

> Pozor, tato ukázka není bezpečná! Pokud by nečestný návštěvník do URL adresy předal například HTML kód, tak bude vložen do stránky a vykonán. Výpis musíme proto vždy ošetřit, k tomu se používá funkce `htmlspecialchars()`.

```php
$a = $_GET['a'];

echo htmlspecialchars($a);
```


> Pokud budeme přistupovat ke stránce bez uvedení parametru `?a=cokoli`, tak proměnná `$_GET['a']` nebude existovat a PHP vyhodí chybové hlášení. Tento stav musíme ošetřit podmínkou a v případě neexistence proměnné neprovádět nic (nebo alternativně vypsat alternativní obsah). Existenci proměnné je možné ověřit funkcí `isset()`.

```php
if (isset($_GET['a'])) {
	$a = $_GET['a'];

	echo htmlspecialchars($a);
} else {
	echo 'Proměnná "a" neexistuje!';
}
```


Příklad s počítáním
--------------------------

S proměnnými z URL adresy můžeme dělat psí kusy, třeba je sečíst a rovnou výsledek vypsat:

```php
echo $_GET['a'] + $_GET['b'];
```


Pokud chceme do URL adresy zapsat více vstupních parametrů, tak je musíme oddělit znakem ampersandu (`&`). Adresa tedy může vypadat například takto: `index.php?a=5&b=3`.

Propojení textových vstupů (řetězců)
--------------------------

Také můžeme snadno propojit 2 textové vstupy (řetězce). Dělá se to pomocí operátoru tečky. Propojovat můžete v proměnné nebo při výpisu.

```php
$a = 'pes';
$b = 'kočka';

echo $a . ' a ' . $b;
```


Vypíše `pes a kočka`.
