Overovanie a formátovanie telefónnych čísel
===========================================

> id: bbfe35c3-3a8c-4fe5-a9bd-5bff0fc234e0
> slug:
> 	cs: telefonni-cisla
> 	sk: overovanie-a-formatovanie-telefonnych-cisel
> 
> publicationDate: '2021-06-18 09:20:00'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: '224b3857a6bb898d9d322499cd1d3757'

V PHP neexistuje jednoduchý spôsob overovania a formátovania telefónnych čísel, preto som napísal jednoduchú knižnicu, ktorá nemá žiadne závislosti, ale napriek tomu dokáže túto úlohu zvládnuť.

Cieľom je skontrolovať formát telefónneho čísla alebo ho previesť do základného kanonického tvaru (ktorý je vždy platný).

Inštalácia stránky
---------

Jednoducho podľa skladateľa:

$ composer require baraja-core/phone-number

Alebo si stiahnite balík [Download on GitHub](https://github.com/baraja-core/phone-number).

Ako používať knižnicu
----------

Princíp tohto nástroja je založený na formátovaní a overovaní telefónnych čísel od používateľa alebo zo zdrojov, nad ktorými nemáte kontrolu.

Najčastejšie sa používa na opravu formátovania telefónneho čísla:

```php
$original = '+420 777123456';
$formatted = \Baraja\PhoneNumber\PhoneNumberFormatter::fix($original);

echo $original . '<br>';
echo $formatted;
```

Funkcia opraví formátovanie čísla a vráti reťazec `+420 777 123 456`.

Ak používateľ nezadá žiadnu predponu, predpokladá sa predpona `+420`. Predvolené preferencie môžete zmeniť pomocou druhého parametra:

```php
// vráti: +421 777 123 456
\Baraja\PhoneNumber\PhoneNumberFormatter::fix('+420 777123456', 421);
```

Telefónny kód sa prepíše len vtedy, ak ho používateľ nezadá a nepodarí sa ho automaticky rozpoznať.

Vstupné a výstupné formátovanie
----------------------------

Vstupný reťazec môže vyzerať (takmer) ľubovoľne. Zabudovaný algoritmus dokáže automaticky odstrániť neplatné znaky (napríklad niektorí používatelia píšu pri telefónnom čísle poznámku, ktorá sa automaticky odstráni). Nemusíte sa teda vôbec starať o formátovanie vstupu, ale výstup bude vždy konzistentný.

Výstup vyzerá vždy rovnako (je normalizovaný do kanonického formátu).

Všeobecný formát je nasledovný:

   +420 777 123 456
     |  \_________/
  Prefix     |
      National number

Ak zadáte neplatný vstup (alebo vstup, ktorý nemožno automaticky opraviť), vyhodí sa výnimka.

Zachytávanie chýb
----------------

Ak číslo nemožno bezpečne normalizovať na základný tvar alebo neexistuje, vyhodí výnimku `\InvalidArgumentException`.

Ak chcete previesť výnimku na logickú hodnotu, použite vstavaný validátor aktív:

```php
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('123'); // false
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('777123456'); // true
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('+420 777123456'); // true
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('+420 777 123 456'); // true
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('+420 77 712 34 56'); // true
```
