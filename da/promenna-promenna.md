Variabler Variabler
===================

> id: a0baeb3a-ab6e-4dd9-b1bc-b1872a12ee08
> slug:
> 	cs: promenna-promenna
> 	da: variabler-variabler
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '590515cc711ec27cc0f0bfe190c8fbff'

> **Varsel:** Denne artikel blev skrevet for mange år siden, og nogle oplysninger kan være forældede eller forkerte. Vær opmærksom på dette, når du læser.

**Variabler** er ikke beregnet til almindelig anvendelse (de løser problemer, der kan løses på andre måder), de bruges primært til at gøre skrivninger mere kortfattede og hukommelsestilgange mere komplekse.

Tag følgende eksempel:

```php
$x = 25;                  // indeholder 25
$nacitana_promenna = 'x'; // indeholder "x"
$y = $$nacitana_promenna; // indeholder 25
echo $y;                  // udskriver 25
```

Læg mærke til de to dollars, der følger efter hinanden. I dette tilfælde vil variablen $y blive indlæst med værdien af den variabel, der har det navn, som er indeholdt i $nacitana_variable.

Lidt forvirrende, ikke sandt? Det er derfor, at du hellere ikke skal bruge variabler.
> **Note:** Variable variabler er en specialitet i PHP på grund af dollartegnet. I andre sprog er begyndelsen af variabelnavnet ikke markeret med et tegn, så du kan ikke bruge variable variabler, fordi det ville være tvetydigt, hvornår det er en klassisk variabel, og hvornår det ikke er det.
