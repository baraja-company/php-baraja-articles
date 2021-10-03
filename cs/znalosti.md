Přehled znalostí o tvorbě webu
==============================

> id: e5e9613c-66fe-4d5e-a686-a182aab8061a
> slug:
> 	cs: znalosti
>
> publicationDate: "2019-09-11 10:07:07"
> mainCategoryId: "6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070"

Umět vytvořit webové stránky stránky a pak se o ně komplexně starat není jenom tak. Na cestě čeká mnoho překážek a je dobré mít o každé věci aspoň základní představu. Když jsem začínal, tak jsem nevěděl, co všechno se vlastně naučit. Tato stránka slouží jako rozcestník tématy, které jsem si postupně musel nastudovat, abych byl schopen tvorbu webu pochopit a poradit si s většinou běžných situací.

Správa serveru
--------------

> Webový server je počítač, na kterém běží web. Když si uživatel prohlíží stránku, tak mu ji posílá webový server.
>
> V současné době (2021) již nemá smysl pořizovat free hosting, pokud to s webem myslíme aspoň trochu vážně. Server lze **pronajmout již od 50 korun měsíčně**.

- Instalace serveru (liší se na Windows a Linuxu)
- Konfigurace serveru
	- Nastavení Apache
	- Nastavení PHP
	- <a href="/info">Zjištění aktuální konfigurace PHP</a> (funkce `phpinfo()`)
	- Routování v Ngnixu (zvyšuje výkon serveru)

Internet a internetový prohlížeč
--------------------------------

- Webové prohlížeče
- Princip request & response
	- URL adresa
	- Ajax a Ajaj
	- Generování HTML kódu (šablonovací systémy)

Stringy (řetězce)
-----------------

- Čtení, zápis a spojování řetězců, zejména základní práce s řetězci
- Zpracovná řetězců
	- Procházení znak po znaku
	- <a href="/if">Porovnávání řetězců</a>
	- Podobnost řetězců (funkce `levenshtein()`, `similar_text()` a `soundex()`)
	- <a href="/explode">Explode</a>, rozdělení dle oddělovače
	- **Regulární výrazy** rozdělují řetězce podle univerzálně nastavitelné masky
	- **Tokenizér** rozebere řetězce na malé části (tokeny)
- Serializace dat do stringu
	- <a href="/json">Json</a>, javascriptový objekt uložený do stringu
