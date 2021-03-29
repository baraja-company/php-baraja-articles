Metody odesílání dat (GET a POST)
=================================

> id: "32f9083f-7cb1-469f-911a-765df059123d"
> slugCS: metody-odesilani-dat
> perex: "Metoda GET a POST, získávání dat z formuláře a URL. Komunikace přes API a zpracování dat."
> publicationDate: "2019-11-26 11:38:32"
> mainCategoryId: "2a1ef8bc-14aa-438a-87e7-5b3f9643f325"

Kromě běžných proměnných máme v PHP k dispozici i tzv. **superglobální proměnné**, které nesou informace o aktuálně zavolané stránce a data, která předáváme.

Typicky máme na stránce formulář, do kterého nám uživatel cokoli vyplní a my chceme tato data přenést na webový server, kde je zpracujeme v PHP.

K tomu se nejčastěji používají 3 metody:

- `GET` ~ data se předávají v URL adrese jako parametry
- `POST` ~ data putují skrytě společně s požadavkem na stránku
- <a href="/ajax-post">Ajax POST</a> ~ asynchronní zpracování javascriptem

Metoda GET - `$_GET`
--------------------

Data odesílané metodou GET jsou vidět v URL adrese (jako parametry za otazníkem), maximální délka je v Internet Exploreru 1024 znaků (ostatní prohlížeče *téměř neomezují*, ale větší texty není vhodné takto předávat). Výhoda této metody je převážně v jednoduchosti (vidíte co odesíláte) a v možnosti uvést odkaz na výsledek zpracování. Data se odešlou do proměnné.

Adresa přijímací stránky může vypadat třeba takto:

`https://____________.com/script.php?promenna=obsah&promenna2=obsah`

V PHP poté můžeme například hodnotu parametru `promenna` vypsat takto:

```php
echo $_GET['promenna'];	// vypíše "obsah"
```

> **Důrazné varování:** Tento způsob vypisování dat přímo do HTML stránky není bezpečný, protože můžeme v URL adrese přenést například HTML kód, který by se vypsal do stránky a následně provedl.
>
> Data musíme před jakýmkoli výpisem do stránky **vždy ošetřit**, k tomu se používá funkce `htmlspecialchars()`.
>
> Například: `echo htmlspecialchars($_GET['promenna']);`

Metoda POST - `$_POST`
----------------------

Data odesílané metodou POST nejsou vidět v URL adrese, což řeší problém maximální délky odesílaných dat. Metodou POST bychom měli vždy odesílat formulářová pole, protože tím zabezpečíme, že nebudou vidět například hesla a na stránku se zpracováním výsledku konkrétního vstupu nepůjde uvést odkaz.

Data jsou k dispozici v proměnné `$_POST` a použití je stejné, jako u metody GET.

Ověření existence odeslaných dat
--------------------------------

Před zpracováním jakýchkoli dat bychom měli nejprve ověřit, že data byla skutečně odeslána, jinak bychom přistupovali
 k neexistující proměnné, což by vyhodilo chybové hlášení.

K ověření existence proměnné slouží funkce `isset()`.

```php
if (isset($_GET['jmeno'])) {
	echo 'Vaše jméno: ' . htmlspecialchars($_GET['jmeno']);
} else {
	echo 'Nebylo zadáno žádné jméno.';
}
```

Formulář pro vložení dat
------------------------

Formulář se dělá v HTML, nikoli v PHP. Může být i na obyčejné HTML stránce. O veškerou "magii" se stará až PHP script, který data přijme.

Pro ukázku nám může posloužit formulář pro přijmutí 2 čísel, odeslaný metodou GET:

```html
<form action="script.php" method="get">
	První číslo: <input type="text" name="x">
	Druhé číslo: <input type="text" name="y">

	<input type="submit" value="Sečíst čísla">
</form>
```

V prvním řádku je vidět, kam se budou data odesílat, a jakou metodou.

Na dalších 2 řádcích jsou jednoduché formulářové prvky, všimněte si atributu **name=""**, tam se píše jméno proměnné, do které se dosadí to, co je nyní ve formuláři.

Dále následuje tlačítko pro odeslání dat (povinné) a ukončovací HTML tag formuláře (povinné, aby prohlížeč poznal, co ještě odesílat a co ne).

> Na jedné stránce můžeme mít libovolné množství formulářů, přičemž je nelze do sebe zanořovat. Pokud k zanoření dojde, odesílá se vždy nejvíce zanořený a zbytek se ignoruje.

Zpracování formuláře na serveru
-------------------------------

Nyní máme již hotový HTML formulář a odesíláme ho na `script.php`, který data přijímá metodou GET. Adresa požadavku na stránku může vypadat třeba takto:

`https://________.com/script.php?x=5&y=3`

**script.php**

```php
$x = $_GET['x'];	// 5
$y = $_GET['y'];	// 3

echo $x + $y;		// vypíše 8
```

Správně bychom měli nejprve ověřit, že obě formulářová pole byla vyplněna, to se dělá funkcí `isset()`:

```php
if (isset($_GET['x']) && isset($_GET['y'])) {
	$x = $_GET['x'];	// 5
	$y = $_GET['y'];	// 3

	echo $x + $y;		// vypíše 8
} else {
	echo 'Formulář nebyl správně vyplněn.';
}
```

> **TIP:** Konstrukci `isset()` je možné předat více parametrů a tím ověřit, že všechny existují.
>
> Proto místo `isset($_GET['x']) && isset($_GET['y'])` je možno uvést jen:
>
> `isset($_GET['x'], $_GET['y'])`

Zpracování dat přijmutých metodou POST
--------------------------------------

Pokud data přijímáme metodou POST, tak URL adresa scriptu pro zpracování bude vypadat vždy takto:

`https://________.com/script.php`

A nikdy jinak. Prostě ne. Data jsou skryta v HTTP requestu a nevidíme je.

> Skrytě metodou POST je z důvodu bezpečnosti nutné odesílat přihlašovací jména a hesla.
>
> **Varování:** Pokud na webu pracujete s hesly, přihlašovací i registrační formulář by měl být umístěn na HTTPS a hesla musíte patřičně hashovat (například funkcí BCrypt).

Zpracování ajaxových požadavků
------------------------------

V některých případech při zpracování ajaxových požadavků nemusí být jednoduché data získat. Důvod je ten, že ajaxové knihovny obvykle odesílají data jako tzv. `json payload`, zatímco superglobální proměnná `$_POST` obsahuje pouze data z formulářů.

K datům se dá i přesto dostat, podrobnosti jsem popsal v článku <a href="/ajax-post">Zpracování ajaxových POST požadavků</a>.
