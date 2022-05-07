Validering och formatering av telefonnummer
===========================================

> id: bbfe35c3-3a8c-4fe5-a9bd-5bff0fc234e0
> slug:
> 	cs: telefonni-cisla
> 	sv: validering-och-formatering-av-telefonnummer
> 
> publicationDate: '2021-06-18 09:20:00'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: '224b3857a6bb898d9d322499cd1d3757'

Det finns inget enkelt sätt att validera och formatera telefonnummer i PHP, så jag skrev ett enkelt bibliotek som inte har några beroenden, men som ändå kan hantera den här rollen.

Målet är att kontrollera formatet för ett telefonnummer eller konvertera det till en grundläggande kanonisk form (som alltid är giltig).

Installation av
---------

Helt enkelt efter kompositör:

```txt
$ composer require baraja-core/phone-number
```

Du kan också hämta paketet [Download on GitHub] (https://github.com/baraja-core/phone-number).

Hur du använder biblioteket
----------

Principen för det här verktyget bygger på att formatera och validera telefonnummer från användaren eller från källor som du inte har någon kontroll över.

Den vanligaste användningen är att korrigera formateringen av telefonnummer:

```php
$original = '+420 777123456';
$formatted = \Baraja\PhoneNumber\PhoneNumberFormatter::fix($original);

echo $original . '<br>';
echo $formatted;
```

Funktionen korrigerar talformateringen och returnerar strängen `+420 777 123 456`.

Om användaren inte anger något prefix antas prefixet `+420`. Du kan ändra standardinställningen med den andra parametern:

```php
// Returnerar: +421 777 123 456
\Baraja\PhoneNumber\PhoneNumberFormatter::fix('+420 777123456', 421);
```

Telefonkoden skrivs över endast om användaren inte anger den och den inte upptäcks automatiskt.

Formatering av in- och utdata
----------------------------

Inmatningssträngen kan se ut (nästan) hur som helst. Den inbyggda algoritmen kan automatiskt ta bort ogiltiga tecken (vissa användare skriver till exempel en anteckning bredvid ett telefonnummer som tas bort automatiskt). Du behöver alltså inte oroa dig för att formatera inmatningen alls, men resultatet kommer alltid att vara konsekvent.

Utgången ser alltid likadan ut (den är normaliserad till det kanoniska formatet).

Det allmänna formatet är:

```txt
   +420 777 123 456
     |  \_________/
  Prefix     |
      National number
```

Om du skickar in en icke giltig inmatning (eller en inmatning som inte kan korrigeras automatiskt) kommer ett undantag att visas.

Fällande av fel
----------------

Om ett tal inte kan normaliseras säkert till en basform, eller om det inte existerar, ska du kasta ett undantag `\InvalidArgumentException`.

Om du vill omvandla undantaget till en boolsk siffra kan du använda den inbyggda validatorn för tillgångar:

```php
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('123'); // falskt
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('777123456'); // sant
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('+420 777123456'); // sant
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('+420 777 123 456'); // sant
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('+420 77 712 34 56'); // sant
```
