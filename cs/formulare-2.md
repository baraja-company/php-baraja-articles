Formuláře, zpracování formulářů v PHP
================================

> id: d1871a8d-bcfe-408d-ac12-6b827633f77e
> slugCS: formulare-2
> publicationDate: "2019-08-22 20:48:46"
> mainCategoryId: a23332c0-a233-4093-abd7-85b1b00a383b

Předpokládám, že máme vytvořený HTML formulář, který odesíláme a nyní chceme data zpracovat. O vytvoření HTML formuláře pojednává <a href="/formulare">samostatný článek</a>.

Přijmutí dat - různé způsoby
----------------------------

Jakým způsobem se bude formulář odesílat, se nastavuje přímo v HTML

Jsou 2 možnosti:

- **GET** - Je vidět v adresním řádku za otazníkem
 Například: `php.baraja.cz/search.php?query=formulare`
- **POST** - Skrytě (není vidět), přes post se posílá většina formulářů

Stejnou metodou je pak musíme v PHP přečíst.

Získání dat od uživatele a jejich přenesení do scriptu
------------------------------------------------------

Základem je HTML formulář, jak ho udělat se dočtete <a href="/formulare">v samostatném článku</a>.

Pro začátek předpokládejme jednoduchý formulář pro zadání jména uživatele:

```html
<form action="welcome.php" method="GET"> 
   Zadejte jméno: <input type="text" name="username">
   <input type="submit" value="odeslat">
</form>
```


Zobrazí se textové pole pro zadání jména a tlačíto pro odeslání. Po kliknutí na tlačítko se obsah pole odešle na script `welcome.php`.

Nyní k samotnému zpracování v souboru `welcome.php`:

```php
$username = $_GET['username'];

echo 'Zadané jméno je: ' . $username;
```


Všimněte si speciální proměnné `$_GET`. Jedná se o tzv. **superglobální proměnnou**, která obsahuje data z formuláře a lze k ní přistupovat jako k poli.

Problém tohoto řešení ovšem je, že přijmutá data **nejsou zabezpečena** a na podobný formulář můžeme snadno zaútočit. Potenciální útočník může do pole místo jména zadat například javascriptový kód, který bude vypsán do stránky a spuštěn.

Proto musíme jakákoli uživatelská data vždy před výstupem do HTML kódu ošetřit:

```php
$username = $_GET['username'] ?? 'Neznámé';

echo 'Zadané jméno je: ' . htmlspecialchars($username);
```

Další zpracování
----------------

S přijmutými daty můžeme dělat úplně cokoli a zacházet s nimi jako s jakoukoli běžnou proměnnou.

Například sečíst hodnotu ve dvou polích:

```php
echo $_GET['x'] + $_GET['y'];
```

Nebo uložit do souboru, databáze, poslat mailem, ...

K tomu se vám hodějí následující funkce:

- <a href="/file-put-contents">file_put_contents</a> - funkce pro uložení dat do souboru
- <a href="/hashovani">MD5</a> - výpočet kontrolního součtu, například pro hesla
- <a href="/cookies">Cookies</a> - uložení dat do **cookies** (malé soubory uvnitř webového prohlížeče)
