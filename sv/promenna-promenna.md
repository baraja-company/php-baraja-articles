Variabler Variabler
===================

> id: a0baeb3a-ab6e-4dd9-b1bc-b1872a12ee08
> slug:
> 	cs: promenna-promenna
> 	sv: variabler-variabler
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '590515cc711ec27cc0f0bfe190c8fbff'

> **Varning:** Den här artikeln skrevs för många år sedan och viss information kan vara föråldrad eller felaktig. Tänk på detta när du läser.

**Variabler** är inte avsedda för allmän användning (de löser problem som kan lösas på andra sätt), de används främst för att göra skrivningar mer kortfattade och minnesåtkomster mer komplexa.

Ta följande exempel:

```php
$x = 25;                  // innehåller 25
$nacitana_promenna = 'x'; // innehåller "x"
$y = $$nacitana_promenna; // innehåller 25
echo $y;                  // skriver ut 25
```

Lägg märke till de två dollar som följer efter varandra. I det här fallet kommer variabeln $y att laddas med värdet av den variabel som har det namn som finns i variabeln $nacitana_variable.

Lite förvirrande, eller hur? Det är därför det är bäst att inte använda variabler.
> **Note:** Variabla variabler är en specialitet i PHP på grund av dollartecknet. I andra språk markeras inte början av variabelnamnet med något tecken, så du kan inte använda variabla variabler eftersom det skulle vara tvetydigt när det är en klassisk variabel och när det inte är det.
