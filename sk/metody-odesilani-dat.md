Metódy odosielania údajov (GET a POST)
======================================

> id: '32f9083f-7cb1-469f-911a-765df059123d'
> slug:
> 	cs: metody-odesilani-dat
> 	sk: metody-odosielania-udajov-get-a-post
> 
> perex: 'Metoda GET a POST, získávání dat z formuláře a URL. Komunikace přes API a zpracování dat.'
> publicationDate: '2019-11-26 11:38:32'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '2282538597ebfe95877ae0e005ddd352'

Okrem bežných premenných máme v PHP aj takzvané **superglobálne premenné**, ktoré nesú informácie o aktuálne volanej stránke a odovzdávaných údajoch.

Zvyčajne máme na stránke formulár, ktorý používateľ vyplní, a tieto údaje chceme preniesť na webový server, kde ich spracujeme v jazyku PHP.

Na tento účel sa najčastejšie používajú 3 metódy:

- `GET` ~ údaje sa odovzdávajú v URL ako parametre
- `POST` ~ údaje sa odovzdávajú skryto spolu s požiadavkou na stránku
- <a href="/ajax-post">Ajax POST</a> ~ asynchrónne spracovanie javascriptom

Metóda GET - `$_GET`
--------------------

Údaje odoslané metódou GET sú viditeľné v adrese URL (ako parametre za otáznikom), maximálna dĺžka je 1024 znakov v prehliadači Internet Explorer (ostatné prehliadače ju *takmer* neobmedzujú, ale väčšie texty by sa týmto spôsobom nemali odovzdávať). Výhodou tejto metódy je najmä jednoduchosť (vidíte, čo odosielate) a možnosť poskytnúť odkaz na výsledok spracovania. Údaje sa odošlú do premennej.

Adresa prijímajúcej stránky môže vyzerať takto:

`https://____________.com/script.php?promenna=obsah&promenna2=obsah`

V PHP potom môžeme napríklad zapísať hodnotu parametra `premenná` takto:

```php
echo $_GET['variable'];	// vypíše "content"
```

> **Dôrazné upozornenie:** Tento spôsob zápisu údajov priamo do stránky HTML nie je bezpečný, pretože v adrese URL môžeme odovzdať napríklad kód HTML, ktorý by sa zapísal do stránky a následne vykonal.
>
> Údaje musíme **vždy** spracovať pred akýmkoľvek výstupom na stránku, na čo sa používa funkcia `htmlspecialchars()`.
>
> Napríklad: `echo htmlspecialchars($_GET['variable']);`

Metóda POST - `$_POST`
----------------------

Údaje odoslané metódou POST nie sú viditeľné v adrese URL, čo rieši problém maximálnej dĺžky odosielaných údajov. Na odosielanie polí formulára by sa mala vždy používať metóda POST, pretože sa tak zabezpečí, že napríklad heslá nebudú viditeľné a nebude možné poskytnúť odkaz na stránku, ktorá spracúva výsledok konkrétneho vstupu.

Údaje sú k dispozícii v premennej `$_POST` a použitie je rovnaké ako pri metóde GET.

Overenie existencie predložených údajov
--------------------------------

Pred spracovaním akýchkoľvek údajov by sme mali najprv overiť, či boli údaje skutočne odoslané, inak by sme pristupovali k
 do neexistujúcej premennej, čo by spôsobilo chybové hlásenie.

Funkcia `isset()` sa používa na overenie existencie premennej.

```php
if (isset($_GET['name'])) {
    echo "Vaše meno: . htmlspecialchars($_GET['name']);
} else {
    echo "Nebolo zadané žiadne meno.;
}
```

Formulár na zadávanie údajov
------------------------

Formulár je vytvorený v jazyku HTML, nie v jazyku PHP. Môže to byť na obyčajnej stránke HTML. O všetky "kúzla" sa stará skript PHP, ktorý prijíma údaje.

Ako príklad môžeme použiť formulár na prijatie 2 čísel odoslaných metódou GET:

<form action="script.php" method="get">
    První číslo: <input type="text" name="x">
    Druhé číslo: <input type="text" name="y">

    <input type="submit" value="Sečíst čísla">
</form>

V prvom riadku môžete vidieť, kam sa údaje odošlú a akou metódou.

Ďalšie 2 riadky sú jednoduché prvky formulára, všimnite si atribút **name=""**, je tam názov premennej, ktorá bude obsahovať to, čo je teraz vo formulári.

Ďalej nasleduje tlačidlo na odoslanie údajov (povinné) a uzatváracia značka HTML formulára (povinná, aby prehliadač vedel, čo má ešte odoslať a čo nie).

> Na jednej stránke môžeme mať ľubovoľný počet formulárov, ktoré nemôžu byť vnorené. Ak sa vyskytne vnorenie, vždy sa odošle najviac vnorený formulár a ostatné sa ignorujú.

Spracovanie formulára na serveri
-------------------------------

Teraz máme hotový formulár HTML a odošleme ho do súboru `script.php`, ktorý získa údaje pomocou metódy GET. Adresa požiadavky na stránku môže vyzerať takto:

`https://________.com/script.php?x=5&y=3`

**script.php**

```php
$x = $_GET['x'];	// 5
$y = $_GET['y'];	// 3

echo $x + $y;		// vytlačí 8
```

Správne by sme mali najprv overiť, či boli vyplnené obe polia formulára, čo sa vykoná pomocou funkcie `isset()`:

```php
if (isset($_GET['x']) && isset($_GET['y'])) {
    $x = $_GET['x'];	// 5
    $y = $_GET['y'];	// 3

    echo $x + $y;		// vytlačí 8
} else {
    echo "Formulár nebol vyplnený správne.;
}
```

> **TIP:** Konštrukcii `isset()` môžete odovzdať viacero parametrov a overiť, či všetky existujú.
>
> Preto namiesto `isset($_GET['x']) && isset($_GET['y'])` stačí zadať:
>
> `isset($_GET['x'], $_GET['y'])`.

Spracovanie údajov prijatých metódou POST
--------------------------------------

Ak sa údaje prijímajú prostredníctvom POST, adresa URL spracovávaného skriptu bude vždy vyzerať takto:

`https://________.com/script.php`

A nikdy inak. Jednoducho nie. Údaje sú skryté v požiadavke HTTP a my ich nemôžeme vidieť.

> Na odosielanie používateľských mien a hesiel sa z bezpečnostných dôvodov vyžaduje skrytá metóda POST.
>
> **Bezpečnosť:** Ak na svojich stránkach pracujete s heslami, prihlasovací a registračný formulár by mal byť umiestnený na HTTPS a heslá musíte vhodne hashovať (napríklad pomocou BCrypt).

Spracovanie požiadaviek ajax
------------------------------

V niektorých prípadoch nemusí byť pri spracovaní požiadaviek ajax jednoduché získať údaje. Je to preto, že ajaxové knižnice zvyčajne posielajú údaje ako `json payload`, zatiaľ čo superglobálna premenná `$_POST` obsahuje iba údaje formulára.

K údajom sa dá stále pristupovať, podrobnosti som popísal v článku <a href="/ajax-post">Obsluha ajaxových POST požiadaviek</a>.
