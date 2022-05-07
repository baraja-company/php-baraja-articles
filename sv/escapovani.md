Att undvika tecken i en sträng i PHP
====================================

> id: '40f9361e-e286-4b5e-a0c0-1f6cda8106af'
> slug:
> 	cs: escapovani
> 	sv: att-undvika-tecken-i-en-straeng-i-php
> 
> publicationDate: '2019-11-26 11:56:52'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '026284252541bc9c918a87298b9e261c'

Escaping används för att skriva tecken som har olika betydelser i olika sammanhang.

Vi vill till exempel infoga ytterligare ett citationstecken i en sträng som är omgiven av citationstecken. Hur gör man det?

Det finns 2 alternativ:

```php
echo "Levi's jeans"; // Kombination av typer av citationstecken

echo 'Levi\'s jeans'; // Backslash-utrymning
```

Escaping är också viktigt när du skriver variabler till en HTML-mall, där stränginnehållet kan vara i ett annat sammanhang och betyda något speciellt.

När vi till exempel listar HTML-kod (som vi har i en variabel) måste vi behandla listningen, annars kommer HTML-koden att köras.

Till exempel:

```php
$message = 'Hej <b>Tommy!</b>';

echo $message; // Fel!

echo htmlspecialchars($message); // Just det :)
```

Frågan om escaping är mycket komplex och jag rekommenderar att du läser artikeln <a href="https://phpfashion.com/escapovani-definitivni-prirucka">Escaping - The Definitive Guide</a> av David Grudel.
