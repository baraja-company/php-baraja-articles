Unikanie znaków w łańcuchu w PHP
================================

> id: '40f9361e-e286-4b5e-a0c0-1f6cda8106af'
> slug:
> 	cs: escapovani
> 	pl: unikanie-znakow-w-lancuchu-w-php
> 
> publicationDate: '2019-11-26 11:56:52'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '026284252541bc9c918a87298b9e261c'

Ucieczka jest używana do pisania znaków, które mają różne znaczenia w różnych kontekstach.

Na przykład chcemy wstawić kolejny znak cudzysłowu do łańcucha zawartego w cudzysłowie. Jak to zrobić?

Dostępne są 2 opcje:

```php
echo "Dżinsy Levi's"; // Kombinacja typów cudzysłowów

echo 'Dżinsy Leviego'; // Ucieczka przed odwrotnym ukośnikiem
```

Ucieczka jest również ważna przy zapisywaniu zmiennych do szablonu HTML, gdzie zawartość łańcucha może występować w innym kontekście i oznaczać coś szczególnego.

Dlatego, na przykład, podczas wypisywania kodu HTML (który mamy w zmiennej), musimy potraktować wypisywanie, gdyż w przeciwnym razie kod HTML zostanie wykonany.

Na przykład:

```php
$message = 'Cześć <b>Tommy!</b>';

echo $message; // Nieprawda!

echo htmlspecialchars($message); // Prawo :)
```

Zagadnienie ucieczki jest bardzo złożone i polecam przeczytanie artykułu <a href="https://phpfashion.com/escapovani-definitivni-prirucka">Escaping - The Definitive Guide</a> Davida Grudela.
