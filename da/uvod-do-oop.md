Introduktion til objektorienteret programmering i PHP
=====================================================

> id: '44a79461-dcfd-4dd0-b6fd-ba5db76db6de'
> slug:
> 	cs: uvod-do-oop
> 	da: introduktion-til-objektorienteret-programmering-i-php
> 
> publicationDate: '2020-01-01 19:54:35'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '888db2cf35845892eebade9fdcc18871'

Velkommen til den første artikel i onlinekurset OOP i PHP. Du kan finde en komplet liste over artikler på <a href="/oop">oversigtssiden</a>.

> **Indholdsnoter:**
>
> Målet med denne serie er at **forklare essensen** af objektorienteret programmering bedst muligt, så du ikke behøver at bruge hundredvis af timer på at eksperimentere med ting, der ikke giver mening. Alle teknikker og tutorials er fra praksis og er forklaret, som jeg selv ville have ønsket at læse dem, da jeg lærte at programmere. Nogle **begreber er stærkt forenklede** på bekostning af 100 % faktuel korrekthed, fordi jeg mener, at det altid er **bedre at forstå principperne** og i det mindste have en idé om problemerne end at fortabe sig i programmering og se det hele som et stort rod.
>
> Som du snart vil se, har programmering i OOP store fordele for din kode. Det giver dig et nyt abstraktionsniveau for din applikation og gør det muligt for dig at løse meget komplekse problemer, som ikke ville være mulige på anden vis.

Hvad er objekter
------------------

I det grundlæggende koncept for programmering er objekter specielle datatyper, der ud over deres egen værdi kan definere detaljeret tilstand, metoder og adfærd, og som giver dig mulighed for at udføre forskellige operationer, som i den virkelige verden.

Et objekt i programmering er noget, der ligner en "ting" fra den virkelige verden, som vi kan navngive (f.eks. ved hjælp af et navneord) og som har egenskaber som en ting fra den virkelige verden. Et objekt kan f.eks. være en artikel, der har nogle værdier (titel, indhold, forfatter, udgivelsesdato, ...) og metoder (indsæt ny titel, læs indhold, beregn antal dage siden udgivelsen, ...). I denne artikel vil vi primært beskæftige os med, hvordan du opretter og arbejder med objektet på et grundlæggende niveau.

Objekter og klasser
---------------

Som vi allerede har fastslået, er et **objekt noget, der eksisterer**. Ordet "eksisterer" er meget vigtigt på dette stadium, for som vi snart vil se, er der stadig en praktisk implementering, der skal håndteres i programmeringen.

Da vi i programmering skriver kode, og et objekt er noget "levende", vil vi fra dette punkt skelne mellem to forskellige begreber, for hvilke det er vigtigt at forstå betydningen i detaljer og skelne den nøje:

- Et **objekt** er noget "levende", der eksisterer lige nu (har en instans) og kan udføre noget eller reagere på andre objekter.
- En **klasse** er statisk skrevet kode, som endnu ikke er blevet evalueret og forbliver den samme hele tiden.

Da vi altid skriver kode som en statisk streng (tekst), når vi programmerer, vil vi skelne programmerne i `klasser` (statisk definition af hele objektet) og `objekter` (en `levende` instans af klassen).

Eksempel på en klassedefinition:

```php
class Article
{
    public string $title;

    public ?string $autor = null;
}
```

> **Praktiske bemærkninger:**
>
> I et rigtigt program skrives hver klasse normalt til en separat PHP-fil, som har samme navn som klassens navn.
>
> Så for klassen `Article` opretter vi f.eks. `src/Article.php`. Denne konvention er ikke påkrævet i PHP, men den hjælper med at gøre programmet mere læsbart, så du ikke har en masse kode samlet på ét sted.
>
> Hver klasse kan højst eksistere én gang i hele programmet (har et unikt navn), men der kan være (næsten) uendeligt mange forekomster (afhængigt af RAM-kapaciteten). Desuden kan en enkelt klasse ikke opdeles i flere filer, men skal defineres ét sted på én gang. Hvis du har brug for at oprette flere klasser med samme navn, skal du bruge **namespaces**.
>
> Vi vil også senere vise, at velstruktureret kode vil gøre det muligt at genbruge den på tværs af mange projekter.

Denne kode definerer en `Article`-klasse med to egenskaber (`title` og `author`). PHP ignorerer kommentarer og bruger dem kun til statisk typekontrol (typisk læst af PhpStorm).

Kode:

```php
public string $title;
```

betyder definitionen af `properties`, som er en egenskab for klassen `Article`. Vi kan se det sådan, at `Article` er en slags felt, og `title` er dets indeks, som vi kan skrive en værdi ind i. Hver egenskab skal have defineret en adgangsdefinition (`public`, `protected` eller `private`), vi vil senere forklare, hvad hver indstilling betyder. Hvis du ikke ved, hvilken tilgængelighed du skal vælge i et bestemt tilfælde, skal du angive `public`.

Instantiering af en klasse og oprettelse af et objekt
----------------------------------

Begrebet `instance` er et af de mest vanskelige begreber at forstå, så det er vigtigt, at du læser de følgende linjer meget omhyggeligt, og jeg anbefaler, at du prøver alle eksemplerne.

Hvis vores program allerede har en klasse defineret i kildekoden, bruges den ikke rigtig nogen steder, og PHP kender ikke til den. Dette skyldes, at OOP indfører `dataindkapslingsprincippet`, hvilket betyder, at en klasse har en intern (lokal) kontekst, som kun er gyldig inden for klassen selv. Det skyldes, at vi i objektløs udvikling var vant til altid at evaluere koden i sin helhed. Prøv også at tænke på en klasse som en form for funktion eller et eksternt kaldt program.

Nu kan vi oprette vores første instans ved at bruge den nye `new`-operator.

```php
$myArticle = new Article;
$myArticle->title = 'Min første artikel'; // skriver værdien

echo $myArticle->title; // lister "Min første artikel"
```

Bemærk, at vi placerer hele `Article`-objektet, der er oprettet af `new`-operatoren, i variablen `$myArticle`. Det betyder for os, at objektet faktisk er en `datatype`, så vi kan flytte det på tværs af variabler og fortsætte med at arbejde med det.

Piloperatoren `->` bruges til at manipulere objektet. Hvis vi har en instans af et bestemt objekt (vi har en variabel med dets indhold), kan vi nemt skrive værdier til de interne egenskaber eller læse dem eller ændre dem på forskellige måder ved hjælp af metoder (vi ser senere).

**Instance af et objekt er oprettelsen af en lokal "levende" kopi af klassen i hukommelsen (en variabel).**

Oprettelse af flere forekomster af den samme klasse på samme tid
---------------------------------------------

Som vi allerede har diskuteret, har hvert objekt en lokal kontekst, der gælder for dets interne dele. Takket være dette princip kan vi oprette mange uafhængige instanser af den samme klasse og manipulere dem frit. Vi kan endda oprette et dynamisk antal forekomster og f.eks. gemme dem som array-elementer. Mulighederne er uendelige.

> **Varsling:**
>
> Når du opretter et ekstremt antal instanser (hundreder eller flere), skal du huske på hukommelsesaftrykket, da PHP skal gemme oplysninger om alle instanser i RAM. I praksis opstår problemet med hukommelsesoverløb på grund af mange forekomster imidlertid ikke.

Eksempel:

```php
$firstArticle = new Article;
$firstArticle->title = 'Min første artikel';

$secondArticle = new Article;
$secondArticle->title = 'Om en hund og en kat';

echo $firstArticle->title;  // lister "Min første artikel"
echo $secondArticle->title; // skriver ud "Om en hund og en kat"
```

Bemærk, at opførelsen af egenskaben `title` (`->title`) afhænger af, hvilken instans vi læser fra.

Resumé
-------

Vi har vist, hvordan vi definerer vores første klasse og opretter en instans af den (et objekt), som vi har skrevet flere værdier ind i.

I næste afsnit forklarer vi begrebet <a href="/methods-and-passing-input">Constructor, Methods and Passing Input</a>.
