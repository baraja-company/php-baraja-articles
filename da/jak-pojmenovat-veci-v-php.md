Hvordan du navngiver variabler, funktioner, metoder og klasser
==============================================================

> id: f70ac75d-422f-4696-88d5-9e1b843e060a
> slug:
> 	cs: jak-pojmenovat-veci-v-php
> 	da: hvordan-du-navngiver-variabler-funktioner-metoder-og-klasser
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1'
> sourceContentHash: '10510114a0160c0826c9ee1f54c95b03'

For at holde orden i koden er det vigtigt at vælge klare regler for, hvordan vi udleder navne. Denne side er en oversigt over de relativt populære fremgangsmåder, der anvendes af et stort antal programmører, herunder mig selv og folk, jeg arbejder sammen med.

Hvis du arbejder på et udviklingsteam, kan du sagtens bruge deres regler, men i forbindelse med generel udvikling er det lige så gavnligt at etablere nogle få gode vaner.

Da hele PHP-syntaksen er et meget bredt begreb, har jeg inddelt guiden her i mange kategorier, som er meget specialiserede.

Hvis du leder efter en hurtig løsning, anbefaler jeg, at du studerer <a href="https://www.php-fig.org/psr/psr-4/"> PSR-4-standarden</a>.

Indsættelse af scripts, link til HTML, indlæsning af filer
---------------------------------------------------

Hvert script skal starte med et `<?php`-tag.

Hvis der ikke er **ingen HTML** i slutningen af filen, skal den ikke afsluttes (for at undgå et hvidt tegn i slutningen af siden).

Indlæsning af andre filer bør følge følgende regler:

- Hvis filen ikke er kritisk og kan undværes ved rendering af siden (f.eks. yderligere oplysninger i sidefoden), skal den indlæses via include: `include 'file.php';`
- Kritiske filer kun via require-konstruktionen: `require 'file.php';`
- Hvis vi arbejder med objekter, bruger vi require-konstruktionen til at indlæse autoloaderen, som selv indlæser de andre klasser (de fleste frameworks implementerer denne adfærd).


Hvis det er muligt, bør vi adskille logikken for datahentning fra HTML-gengivelsen, dvs. bruge MVC-modellen (model - view - controller).

Så vi forbereder først dataene i f.eks. Presenter:

`Presenter.php`

```php
$cisla = [1, 2, 3];

$data = [];
$data['numre'] = $cisla; // videregive dataene til skabelonen
include 'renderCisel.php'; // indlæse skabelon
```

Derefter tegner du den i skabelonen:

`renderCisel.php`

```html
<table>
<?php
    foreach ($data['cisla'] as $cislo) {
        echo '<tr><td>' . $cislo . '</td></tr>';
    }
?>
</table>
```

Denne fremgangsmåde anvendes af de fleste frameworks og er nyttig til at øge kodens klarhed. I den senere del af udviklingsprocessen bør denne fremgangsmåde anvendes af alle erfarne programmører, som ønsker at udvikle klart strukturerede applikationer (for store applikationer er denne fremgangsmåde et must).

Navne på variabler
----------------

Hvis en variabel indeholder et array af værdier eller andre objekter, skal den navngives i flertal:

```php
$numbers = [1, 2, 3];
```

For så kan vi simpelthen iterere værdierne efter et enkelt tal:

```php
foreach ($numbers as $number) {
    // behandling af tal
}
```

Et navn, der består af flere ord, kombineres til ét langt ord med cameCase-syntaksen, dvs. det første ord starter med et lille bogstav, og hvert efterfølgende ord med et stort bogstav:

```php
$promenna = 'Hey! Hey!';
$seznamUzivatelu = [
   'Jan Barášek',
   'Barack Obama',
   'Steve Jobs',
   'Stephen Wolfram',
];
$maxFilesInDirectory = 12;
$nameOfPhpScript = 'index.php'; // PHP-forkortelsen er blevet reduceret i størrelse
```

Navne på funktioner og metoder
--------------------

Funktioner og metoder skal altid gøre det klart i deres navn, hvad de gør. Ofte kan de forventede inputparametre og returværdien også indgå i navnet.

Prøv at gætte, hvad de følgende funktioner gør, og hvad deres returværdi er:

```php
getUserById($id);
saveErrorToLog($message);
createDefaultDirectory($path);
setAuthors(['Jan Barášek', 'Chuck Norris']);
getCurrentTime();
```

Hele tricket ligger i det første ord i navnet, som gør det klart, hvilken metode funktionen vil bruge. Følgende konvention følges normalt:

- `get` - henter data som et array eller et objekt, inputparametre angiver den enhed, der søges efter
- `save` - gemmer til en fil eller en database
- `create` - opretter en enhed (f.eks. opretter en instans af et objekt)
- `set` - gemmer data til en foruddefineret variabel (inden for en funktion)

Navne på klasser
----------

En klasse er en stor enhed, der indeholder et stort antal egenskaber og metoder, så den skal også starte med et stort bogstav. En klasse skal også kun indeholde én enhed (og beskrive dens egenskaber), så den skal navngives med et enkelt navn. Hvis vi har brug for at arbejde med flere enheder, kan vi blot gemme hver enkelt instans i et array.

Eksempel:

```php
class User
{
    public string $username;

    public string $password;

    public string $role;
}

class Users
{
    /** @var User[] */
    public array $users;

    public function addUser(User $user): void
    {
        $this->users = array_push($this->users, $user);
    }
}
```

**User**-klassen specialiserer sig i oplysninger om kun én bestemt bruger. Hvis vi ønsker at arbejde med flere brugere, opretter vi en anden klasse (envelope), der indeholder et array af instanser af en bestemt enhed.

Fabrikker kan også ofte være nyttige til dette formål, da de giver os mulighed for nemt at oprette lignende objekter og genbruge de oprindelige instanser, hvilket resulterer i klarere kode og samtidig sparer systemressourcer.

Navneområde - Navneområder
---------------------------

Selv om Namespace er uafhængig af den fysiske mappe, hvor scriptet er tilgængeligt, er det en god praksis at respektere projektets layout i det mindste delvist (hvilket fører til et bedre system til oprettelse af nye navne, som er mere entydigt på denne måde).

Personligt navngiver jeg navnerum i overensstemmelse med den fælles undermappe for klasser af den pågældende type.

Eksempler:

```php
App\Presenters; // Her er navnene på alle oplægsholderne
App\Model;      // Dette er navnet på den generelle model
App\Model\Math; // Dette er navnet på den model, der arbejder med matematik
```

For at sikre korrekt autoloading af klasser er det en god idé at følge <a href="https://jakpsatphp.cz/PSR4/">PSR-4-standarden</a>.

Brugerdefinerede konventioner (f.eks. i et virksomhedsteam)
-----------------------------------------

Og hvad kalder du dine? Jeg vil gerne have tips til, hvordan jeg kan forbedre denne artikel.

Generelt giver brugerdefinerede konventioner inden for et team dog ikke meget mening, fordi det gør det sværere at portere koden til andre frameworks, og når du ansætter en ny kollega, skal vedkommende lære de nuværende konventioner. Det er derfor bedst at følge `PSR-4`-standarden.
