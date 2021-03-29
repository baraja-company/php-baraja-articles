Hashování řetězců a hesel
=========================

> id: "7978bee8-62cc-4770-b15b-a8d08d1dcf34"
> slugCS: hashovani
> perex: "Hash není šifra! Metody hashování dat a hesel. MD5, SHA1, Bcrypt. Ověření hesla."
> publicationDate: "2019-09-11 10:13:30"
> mainCategoryId: "3666a8a6-f2a3-405d-8263-bd53c4301fb3"

Proces hashování (na rozdíl od šifrování) ze vstupu vytvoří takový výstup, z kterého již nelze odvodit původní řetězec.

Výborně se proto hodí pro ochranu citlivých řetězců, hesel a kontrolní součty.

Další příjemná vlastnost hashovacích funkcí je, že generují výstupy vždy se stejnou délkou a malá změna na vstupu vždy kompletně změní celý výstup.

Hashovací funkce
----------------

V PHP existuje mnoho hashovacích funkcí, ty důležité jsou:

- **Bcrypt: password_hash()** - Nejbezpečnější hashování hesel, výpočetně pomalá, používá vnitřní salt a hashuje iterativně.
- **md5()** - Velice rychlá funkce vhodná pro hashování souborů. Výstup má vždy 32 znaků.
- **sha1()** - Rychlá hashovací funkce pro hashování souborů, používá ji interně Git pro hashovací commitů. Výstup má vždy 40 znaků.

Zahashování
-----------

```php
$password = 'tajne-heslo';

echo password_hash($password); // Bcrypt
echo md5($password);
echo sha1($password);
```

> **Pozor:** Funkce`md5()` ani `sha1()` není vhodná k hashování hesel, protože je výpočetně jednoduché původní heslo odhalit, nebo si hesla aspoň předpočítat. Mnohem lepší je použít `bcrypt`, který pro hashování hesel vznikl.
>
> Web <a href="https://www.md5cracker.com/">md5cracker.com</a> obsahuje databázi kontrolních součtů (hashů), zkuste vyhledat třeba hash: `79c2b46ce2594ecbcb5b73e928345492`, jak je vidět, tak čisté `md5()` není zas tak bezpečné pro častá slova a hesla.

Jediné správné řešení: `Bcrypt + salt`
--------------------------------------

Na přednášce <a href="https://www.youtube.com/watch?v=F58_A5TM-Sc">Jak to nepokazit v cílové rovince</a> řešil David Grudl způsoby, jak správně hashovat a ukládat hesla.

Jediné správné řešení je: `Bcrypt + salt`.

Konkrétně:

```php
$password = 'hash';

// Vygeneruje bezpečný hash
echo password_hash($password, PASSWORD_BCRYPT);

// Případně s vyšší složitostí (výchozí je 10):
echo password_hash($password, PASSWORD_BCRYPT, ['cost' => 12]);
```

Výhoda Bcrypu je hlavně v jeho rychlosti a automatickém solení.

Díky tomu, že generování trvá **dlouho**, třeba 100 ms, tak je pro útočníka velmi drahé testovat mnoho hesel.

Výstupní hash je navíc automaticky ošetřen pomocí **náhodné sole**, díky které při opakovaném hashování stejného hesla je na výstupu vždy jiný hash. Útočník proto nebude moci použít předpočítanou tabulku hashů.

Správnost hesla proto nebudeme moci ověřit opakovaným hashováním, ale bude potřeba zavolat specializovanou funkci:

```php
if (password_verify($password, $hash)) {
    // Heslo je správně
} else {
    // Heslo není správně
}
```

Solení hesel
------------

Aby bylo prolomení hashů složitější, je dobrý nápad do originálního vstupu vkládat nějaký další řetězec. V ideálním případě náhodný. Tomuto procesu se říká **solení hesel**.

Bezpečnost je založena na myšlence, že útočník nebude moci použít předpočítanou tabulku hesel a hashů, protože nebude znát sůl a bude muset hesla lámat jednotlivě.

Například:

```php
$password = 'tajne_heslo';
$salt = 'fghjgtzjjhg';

$hash = md5($password . $salt);

echo $password; // vypíše původní heslo
echo $hash;     // vypíše hash hesla včetně soli
```

Složené hashovací funkce
------------------------

Možná vás napadá, že by byl dobrý nápad hashovací funkci provést opakovaně a tím zvednout složitost prolomení, protože bude potřeba originální heslo opakovaně hashovat.

Například:

```php
$password = 'heslo';

for ($i = 0; $i <= 1000; $i++) {
    $password = md5($password);
}

echo $password; // 1000x zahashováno přes md5()
```

Tímto postupem se náročnost prolomení paradoxně snižuje, nebo zůstává téměř stejná.

Důvod je ten, že funkce `md5()` je extrémně rychlá a na běžné počítači lze spočítat přes milion hashů za sekundu, proto se postupné zkoušení hesel o moc nezpomalí.

Druhý důvod je spíše teorie, a to možnost narazit na tzv. kolizi. Pokud budeme heslo opakovaně hashovat, časem se může stát, že se trefíme do hashe, který už útočník zná a díky tomu bude moci heslo promolit pomocí databáze.

Lepší je proto použít pomalou bezpečnou hashovací funkci a provádět hashování pouze jednou, přičemž se finální výstup ještě ošetří solením.
