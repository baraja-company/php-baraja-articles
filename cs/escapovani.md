Escapování znaků v řetězci v PHP
================================

> id: "40f9361e-e286-4b5e-a0c0-1f6cda8106af"
> slugCS: escapovani
> publicationDate: "2019-11-26 11:56:52"
> mainCategoryId: "6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070"

Escapování slouží pro zápis znaků, které mají v různém kontextu různý význam.

Například chceme do řetězce obalený uvozovkami vložit další uvozovku. Jak na to?

Existují 2 možnosti:

```php
echo "Džíny Levi's"; // Kombinace typů uvozovek

echo 'Džíny Levi\'s'; // Escapování zpětným lomítkem
```

Escapování je důležité provádět také při výpisu proměnných do HTML šablony, kde může být obsah řetězce v jiném kontextu a znamenat něco speciálního.

Proto například při výpisu HTML kódu (který máme v proměnné) musíme výpis ošetřit, jinak by se HTML kód spustil.

Například:

```php
$message = 'Ahoj <b>Tomáši!</b>';

echo $message; // Špatně!

echo htmlspecialchars($message); // Správně :)
```

Problematika escapování je velmi složitá a doporučuji přečíst článek <a href="https://phpfashion.com/escapovani-definitivni-prirucka">Escapování - definitivní příručka</a> od Davida Grudla.
