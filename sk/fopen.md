Funkcia PHP fopen()
===================

> id: '733ba757-1d5f-4717-a088-5ddabe730838'
> slug:
> 	cs: fopen
> 	sk: funkcia-php-fopen
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '6b791877e57d88840d603dea69967b69'

Funkcia `fopen()` predstavuje nízkoúrovňový prístup k súborom na disku.

Programátor musí všetko urobiť sám (otvorenie súboru, čítanie údajov, zápis nových údajov, zatvorenie súboru).

Ak potrebujete len rýchlo čítať a zapisovať súbory, existujú jednoduchšie možnosti:

- <a href="/file-get-contents">file_get_contents()</a> - čítanie zo súboru
- <a href="/file-put-contents">file_put_contents()</a> - zápis do súboru

Základné použitie
----------------

```php
$text = 'Akýkoľvek text, ktorý sa má uložiť...';

$file = fopen('file.html', 'a+'); // Otvorí súbor a režim

fwrite($file, $text); // Uloží do súboru
fclose($file); // Zatvorí súbor
```

> Ak otvoríme súbor na čítanie a ten nie je uzavretý, žiadny iný proces k nemu nemôže pristupovať!

Typy režimov spracovania súborov
----------------------------

So súbormi môžeme pracovať v rôznych režimoch, ktoré informujú o prístupových právach.

Ak chceme napríklad otvoriť súbor len na čítanie, postačí režim `r`.

Ak súbor otvoríme na zápis, bude na disku označený ako `otvorený` a iný proces (skript) doň nebude môcť zapisovať, kým ho opäť nezatvoríme. Tým sa zabezpečí, že sa súbor počas zápisu nepoškodí.

| Režim | Význam |
|-------|--------|
| `and` | Otvorí súbor, ak neexistuje, vytvorí sa |
| `a+` | Otvorí súbor na pridanie údajov alebo čítanie údajov, ak neexistuje, vytvorí sa |
| `r` | Otvoriť len na čítanie |
| | `r+` | Otvoriť na čítanie a zápis |
| `w` | Otvoriť na zápis, pôvodné údaje budú vymazané a nahradené novými, ak neexistujú, budú vytvorené |
| `w+` | Otvoriť pre zápis a čítanie, pôvodné údaje budú vymazané a nahradené novými, ak neexistujú, budú vytvorené |
