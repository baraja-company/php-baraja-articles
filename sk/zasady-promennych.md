Zásady zápisu premenných
========================

> id: '4c8e631b-b305-4169-8885-4f9506155999'
> slug:
> 	cs: zasady-promennych
> 	sk: zasady-zapisu-premennych
> 
> publicationDate: '2020-02-16 16:26:08'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '5528d9b73c1d1c07c330a58e4aeaa06a'

Toto je druhá časť série návodov o PHP. V tejto časti sa pozrieme na základné pravidlá zápisu premenných.

Táto stránka je len stručným prehľadom. Ak hľadáte podrobný technický opis všetkých funkcií, napísal som <a href="/variable">samostatný článok</a>.

Základná syntax
--------------------------

Premenné v PHP začínajú znakom dolára `$`, za ktorým bezprostredne nasleduje názov.

```php
$zvire = 'cat';
```

Reťazce (sekvencie znakov) sú uzavreté v úvodzovkách alebo apostrofoch:

```php
$a = "úvodzovky";
$b = "apostrofy;
```

Číslice sa neuvádzajú v úvodzovkách:

```php
$a = 5;
$b = 10;
$c = 3.14159;
```

Názov premennej môže pozostávať len zo znakov anglickej abecedy a číslic. Názov sa vždy začína písmenom.

Ak sa názov skladá z viac ako jedného slova, je zvykom používať syntax `camelCase` (prvé písmeno malé a každé ďalšie slovo začína veľkým písmenom):

```php
$kocka = 'kitty';
$rychlyPocitac = "Samozrejme, že je môj!;
$pocetRohuJednorozce = 1;
```

Názov nesmie obsahovať medzery, pomlčky, háčiky, čiarky, úvodzovky, zátvorky ani iné špeciálne znaky. Jediný povolený špeciálny znak je `podčiarknutie`.

Desatinné čísla sa píšu s bodkou:

```php
$pi = 3.14159;
```

Pri definovaní premennej môže byť často užitočné vykonávať matematické operácie priamo:

```php
$a = 5;
$b = 3;
$c = $a + $b;	// pridajte 5 + 3
echo $c;		// vytlačí 8
```

Správne vloženie úvodzoviek alebo apostrofu
--------------------------

Úvodzovky a apostrofy sa nesmú ľubovoľne kombinovať. Ak sa napríklad rozhodneme použiť úvodzovky, musíme nimi aj ukončiť reťazec a nepoužiť ich vo vnútri.

Preto je to nesprávne:

```php
echo "<img src="obrazek.gif">";
```

Pretože nie je jasné, kde sa reťaz začína a kde končí. Úvodzovky a apostrofy sa nemôžu vkladať.

Jedno z možných riešení sa nazýva <a href="/escapovani">escapovanie</a>, kde problematickému znaku predchádza spätné lomítko.

```php
echo "<img src="image.gif\">";
```

Spätné lomítko hovorí, že nasledujúci znak bude presne ten, ktorý chceme použiť.

Pre výstup kódu HTML je však vhodnejšie uzavrieť celý reťazec do apostrofov a potom použiť úvodzovky bežným spôsobom:

```php
echo '<img src="image.gif">';
```

Prípadne ho možno obrátiť:

```php
echo "<img src='image.gif'>";
```

Vyplnenie premennej z adresy url alebo z formulára
--------------------------

Adresy obsahujúce otáznik nesú informácie o vstupných premenných, takže napríklad `index.php?page=contacts` označuje premennú `page` s hodnotou `contacts`. Hodnota tejto premennej sa načíta ako `$_GET['page']`.

Znak otáznika nijako nesúvisí s názvom súboru na disku. Je to vždy ten istý súbor, ktorému odovzdávame parametre v adrese.

Tejto problematike sa podrobne venujem vo svojom článku o <a href="/methods-odesilani-dat">metódach odosielania údajov</a>.

Definovanie obsahu premennej z adresy
--------------------------

Niektoré premenné sú k dispozícii už v čase spustenia skriptu (a preto ich možno hneď použiť), nazývajú sa **superglobálne premenné**. Ak chceme napríklad prečítať hodnotu z adresy URL, použijeme premennú `$_GET`.
Použitie je nasledovné:

```php
$a = $_GET['a'];

echo $a;
```

Tento skript vypíše do zdrojového kódu to, čo má v adrese URL za otáznikom.

> Varovanie, táto vzorka nie je bezpečná! Ak by nepoctivý návštevník zadal do adresy URL napríklad kód HTML, vložil by sa do stránky a vykonal. Preto musíme výstup vždy ošetriť; na to sa používa funkcia `htmlspecialchars()`.

```php
$a = $_GET['a'];

echo htmlspecialchars($a);
```

> Ak pristupujeme na stránku bez zadania parametra `?a=anything`, premenná `$_GET['a']` nebude existovať a PHP vyhodí chybové hlásenie. Túto podmienku musíme ošetriť podmienkou a nerobiť nič, ak premenná neexistuje (prípadne vypísať alternatívny obsah). Existenciu premennej možno overiť pomocou funkcie `isset()`.

```php
if (isset($_GET['a'])) {
    $a = $_GET['a'];

    echo htmlspecialchars($a);
} else {
    echo 'Premenná "a" neexistuje!';
}
```

Príklad s počítaním
--------------------------

S premennými z adresy URL môžeme robiť psie kusy, napríklad ich sčítať a priamo zapísať výsledok:

```php
echo $_GET['a'] + $_GET['b'];
```

Ak chceme do adresy URL zahrnúť viac vstupných parametrov, musíme ich oddeliť ampersandom (`&`). Adresa môže vyzerať takto: `index.php?a=5&b=3`.

Prepojenie textových vstupov (reťazcov)
--------------------------

Môžeme tiež ľahko prepojiť 2 textové vstupy (reťazce). Toto sa vykonáva pomocou operátora bodka. Odkaz môžete uviesť v premennej alebo pri výpise.

```php
$a = "pes;
$b = 'cat';

echo $a . ' a ' . $b;
```

Je tam napísané "pes a mačka".
