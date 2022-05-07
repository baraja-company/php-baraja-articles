Formuláre, spracovanie formulárov v PHP
=======================================

> id: d1871a8d-bcfe-408d-ac12-6b827633f77e
> slug:
> 	cs: formulare-2
> 	sk: formulare-spracovanie-formularov-v-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: a23332c0-a233-4093-abd7-85b1b00a383b
> sourceContentHash: '91f43ee06328e88a6a1058ecbf028222'

Predpokladám, že sme vytvorili formulár HTML, ktorý sme odoslali, a teraz chceme spracovať údaje. O vytvorení formulára HTML existuje <a href="/formulare">samostatný článok</a>.

Prijímanie údajov - rôzne spôsoby
----------------------------

Spôsob odoslania formulára sa nastavuje priamo v HTML

Existujú 2 možnosti:

- **GET** - je viditeľný v adresnom riadku za otáznikom
 Napríklad: `php.baraja.cz/search.php?query=formulare`
- **POST** - Skrytý (neviditeľný), väčšina formulárov sa posiela poštou

Potom ich musíme prečítať v PHP rovnakou metódou.

Získanie údajov od používateľa a ich prenos do skriptu
------------------------------------------------------

Základom je HTML formulár, ako ho vytvoriť sa dočítate <a href="/formulare">v samostatnom článku</a>.

Na začiatok predpokladajme jednoduchý formulár na zadanie mena používateľa:

<form action="welcome.php" method="GET">
   Zadejte jméno: <input type="text" name="username">
   <input type="submit" value="odeslat">
</form>

Zobrazí sa textové pole, do ktorého zadajte meno a kliknite na tlačidlo Odoslať. Po kliknutí na tlačidlo sa obsah poľa odošle do skriptu `welcome.php`.

Teraz sa venujte samotnému spracovaniu v súbore `welcome.php`:

```php
$username = $_GET['username'];

echo 'Zadané meno je: ' . $username;
```

Všimnite si špeciálnu premennú `$_GET`. Ide o **nadglobálnu premennú**, ktorá obsahuje údaje z formulára a ku ktorej možno pristupovať ako k poľu.

Problémom tohto riešenia však je, že prijaté údaje nie sú **bezpečné** a podobný formulár možno ľahko napadnúť. Potenciálny útočník môže napríklad do poľa namiesto mena zadať javascriptový kód, ktorý sa zapíše na stránku a vykoná.

Preto musíme vždy upraviť všetky údaje používateľa pred ich odoslaním do kódu HTML:

```php
$username = $_GET['username'] ?? "Neznámy;

echo 'Zadané meno je: ' . htmlspecialchars($username);
```

Ďalšie spracovanie
----------------

S prijatými údajmi môžeme robiť čokoľvek a zaobchádzať s nimi ako s bežnou premennou.

Napríklad pridajte hodnotu do dvoch polí:

```php
echo $_GET['x'] + $_GET['y'];
```

Alebo uložte do súboru, databázy, e-mailu, ...

Na tento účel sú užitočné nasledujúce funkcie:

- <a href="/file-put-contents">file_put_contents</a> - funkcia na uloženie údajov do súboru
- <a href="/hashovani">MD5</a> - výpočet kontrolného súčtu, napríklad pre heslá
- <a href="/cookies">Cookies</a> - ukladanie údajov do **cookies** (malé súbory vo webovom prehliadači)
