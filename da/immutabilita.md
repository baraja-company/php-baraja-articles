Objektets uforanderlighed - et centralt designkoncept
=====================================================

> id: '057467db-4e3b-4e18-9ea5-dfb25feb3800'
> slug:
> 	cs: immutabilita
> 	da: objektets-uforanderlighed-et-centralt-designkoncept
> 
> publicationDate: '2022-07-24 15:00:00'
> mainCategoryId: ae4c1c70-11b3-433e-b1d0-e590155bb8b9
> sourceContentHash: '88c26f35883c860426b4708f0be8761f'

Uforanderlighed er et af de vigtigste designkoncepter til opbygning af stabile applikationer. Det grundlæggende princip er, at når først en tilstand er skrevet, kan den kun læses senere uden mulighed for at ændre den. Hvis vi skal ændre tilstanden, skal vi oprette en ny instans og erstatte hele objektet med et andet.

Datatyper kan derfor groft sagt opdeles i to store kategorier:

- **Mutabel** (foranderlig tilstand inden for en enkelt instans)
- **Immutable** (uforanderlig intern tilstand)

Mutable objekter kan ændres internt. Det vil sige, at de leverer operationer, der, når de kaldes i forskellige kombinationer, giver forskellige resultater. Uforanderlighed forsøger at forhindre denne adfærd.

Definition
--------

> En klasse er **immutabel**, hvis instansdataene ikke kan ændres på nogen måde, efter at instansen er oprettet.

Så alle data er fastlagt i konstruktøren. Alle skalare datatyper er også automatisk uforanderlige.

En stor fordel
--------------

Design af applikationer med uforanderlige tilstande giver en grundlæggende fordel med hensyn til sikkerheden ved udførelse af operationer. Hvis vi ved, at dataene ikke kan ændres (muteres) senere, når de først er skrevet, kan vi f.eks. fejlfinde meget pålideligt eller opdele programmet i underfunktioner uden risiko for at glemme nogen af de mellemliggende tilstande.

Idéen om uforanderlighed er generelt imod princippet om at lagre tilstande i egenskaber i objekter/enheder og beskriver snarere en funktionel tilgang, hvor data bare "flyder" gennem programmet, som f.eks. javascript gør.

Ud fra et ydelsesmæssigt perspektiv kan vi automatisk sige om uforanderlige objekter, at de kan lagres i cachen i det uendelige, fordi de aldrig vil være forældede.

Et konkret eksempel fra PHP
--------------------

Langt den mest almindelige anvendelse af uforanderlige objekter i PHP er objektet `DateTimeImmutable`, som efter at være oprettet kun kan kaldes af formateringsmetoder. Hvis vi ændrer de interne indstillinger, returnerer metoden en ny instans. Denne funktion er afgørende, når du bruger en ORM, der bruger det såkaldte identitetsmønster - den giver os f.eks. mulighed for at garantere, at når vi læser oprettelsesdatoen for en ordre, vil den være den samme overalt i programmet, og den referentielle integritet vil ikke blive ødelagt.

Et konkret eksempel på et foranderligt objekt:

```php
$date = new DateTime('2021-05-14');
$tomorrow = $date->modify('+1 dag');
echo $date->format('Y-m-d'); // 2021-05-15
echo $tomorrow->format('Y-m-d'); // 2021-05-15
```

Den samme dato blev udskrevet, fordi metoden `modify()` kun ændrede den interne tilstand af `DateTime`-objektet og returnerede den samme instans. Der var således ingen såkaldt **mutation af intern tilstand**, hvilket er en grundlæggende adfærd i objektorienteret programmering. Ved at opdatere variablen ændrede man også den oprindelige variabel.

Og nu et eksempel på et objekt, der ikke kan ændres:

```php
$date = new DateTimeImmutable('2021-05-14');
$tomorrow = $date->modify('+1 dag');
echo $date->format('Y-m-d'); // 2021-05-14
echo $tomorrow->format('Y-m-d'); // 2021-05-15
```

Objektet `DateTimeImmutable` er uforanderligt, hvilket betyder, at dets interne tilstand aldrig ændres. En ny modificeret instans (som også er uforanderlig) gemmes i variablen, efter at metoden `modify()` er blevet kaldt. Hvis vi ikke gemte den nye værdi i variablen, ville den ikke kunne bruges senere.

Den oprindelige værdi bliver aldrig rørt og forbliver sikkert opbevaret.

Hvornår skal en klasse være uforanderlig?
---------------------------

Medmindre du har en meget god grund til at gøre den foranderlig, skal du altid skrive en klasse eller funktion som uforanderlig. Dette vil forenkle dit design i fremtiden.

Mutable klasser bør ændres så lidt som muligt. Jeg anbefaler altid, at man dokumenterer opførslen af uforanderlighed.

Den eneste ulempe ved uforanderlighed er måske, at der skal oprettes en ny instans ved hver attributændring, hvilket har en mindre indvirkning på ydeevnen. Hvis dit program (som de fleste programmer) har tendens til at vise data og ændre dem sjældent, er denne ulempe ret ubetydelig i forhold til ydeevnen på nutidens computere.

Hvilke typer data bør være uforanderlige?
------------------------------------

- Alle identifikatorer og unikke koder
- De fleste ManyToOne- og OneToOne-databasesessioner
- Datoer, tidspunkter, kalenderværdier
- Cykler gennem arrays, hvor vi ønsker at udføre den samme operation på hvert element
- Intervaller, par, tripler, ...
- Geometriske figurer, punkter, linjer, GPS-koordinater, fysiske adresser, ...
- Logbøger og historiske optegnelser
- Oplysninger om behandlede ordrer og de fleste finansielle data
- Metadata om den relaterede enhed

Hvad bør ikke være uforanderligt:

- Store objekter med mange egenskaber
- De fleste tabeller fra en database, f.eks. Doctrine-enheder
- Progressivt konstruerede genstande af mindre dele
