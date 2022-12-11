Funktioner flag/funktion til/fra-knapper
========================================

> id: '4209c32f-655c-46fa-9090-e5ec3883ea61'
> slug:
> 	cs: feature-flagy
> 	da: funktioner-flag-funktion-til-fra-knapper
> 
> publicationDate: '2022-12-11 15:00:00'
> mainCategoryId: '28a2aef7-7490-43a9-add7-80d8a051f8a9'
> sourceContentHash: '1e32c312eab2ca16539cdb7431e2dda9'

Når du udvikler et mere komplekst program, vil du sætte pris på muligheden for at udvikle flere funktioner på forhånd, distribuere dem med den næste version af din software og aktivere funktionen senere.

Det er præcis det, som funktionsflagene blev skabt til. Denne artikel viser dig, hvordan du kan bruge dem.

Grundlæggende gennemførelse
---------------------

Featureflag er grundlæggende et meget simpelt koncept, der går ud på at kalde en enkelt funktion/metode, der afgør, om en ny funktion er aktiv.

For eksempel:

```php
echo '<h1>Vejr-apps</h1>';
echo 'I dag er det:' . getWeather();

if (feature('kort')) {
   echo 'Kort:' . getMap();
}
```

For at kontrollere, om en bestemt nyhed er tilgængelig, kaldes funktionen `feature()`, som beslutter, om den kan tillade eller ignorere den pågældende funktion baseret på kaldsnavnet.

Gennemførelse af beslutningslogikken
-------------------------------

Beslutningslogikken er ofte kompleks. Du kan f.eks. kun køre en bestemt funktion fra en bestemt dato eller for brugere i en bestemt gruppe. Jeg tester f.eks. ofte implementeringen af en ny funktion på f.eks. 5 % af brugerne på denne måde, så den ikke påvirker alle på en gang.

Når vi f.eks. udvikler corporate sotfware, er det sådan, vi kører reklamekampagner og rabatter, der er gyldige fra en bestemt dato.

Hvis en bestemt ny funktion går i stykker, er det muligt blot at deaktivere den med et funktionsflag for brugerne og aktivere den for en gruppe udviklere, som vil teste den og f.eks. bringe en rettelse.
