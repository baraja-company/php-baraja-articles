Medtag sikkerhed i PHP og vedhæftning af filer
==============================================

> id: '820f8de6-ff1e-406c-8fe5-95080642656f'
> slug:
> 	cs: bezpecnost-include
> 	da: medtag-sikkerhed-i-php-og-vedhaeftning-af-filer
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '1207d637c20fcb5e8f609be2eb449135'

Ofte ønsker vi at vedhæfte en fil til en side, som vi har gemt på en disk et andet sted. Hvis vi indtaster det nøjagtige navn direkte i attach-funktionen, er der intet at bekymre sig om.

Sikker vedhæftning af en fil
--------------------------

```php
include 'menu.html';
```

Den tidligere skrivning er helt sikker, fordi vi altid monterer den samme fil. Der kan ikke opstå en sikkerhedsfejl i dette tilfælde. Det eneste problem, der kan opstå, er, at filen **menu.html** ikke findes, hvilket vil udløse en advarselsmeddelelse (som sandsynligvis ikke vil blive vist alligevel), men denne situation er sjælden, fordi vi normalt vedhæfter en fil, hvis eksistens er så godt som sikker.

Vedhæftning af en fil i henhold til mønsteret
--------------------------

Men hvad nu, hvis vi f.eks. ønsker at vedhæfte artikler til et simpelt indholdssite som dette? På dette websted har jeg en fysisk mappe, hvor artiklerne er gemt i HTML-format, og jeg vedhæfter dem direkte til kildekoden.

Men det er ikke nok blot at oprette forbindelse! En nybegynder kan kalde de enkelte artikler på denne måde:

```php
include 'artikler/' . $_GET['Artikel'] . '.html';
```

> Men det er ekstremt farligt. En angriber kan sende et link til en anden mappe ved at bruge `../` eller noget lignende som artikelnavn, og nogle gange er det muligt at slippe af med enden ved at sende en null-byte i slutningen. Du bør i det mindste bruge funktionen `basename()`, men det er bedre kun at tillade whitelist-værdier.

Hvorfor ikke indlæse ikke-relevante filer?
--------------------------

Ofte har vi ikke noget imod at indlæse en forkert (uventet) fil - det er brugerens skyld, fordi han/hun anmoder om en side, som han/hun faktisk ikke ønsker, men der kan være situationer, hvor det har betydning. Især:

- Brugeren indlæser en fil, som offentligheden ikke kan få adgang til, og som kun serveren kan få adgang til.
- Indlæsning af et andet PHP-script kan udløse en uventet handling eller fejlmeddelelse, som kan give et hint om, hvordan webstedet fungerer, og hjælpe til yderligere angreb.
- Indlæsning af en anden fil kan ikke blot medføre, at den tilføjes til dokumentet, men også at den kører.

Whitelist og validering af input
--------------------------

Hvis vi ikke har mulighed for at validere input på en sikker måde (f.eks. fra en whitelist), bør vi ikke stole på brugerens ærlighed og forsvare scripts, i hvert fald ikke på PHP-niveau.

Den første vigtige ting er at placere alle indlæste filer i den samme mappe (mappe) og deaktivere nogle farlige tegn, især skråstreg og prik. Det vil gøre det umuligt at få adgang til andre mapper, der indeholder potentielt sårbare filer. Du kan også deaktivere farlige tegn ved blot at slette dem fra inputstrengen.

```php
$load = '../index'; // dette input kan være potentielt farligt
$load = strtr($load, './', ''); // sletter alle prikker og skråstreger fra strengen
include $load .'.html';
```

Kørende filer?
--------------------------

Det er vigtigt at bemærke, at **include**-konstruktionen udfører filerne, når de er forbundet, på samme måde som hvis de var PHP-kode, så det er en god idé at tage højde for denne mulighed.

Ofte vil vi imidlertid vedhæfte filer, som ikke skal udføres efterfølgende, og vi er kun interesseret i den lagrede tekst (indhold) i form af en streng. Derfor kan vi indlæse filen i en variabel og arbejde med den som en streng, hvilket er ganske sikkert.

```php
$load = '../index'; // dette input kan være potentielt farligt
$load = strtr($load, './', ''); // sletter alle prikker og skråstreger fra strengen
$file = file_get_contents($load . '.html'); // indlæsning af indholdet i variablen
echo $file; // dump indholdet af filen
```

Denne løsning ser interessant og sikker ud ved første øjekast - og det er den også. Selv hvis det lykkes brugeren at kalde en PHP-fil, vil den aldrig blive kørt. Den kan dog vise den (dvs. dens kildekode), og det skal vi være forsigtige med.

Genkendelse af den ønskede fil fra scriptet
--------------------------

Der findes ikke nogen endelig vejledning til dette, men enhver må gøre det selv i overensstemmelse med manuskriptets behov. Jeg kan f.eks. genkende en artikel fra andre filer ved, at de indeholder en overskrift af størrelsen H1. Så hvis nogen indlæser en fil, hvor der ikke er nogen overskrift, viser jeg ikke noget, og siden ender med en fejlmeddelelse. Det er altid vigtigt at finde nogle unikke funktioner, som kun de ønskede filer har, og som de andre filer ikke har, og så kan du gå videre derfra.

Konklusion
--------------------------

Selv om det er relativt enkelt at validere og indlæse filer, begår mange nybegyndere stadig fejl - og vil fortsat begå fejl. Det vigtigste er at få styr på betydningen af det, vi indlæser, og hvordan vi kan skelne det indhold, vi ønsker, fra resten. Og vigtigst af alt, arbejd med indholdet som en streng og indlæs det aldrig direkte i siden.
