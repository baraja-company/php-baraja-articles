Prehľad znalostí o vývoji webových stránok
==========================================

> id: e5e9613c-66fe-4d5e-a686-a182aab8061a
> slug:
> 	cs: znalosti
> 	sk: prehlad-znalosti-o-vyvoji-webovych-stranok
> 
> publicationDate: '2019-09-11 10:07:07'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: b7284b1fc11b40ddb7628995070198a1

Vedieť, ako vytvoriť webovú stránku a potom sa o ňu komplexne starať, nie je len otázka jej vytvorenia. Na ceste je veľa prekážok a je dobré mať aspoň základnú predstavu o každej veci. Keď som začínal, nevedel som, čo všetko sa mám naučiť. Táto stránka slúži ako rozcestník tém, ktoré som si musel postupne naštudovať, aby som pochopil vývoj webových stránok a dokázal riešiť najčastejšie situácie.

Správa servera
--------------

> Webový server je počítač, na ktorom beží web. Keď používateľ zobrazí stránku, webový server mu ju odošle.
>
> V súčasnosti (2021) už nemá zmysel získavať bezplatný hosting, ak to s webom myslíte vážne. Server si môžete **prenajať od 50 korún mesačne**.

- Inštalácia servera (líši sa v systémoch Windows a Linux)
- Konfigurácia servera
	- Nastavenia Apache
	- Nastavenie PHP
	- <a href="/info">zistenie aktuálnej konfigurácie PHP</a> (funkcia `phpinfo()`)
	- Smerovanie Ngnix (zlepšuje výkon servera)

Internet a webový prehliadač
--------------------------------

- Webové prehliadače
- Princíp žiadosti a odpovede
	- Adresa URL
	- Ajax a Ajaj
	- Generovanie kódu HTML (šablónovacie systémy)

Struny
-----------------

- Čítanie, zápis a spájanie reťazcov, najmä základná práca s reťazcami
- Spracovanie reťazcov
	- Prechádzanie znak po znaku
	- <a href="/if">porovnávanie reťazcov</a>
	- Podobnosť reťazcov (funkcie `levenshtein()`, `similar_text()` a `soundex()`)
	- <a href="/explode">Explode</a>, rozdelenie oddeľovačom
	- **Pravidelné výrazy** rozdeľujú reťazce podľa univerzálne konfigurovateľnej masky
	- **Tokenizer** rozdeľuje reťazce na malé časti (tokeny)
- Serializácia údajov do reťazca
	- <a href="/json">Json</a>, javascriptový objekt uložený v reťazci
