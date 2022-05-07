Convalida e formattazione dei numeri di telefono
================================================

> id: bbfe35c3-3a8c-4fe5-a9bd-5bff0fc234e0
> slug:
> 	cs: telefonni-cisla
> 	it: convalida-e-formattazione-dei-numeri-di-telefono
> 
> publicationDate: '2021-06-18 09:20:00'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: '224b3857a6bb898d9d322499cd1d3757'

Non c'è un modo semplice per convalidare e formattare i numeri di telefono in PHP, così ho scritto una semplice libreria che non ha dipendenze, ma può comunque gestire questo ruolo.

L'obiettivo è controllare il formato di un numero di telefono, o convertirlo in una forma canonica di base (che è sempre valida).

Installazione di
---------

Semplicemente per compositore:

```txt
$ composer require baraja-core/phone-number
```

Oppure scaricate il pacchetto [Download on GitHub](https://github.com/baraja-core/phone-number).

Come usare la biblioteca
----------

Il principio di questo strumento si basa sulla formattazione e la convalida dei numeri di telefono dall'utente, o da fonti su cui non avete controllo.

L'uso più comune è quello di correggere la formattazione dei numeri di telefono:

```php
$original = '+420 777123456';
$formatted = \Baraja\PhoneNumber\PhoneNumberFormatter::fix($original);

echo $original . '<br>';
echo $formatted;
```

La funzione corregge la formattazione del numero e restituisce la stringa `+420 777 123 456`.

Se nessun prefisso è specificato dall'utente, si assume il prefisso `+420`. Potete cambiare la preferenza predefinita con il secondo parametro:

```php
// ritorna: +421 777 123 456
\Baraja\PhoneNumber\PhoneNumberFormatter::fix('+420 777123456', 421);
```

Il codice telefonico viene sovrascritto solo se l'utente non lo inserisce e non viene rilevato automaticamente.

Formattazione di ingresso e uscita
----------------------------

La stringa di input può apparire (quasi) in qualsiasi modo. L'algoritmo incorporato può rimuovere automaticamente i caratteri non validi (per esempio, alcuni utenti scrivono una nota accanto a un numero di telefono che verrà rimossa automaticamente). Quindi non devi preoccuparti affatto di formattare l'input, ma l'output sarà sempre coerente.

L'output ha sempre lo stesso aspetto (è normalizzato al formato canonico).

Il formato generale è:

```txt
   +420 777 123 456
     |  \_________/
  Prefix     |
      National number
```

Se si invia un input non valido (o un input che non può essere corretto automaticamente), verrà lanciata un'eccezione.

Trapping degli errori
----------------

Se un numero non può essere normalizzato in modo sicuro in una forma base, o non esiste, lancia un'eccezione `InvalidArgumentException`.

Se volete convertire l'eccezione in un booleano, usate il validatore di risorse integrato:

```php
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('123'); // falso
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('777123456'); // vero
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('+420 777123456'); // vero
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('+420 777 123 456'); // vero
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('+420 77 712 34 56'); // vero
```
