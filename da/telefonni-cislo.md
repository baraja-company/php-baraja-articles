Validering og formatering af telefonnumre
=========================================

> id: bbfe35c3-3a8c-4fe5-a9bd-5bff0fc234e0
> slug:
> 	cs: telefonni-cisla
> 	da: validering-og-formatering-af-telefonnumre
> 
> publicationDate: '2021-06-18 09:20:00'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: '224b3857a6bb898d9d322499cd1d3757'

Der er ingen nem måde at validere og formatere telefonnumre på i PHP, så jeg skrev et simpelt bibliotek, som ikke har nogen afhængigheder, men som stadig kan håndtere denne rolle.

Målet er at kontrollere formatet af et telefonnummer eller at konvertere det til en grundlæggende kanonisk form (som altid er gyldig).

Installation af
---------

Simpelthen efter komponist:

```txt
$ composer require baraja-core/phone-number
```

Du kan også downloade pakken [Download on GitHub] (https://github.com/baraja-core/phone-number).

Sådan bruger du biblioteket
----------

Princippet i dette værktøj er baseret på formatering og validering af telefonnumre fra brugeren eller fra kilder, som du ikke har kontrol over.

Den mest almindelige anvendelse er at rette formateringen af telefonnumre:

```php
$original = '+420 777123456';
$formatted = \Baraja\PhoneNumber\PhoneNumberFormatter::fix($original);

echo $original . '<br>';
echo $formatted;
```

Funktionen korrigerer talformateringen og returnerer strengen `+420 777 123 456`.

Hvis brugeren ikke har angivet noget præfiks, antages præfikset `+420`. Du kan ændre standardpræferencen med den anden parameter:

```php
// returnerer: +421 777 123 456
\Baraja\PhoneNumber\PhoneNumberFormatter::fix('+420 777123456', 421);
```

Telefonkoden overskrives kun, hvis brugeren ikke indtaster den, og den ikke opdages automatisk.

Formatering af input og output
----------------------------

Indtastningsstrengen kan se ud på (næsten) alle måder. Den indbyggede algoritme kan automatisk fjerne ugyldige tegn (nogle brugere skriver f.eks. en note ved siden af et telefonnummer, som automatisk fjernes), så du behøver slet ikke at bekymre dig om at formatere input, men output vil altid være konsistent.

Output ser altid ens ud (det er normaliseret til det kanoniske format).

Det generelle format er:

```txt
   +420 777 123 456
     |  \_________/
  Prefix     |
      National number
```

Hvis du indsender et input, der ikke er gyldigt (eller et input, der ikke kan korrigeres automatisk), vil der blive sendt en undtagelse.

Fældefangst af fejl
----------------

Hvis et tal ikke kan normaliseres sikkert til en basisform eller ikke findes, skal du kaste en `\InvalidArgumentException`-undtagelse.

Hvis du ønsker at konvertere undtagelsen til en boolean, skal du bruge den indbyggede validator for aktiver:

```php
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('123'); // falsk
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('777123456'); // sandt
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('+420 777123456'); // sandt
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('+420 777 123 456'); // sandt
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('+420 77 712 34 56'); // sandt
```
