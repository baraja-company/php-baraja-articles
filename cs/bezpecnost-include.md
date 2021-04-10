Bezpečnost include v PHP a připojení souborů
============================================

> id: "820f8de6-ff1e-406c-8fe5-95080642656f"
> slug:
> 	cs: bezpecnost-include
> 
> publicationDate: "2019-08-22 20:48:46"
> mainCategoryId: "3666a8a6-f2a3-405d-8263-bd53c4301fb3"

Často můžeme chtít připojit do stránky nějaký soubor, který máme někde jinde uložený na disku. Pokud zadáme jeho přesný název přímo do připojovací funkce, tak není co řešit.

Bezpečné připojení souboru
--------------------------

```php
include 'menu.html';
```


Předchozí zápis je naprosto bezpečný, protože vždy připojíme jeden a ten samý soubor. Bezpečnostní chyba v tomto případě nastat nemůže. Jediný problém, který může nastat je neexistence souboru **menu.html**, který vyvolá varovnou hlášku (která se stejně pravděpodobně nezobrazí), nicméně taková situace nastane jen vzácně, protože obvykle připojujeme soubor, u kterého je jeho existence prakticky jistá.

Připojení souboru dle vzoru
--------------------------

Co když ale chceme například připojovat články do jednoduchého obsahového webu, jako je třeba tento? Na tomto webu mám fyzicky složku, ve které jsou články uložené ve formátu HTML a připojuji je přímo do zdrojového kódu.

Nicméně prosté připojení nestačí! Začátečník může jednotlivé články volat například takto:

```php
include 'clanky/' . $_GET['clanek'] . '.html';
```

> Ale tohle je extrémně nebezpečné. Útočník může jako název článku předat odkaz do jiného adresáře používající `../` nebo něco podobného, někdy je možné se zbavit i koncovky předáním nulového bajtu na konci. Je potřeba použít alespoň funkci `basename()`, lépe ale povolit jen hodnoty z whitelistu.

Proč nenačítat nerelevantní soubory?
--------------------------

Často nám načtení chybného (neočekávaného) souboru vadit nemusí - je to chyba uživatele, že si žádá o stránku, kterou ve skutečnosti nechce, nicméně mohou nastat situace, kdy to vadit bude. Zejména tyto:

- Uživatel načte soubor, kam nemá veřejnost přístup a dostane se k němu jen server.
- Načtení nějakého jiného PHP scriptu může vyvolat nečekanou akci nebo chybovou hlášku, která může napovědět, jak web funguje a pomoci k dalším útokům.
- Načtení jiného souboru nemusí vyvolat pouze jeho přidání do dokumentu, ale také jeho spuštění.

Whitelist a validace vstupu
--------------------------

Pokud nemáme možnost ověření zadaných vstupů nějakým bezpečným způsobem (např. z whitelistu), tak bychom stejně neměli spoléhat na čestnost uživatele a scripty bránit aspoň na úrovni PHP.

První důležitou věcí je umístit všechny načítané soubory do stejné složky (adresáře) a zakázat některé nebezpečné znaky, zejména lomítko a tečku. Tím znemožníme přístup do jiných složek, kde jsou potenciálně napadnutelné soubory. Zakázání nebezpečných znaků lze udělat také prostým vymazáním ze vstupního řetězce.

```php
$load = '../index'; // tento vstup by mohl být potenciálně nebezpečný
$load = strtr($load, './', ''); // smaže všechny tečky a lomítka z řetězce
include $load .'.html';
```


Spuštění souborů?
--------------------------

Je důležité podotknout, že konstrukt (příkaz) **include** soubory po připojení spouští stejným způsobem, jako kdyby se jednalo o PHP kód, proto je dobré s touto možností počítat.

Často však budeme připojovat soubory, u kterých není jejich následné spuštění vyžadováno a zajímá nás jen uložený text (obsah) formou řetězce. Proto můžeme soubor načíst do proměnné a pracovat s ním jako s řetězcem, což je docela bezpečné.

```php
$load = '../index'; // tento vstup by mohl být potenciálně nebezpečný
$load = strtr($load, './', ''); // smaže všechny tečky a lomítka z řetězce
$file = file_get_contents($load . '.html'); // načtení obsahu do proměnné
echo $file; // vypsání obsahu souboru
```

Toto řešení vypadá zajímavě a na první pohled bezpečně - a také bezpečné je. I kdyby se uživateli povedlo zavolat PHP soubor, tak ho nikdy nespustí. Nicméně jej může zobrazit (myšleno jeho zdrojový kód) a na to si musíme dát pozor.

Rozpoznání chtěného souboru od scriptu
--------------------------

Na toto neexistuje jednoznačný návod, každý to musí udělat sám podle potřeb daného scriptu. Já například rozpoznávám článek od ostatních souborů podle toho, že obsahují nadpis velikosti H1. Pokud tedy někdo načte soubor, kde není nadpis, tak nic nezobrazím a stránka skončí chybovým hlášením. Vždy je důležité najít nějakou unikátní vlastnost, kterou mají právě jen chtěné soubory a kterou ostatní soubory nemají a od toho se lze dále odrazit.

Závěr
--------------------------

Ačkoli je validování a načítání souborů relativně jednoduché, tak v tom velké množství začátečníků stále chybuje - a chybovat bude. Nejdůležitější je, správně si uvědomit význam toho, co načítáme a jak chtěný obsah rozpoznat od toho ostatního. A hlavně, pracovat s obsahem jako s řetězcem a nikdy ho nenačítat přímo do stránky.
