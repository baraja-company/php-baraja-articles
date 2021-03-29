Print
=====

> id: "23d672c5-b347-43b8-bd08-8ccb7ecca33f"
> slugCS: print
> publicationDate: "2019-08-22 20:48:46"
> mainCategoryId: "6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070"

`print` - Výstup řetězce

Popis
--------------------------

```php
print 'Ahoj světe!';
```

`print()`, není ve skutečnosti reálná funkce (je to jazykový konstrukt), takže nemusíte používat závorky.

Parametry
--------------------------

- **arg**

Parametr výstupu

Návratové hodnoty
--------------------------

Vždy vrací číslo 1.

Poznámka
--------------------------

Poznámka: Protože se jedná o **jazykový konstrukt** (není to funkce), tak nelze načíst do proměnné.

Příklad
--------------------------

```php
print "ahoj světe";

print "print umí vypsati více řádkový text.Ale pozor na HTML značku
, ta se totiž nevypíše. Od toho je funkce <a href="/nl2br">Nl2br</a>.";

// Ukázka spojení s proměnnou
$a = 'php';

print 'Mám rád ' . $a; // Mám rád php
```

**print** je naprosto stejná funkce jako **echo**. Pokud hledáte více informací, přečtěte si článek o funkci <a href="/echo">echo</a>.
