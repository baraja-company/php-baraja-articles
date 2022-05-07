Parole chiave PHP
=================

> id: d58620d7-57a0-410a-ba73-31a1f9d984fb
> slug:
> 	cs: keywordy-jazyka-php
> 	it: parole-chiave-php
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '38fbdb53b4455903f3300c3c8b4a8ad2'

Quasi tutti i linguaggi di programmazione sono composti da "parole chiave", che sono espressioni speciali del linguaggio che hanno un significato speciale.

Un esempio di parola chiave è la parola `string`, `if` o `echo`.

È importante notare che le parole chiave (a volte anche i "comandi") <a href="/comandi-e-funzioni">non sono funzioni</a>, quindi anche `echo`, per esempio, non è una funzione.

L'elenco delle parole chiave è buono da sapere perché hanno significati speciali in PHP e non possono essere usate sempre per tutto. Per esempio, il nome di una classe non deve essere lo stesso di una delle parole chiave esistenti.

Esempio di errore di analisi
-------------------

Quando si tenta di processare una classe chiamata `String`, si verifica un `Errore Parsing` perché PHP non sa se si tratta di un nome di classe o di un tipo di dati:

Questo è sbagliato:

```php
class String
{
   // ...
}
```

Elenco delle parole chiave
-------------------

Aggiornato l'elenco di parole chiave per PHP 7.1:

`and`, `or`, `xor`, `for`, `do`, `while`, `foreach`, `as`, `return`, `die`, `exit`, `if`, `then`, `else`, `elseif`, `new`, `delete`, `try`, `throw`, `catch`, `finally`, `class`, `function`, `string`, `array`, `object`, `resource`, `var`, `bool`, `boolean`, `int`, `integer`, `float`, `double`, `real`, `string`, `array`, `global`, `const`, `static`, `public`, `private`, `protected`, `published`, `estende`, `switch`, `true`, `false`, `null`, `void`, `this`, `self`, `struct`, `char`, `signed`, `unsigned`, `short`, `long`
