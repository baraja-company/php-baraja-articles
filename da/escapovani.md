Undgåelse af tegn i en streng i PHP
===================================

> id: '40f9361e-e286-4b5e-a0c0-1f6cda8106af'
> slug:
> 	cs: escapovani
> 	da: undgaelse-af-tegn-i-en-streng-i-php
> 
> publicationDate: '2019-11-26 11:56:52'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '026284252541bc9c918a87298b9e261c'

Escaping bruges til at skrive tegn, der har forskellige betydninger i forskellige sammenhænge.

Vi ønsker f.eks. at indsætte endnu et anførselstegn i en streng, der er omgivet af anførselstegn. Hvordan gør man det?

Der er 2 muligheder:

```php
echo "Levi's jeans"; // Kombination af typer af anførselstegn

echo 'Levi\'s jeans'; // Undgåelse af backslash
```

Escaping er også vigtigt, når du skriver variabler til en HTML-skabelon, hvor strengindholdet kan være i en anden kontekst og betyde noget særligt.

Derfor skal vi f.eks. ved oplistning af HTML-kode (som vi har i en variabel) behandle oplistningen, da HTML-koden ellers vil blive udført.

For eksempel:

```php
$message = 'Hej <b>Tommy!</b>';

echo $message; // Forkert!

echo htmlspecialchars($message); // Ja :)
```

Spørgsmålet om flugt er meget komplekst, og jeg anbefaler at læse artiklen <a href="https://phpfashion.com/escapovani-definitivni-prirucka">Escaping - The Definitive Guide</a> af David Grudel.
