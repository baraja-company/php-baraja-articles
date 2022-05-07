Escaping di caratteri in una stringa in PHP
===========================================

> id: '40f9361e-e286-4b5e-a0c0-1f6cda8106af'
> slug:
> 	cs: escapovani
> 	it: escaping-di-caratteri-in-una-stringa-in-php
> 
> publicationDate: '2019-11-26 11:56:52'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '026284252541bc9c918a87298b9e261c'

L'escape è usato per scrivere caratteri che hanno significati diversi in contesti diversi.

Per esempio, vogliamo inserire un'altra virgoletta in una stringa racchiusa tra virgolette. Come fare?

Ci sono 2 opzioni:

```php
echo "Jeans Levi's"; // Combinazione di tipi di virgolette

echo 'I jeans di Levi's'; // Backslash escaping
```

L'escape è anche importante quando si scrivono variabili in un template HTML, dove il contenuto della stringa può essere in un contesto diverso e significare qualcosa di speciale.

Quindi, per esempio, quando si elenca il codice HTML (che abbiamo in una variabile), dobbiamo trattare l'elenco, altrimenti il codice HTML verrà eseguito.

Per esempio:

```php
$message = 'Ciao <b>Tommy!</b>';

echo $message; // Sbagliato!

echo htmlspecialchars($message); // Giusto :)
```

La questione della fuga è molto complessa e vi consiglio di leggere l'articolo <a href="https://phpfashion.com/escapovani-definitivni-prirucka">Fuga - La guida definitiva</a> di David Grudel.
