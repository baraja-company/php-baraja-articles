Tlač
====

> id: '23d672c5-b347-43b8-bd08-8ccb7ecca33f'
> slug:
> 	cs: print
> 	sk: tlac
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: b7f7c3c32d9f3c1efd794ca38b7db384

`print` - Výstup reťazca

Popis
--------------------------

```php
vytlačiť 'Hello World!';
```

`print()` nie je v skutočnosti skutočná funkcia (je to jazykový konštrukt), takže nemusíte používať zátvorky.

Parametre
--------------------------

- **arg**

Výstupný parameter

Vrátené hodnoty
--------------------------

Vždy vráti číslo 1.

Poznámka
--------------------------

Poznámka: Keďže ide o **jazykovú konštrukciu** (nie funkciu), nemožno ju načítať do premennej.

Príklad
--------------------------

```php
vytlačiť "hello world";

print "print môže vypisovať viac riadkov textu.Ale pozor na značku HTML
pretože sa nevytlačí. Na to slúži funkcia <a href="/nl2br">Nl2br</a>.";

// Príklad pripojenia k premennej
$a = 'php';

print 'Páči sa mi ' . $a; // Mám rád php
```

**print** je presne tá istá funkcia ako **echo**. Ak hľadáte ďalšie informácie, pozrite si tento článok o funkcii <a href="/echo">echo</a>.
