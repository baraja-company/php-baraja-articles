Jak napsat první PHP script
================================

> id: 2d6986dc-f24b-4ae5-b24c-d5bb9bf94512
> slugCS: prvni-script
> perex: Jak napsat první PHP script? Úvod do PHP pro začátečníky.
> publicationDate: 2020-02-16 16:26:08
> mainCategoryId: f4a34087-1b51-4761-8128-4459dfe83d8a

Než napíšeme náš první PHP script, tak si nejdříve musíme teoreticky vysvětlit, jak probíhá načítání stránky s použitím PHP.

1. Nejprve uživatel ve svém webovém prohlížeči zavolá nějakou konkrétní URL, například `https://baraja.cz`
2. Následně webový prohlížeč sestaví tzv. `request`, což je speciální požadavek pro webový server, který se pošle do internetu. Obsahuje informace o žádané stránce, základní identifikaci prohlížeče a nastavení, informace o cookies a podobně.
3. Tento `request` doputuje internetem až na webový server (nejčastěji `Apache`), který požadavek přečte a začne sestavovat odpověď.
4. Protože se podle volané URL adresy jedná o PHP script a žádáme soubor s názvem `index.php`, tak `Apache` načte z kořenového adresáře na disku soubor `index.php` a předá ho do `PHP interpretu`, což je program, který umí zpracovat samotný PHP kód a na základě něj sestavit `HTML kód`, který se pošle zpět uživateli.
5. Po sestavení HTML kódu se odpověď pošle zpět uživateli (tomu se říká `response`) a webový prohlížeč stránku vykreslí běžným způsobem, jako by se jednalo o čisté HTML.

> Všimněte si, že se webový prohlížeč nedozví vůbec nic o obsahu PHP scriptu, ale zpracovává až vygenerované HTML, takže vaše scripty a obsah serveru zůstávají v bezpečí.

Pojďme vytvořit první script
----------------------------

> Napsání prvního scriptu předpokládá, že máte v počítači rozchozený webový server. Pro Windows je nejlepší <a href="https://www.apachefriends.org/index.html">XAMPP</a> (stáhněte si verzi PHP 7.0. nebo novější), na Macu funguje <a href="https://www.apachefriends.org/index.html">XAMPP</a> úplně stejně, jako na Windows. Pro Linux doporučuji <a href="https://wiki.ubuntu.cz/servery/apache_s_mysql_a_php">LAMP server</a> (na Lamp serveru běží i tento web).

Název souboru s PHP scriptem musí končit příponou `.php`, aby webový server poznal, že ho chceme zpracovat podle pravidel jazyka PHP. Pojďme si tedy založit například soubor `index.php`, který bude obsahovat kód hlavní stránky našeho webu.

Otevření souboru
--------------------------

Otevřte tento soubor ve vhodném textovém editoru pro psaní zdrojového kódu.

Ve Windows je pro začátek vhodný například <a href="https://www.sublimetext.com">Sublime Text</a>, který hezky obarvuje syntaxi (pravidla jazyka) a kód je tedy lépe čitelný. Později doporučuji zakoupit program <a href="https://www.jetbrains.com/phpstorm/">PhpStorm</a>, který se hodně používá ve firmách a nabízí možnost programování ve více lidech.

Napište základní strukturu HTML stránky
--------------------------

Základní kopyto HTML stránky asi už znáte:

```html
<!DOCTYPE HTML>
<html>
	<head>
	<title>Můj první PHP script</title>
	<meta charset="UTF-8">
	</head>
	<body>

	</body>
</html>
```


Veškerý HTML kód bude zpracován běžným způsobem a pro designování stránek bude našim velkým pomocníkem. PHP využívá principů HTML a CSS ve velké míře.

Oddělení PHP scriptu od HTML kódu
--------------------------

PHP je hlavně šablonovací jazyk, který na vhodná místa v kódu generuje vlastní obsah. Abychom mohli jednoznačně říct, co je HTML a co PHP, musíme použít oddělovací značku.

V současné době je nejlepší použít zápis s `<?php` a `?>`.

```php
<?php
	// tady bude PHP kód
?>
```

> Ukončovací značku `?>` uvádíme v případě, že chceme použít ještě nějaký další HTML kód. Pokud na konci PHP scriptu již žádný další HTML kód nenásleduje, tak je výhodnější značku `?>` neuvádět, aby na konci stránky nebyly zbytečné bílé znaky (mezery a odřádkování), které tam může vložit textový editor.

V minulosti se hodně používala značka `<?` místo `<?php`, ale nemusí být vždy podporována.

Obalovací značky můžeme umístit na libovolné místo v HTML kódu, třeba do těla stránky:

```html
<!DOCTYPE HTML>
<html>
	<head>
	<title>Můj první PHP script</title>
	<meta charset="UTF-8">
	</head>
	<body>
	<?php
		// tady bude PHP kód
	?>
	</body>
</html>
```


Základní konstrukty
--------------------------

Mezi úplně nejzákladnější stavební kameny patří zejména:

- <a href="/echo">Výpis obsahu do zdrojového kódu</a>
- <a href="/promenna">Proměnné</a>
- <a href="/if">Podmínky</a>

V této kapitole si ukážeme jednoduchý výpis obsahu do zdrojového kódu za pomoci proměnné.

Princip výstavby zdrojového kódu
--------------------------------

Všechny konstrukty (výrazy jazyka), příkazy a funkce se oddělují středníky, aby bylo možné jednoznačně určit, odkud a kam má aktuální konstrukt platnost.

Za středníkem obvykle následuje odřádkování.

Symbolicky zapsáno:

```php
příkaz;
další příkaz;
proměnná x = její hodnota;

vypsat proměnnou x;

uložit do souboru;
```


Výpis do zdrojového kódu
--------------------------

K vypisování obsahu slouží konstrukt <a href="/echo">echo</a>.

Použití je velice snadné:

```php
echo 'Ahoj světe!';
```


Do HTML kódu poté vypíše text "Ahoj světe!". Ukázku si vyzkoušejte.

> Všechny další ukázky budou obsahovat jen vnitřek PHP kódu. Okolní HTML kód si můžete libovolně domyslet (například podle ukázky na začátku článku).

Proměnné
--------------------------

Proměnné jsou virutální místa paměti, do kterých se ukládají data a slouží k jejich přesunům. Název proměnné začíná vždy `dolarem`, následuje samotný `název` a poté její `hodnota`.

> Detailní popis fungování proměnných jsem shrnul v <a href="/promenna">samostatném článku o proměnných</a>.

```php
$oblibeneCislo = 1024;
$jmenoAutora = 'Jan Barášek';

echo $oblibeneCislo;
echo '<br>';
echo $jmenoAutora;
```


> Název proměnné by měl vyjadřovat, co proměnná reálně obsahuje, aby byl kód přehlednější. Také si všimněte vložení HTML značky `<br>` pro odřádkování textu. Tuto značku byste již měli znát z jazyka HTML.

Tomu, co se vypisuje v konstruktu `echo` se říká řetězec (posloupnost znaků). Jednotlivé řetězce lze spojovat tečkou (`.`), čímž můžeme zkrátit výpis na jeden řádek:

```php
$oblibeneCislo = 1024;
$jmenoAutora = 'Jan Barášek';

echo $oblibeneCislo . '<br>' . $jmenoAutora;
```


> Po spojení řetězců tečkou se bude na celou věc nahlížet jako na jeden velký řetězec.

Operace s proměnnými
--------------------------

Mezi proměnnými fungují všechny základní <a href="/matematika">matematické operace</a> naprosto intuitivně dle očekávání.

Pojďme si definovat 2 proměnné a dosadit do nich čísla:

```php
$x = 5; // definuje proměnnou x s hodnotou 5
$y = 3; // definuje proměnnou y s hodnotou 3

echo $x + $y; // sečte proměnné a vypíše 8
```


> Všimněte si, že k provedení matematické operace se nepoužívá znak rovnítka (`=`), proto nelze zapsat například rovnice. PHP se v tomto směru chová jen jako kalkulačka.

Pokud bychom nechtěli použít proměnné, tak můžeme operace provádět přímo. Na místě provádění operací tedy nezáleží a vyhodnotí se kdekoli.

```php
echo 5 + 3; // vypíše 8
```


Případně můžeme proměnné sečíst a výsledek uložit do jiné proměnné:

```php
$x = 5;
$y = 3;
$z = $x + $y; // proměnná $z obsahuje 8

echo $z; // vypíše 8
```

V dalším díle se naučíme úplné základy <a href="/zasady-promennych">definice proměnných a jejich použití</a>.
