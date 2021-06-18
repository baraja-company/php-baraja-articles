Validace a formátování telefonních čísel
========================================

> id: bbfe35c3-3a8c-4fe5-a9bd-5bff0fc234e0
> slug:
> 	cs: telefonni-cisla
> 
> publicationDate: "2021-06-18 09:20:00"
> mainCategoryId: "1f73dcfa-92a9-4738-ab30-8cbfb00ad23b"

V PHP neexistuje jednoduchý způsob, jak validovat a formátovat telefonní čísla, proto jsem pro to napsal jednoduchou knihovnu, která nemá žádné závislosti, ale přesto zvládne tuto roli obsloužit.

Cílem je zkontrolovat formát telefonního čísla, případně ho převést na základní kanonický tvar (který je vždy validní).

Instalace
---------

Jednoduše composerem:

```
$ composer require baraja-core/phone-number
```

Nebo si balík [Stáhněte na GitHubu](https://github.com/baraja-core/phone-number).

Jak knihovnu použít
----------

Princip tohoto nástroje je založen na formátování a validaci telefonních čísel od uživatele, nebo ze zdrojů, nad kterými nemáte kontrolu.

Nejčastější použití je oprava formátování telefonního čísla:

```php
$original = '+420 777123456';
$formatted = \Baraja\PhoneNumber\PhoneNumberFormatter::fix($original);

echo $original . '<br>';
echo $formatted;
```

Funkce opraví formátování čísla a vrátí řetězec `+420 777 123 456`.

Pokud uživastel nezadá žádnou předvolbu, předpokládá se předvolba `+420`. Výchozí předvolbu můžete změnit druhým parametrem:

```php
// returns: +421 777 123 456
\Baraja\PhoneNumber\PhoneNumberFormatter::fix('+420 777123456', 421);
```

Telefonní předvolba se přepíše jen v případě, kdy ji uživatel nezadá a nepodaří se detekovat automaticky.

Formátování vstupu a výstupu
----------------------------

Vstupní řetězec může vypadat (téměř) libovolným způsobem. Vestavěný algoritmus umí automaticky odebrat nevalidní znaky (například někteří uživatelé k telefonnímu číslu píší poznámku, která bude automaticky odebrána). Formátování vstupu tedy nemusíte vůbec řešit, výstup ale bude vždy konzistentní.

Výstup vypadá vždy stejně (je normalizován na kanonický tvar).

Obecný formát je:

```
   +420 777 123 456
     |  \_________/
  Prefix     |
      National number
```

Pokud předáte nevalidní vstup (nebo vstup, který nelze automaticky opravit), bude vyhozena výjimka.

Zachytávání chyb
----------------

Pokud číslo nelze bezpečně normalizovat na základní tvar, nebo neexistuje, vyhazujeme výjimku `\InvalidArgumentException`.

Pokud si přejete převést výjimku na boolean, použijte vestavěný asset validátor:

```php
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('123'); // false
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('777123456'); // true
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('+420 777123456'); // true
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('+420 777 123 456'); // true
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('+420 77 712 34 56'); // true
```
