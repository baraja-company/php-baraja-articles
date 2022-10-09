Test af grundlæggende viden om Nette
====================================

> id: d137c177-503c-4e84-8709-0e65b0ce6060
> slug:
> 	cs: test-nette-zaklady
> 	da: test-af-grundlaeggende-viden-om-nette
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '28a2aef7-7490-43a9-add7-80d8a051f8a9'
> sourceContentHash: '66fb122b33aa8ce41158345bb01769ab'

Succesgrænse: 15 point

*Du får 1 point for hvert spørgsmål, du svarer rigtigt på. Du får intet for et forkert besvaret spørgsmål. Hvis svaret kun er delvist (og det ikke ville være muligt at programmere tingen på grundlag af det), tæller spørgsmålet som ukorrekt (det er ikke muligt at få et halvt point). Hvis løsningen indeholder en sikkerhedsfejl eller en slåfejl i koden eller en slåfejl i koden, betragtes svaret som ukorrekt, fordi det ikke ville kunne køre.*

-----------

1 Forklar forskellen mellem sløjferne `for`, `while` og `foreach`. Giv for hver af dem 1 specifikt eksempel på deres anvendelse, der tydeligt viser deres vigtigste fordel.


2. Vi har en variabel, som vi næsten intet ved om (vi kender kun dens navn). Hvordan kan vi se dens indhold? Den hedder f.eks. `$data`.


3. Skriv følgende kommandoer nedenfor for at arbejde med Git-repositoriet:
- Hent de seneste ændringer fra serveren
- tag filen `Statistic.php`.
- tagge alle filer i projektet
- mærker alle filer i mappen `cron`.
- at overføre ændringer med meddelelsen "Min overførsel"
- sende commit til serveren


4. Lad os have en tekststreng i variablen. Giv et eksempel på en funktion til beregning af checksummen.


5. Skriv et kodestykke, der opretter en handling `delete` i `Presenter`, der accepterer emne-ID som et heltal og sletter en række fra tabellen `question` i overensstemmelse med det angivne ID. Efter en vellykket sletning vil den udskrive meddelelsen "Question deleted" (Spørgsmål slettet) og viderestille til handlingen `list`.

Under spørgsmål om et ekstra point: Hvis sletningen mislykkes af en eller anden grund, giver den ikke en fejlmeddelelse, men informerer også brugeren om det med en meddelelse (flash-meddelelse).

6. Når jeg opretter en Nette-formular, bliver den til en komponent. Hvad er en Nette-komponent?

7. Jeg skal lave en simpel Nette-formular til at indsætte en post i en `question` tabel, der indeholder en liste af spørgsmål. Tabellen er opbygget således:

| Kolonne | Egenskaber |
|-----------|----------------------------------|
| id | int(8), usigneret, automatisk stigning |
| question | varchar(255) |
| is_active | tinyint(1), uden fortegn, standardværdi: 1 |

Opret de relevante formularfelter for at indsætte en ny række i denne tabel. Når posten er indsat, skal der sendes en FlashMessage, der informerer om, at indsættelsen af posten er lykkedes, og som omdirigerer til redigering af posten (handling `edit`).

- Validering af, at formularfeltet er blevet udfyldt
- Validering af, at spørgeteksten passer ind i varchar-området i henhold til tabellens struktur
- Validering af, at et spørgsmål med denne tekst ikke længere findes
- Definer endnu en brugerdefineret `group`-tabel til at indeholde oplysninger om grupperne. Når du opretter et spørgsmål, vil det være muligt at bestemme, hvilken gruppe spørgsmålet hører til. Du skal oprette en session mellem bordene (beskrive, hvordan det gøres, og hvordan det vil blive oprettet).
- Hvilken Latte-makro skal jeg bruge til at gengive formularen til skabelonen (standardgengivelse)?

8. Lad os have en redigeringsformular i `Presenter`, som er oprettet som en komponent. Vi ønsker at indsætte standardværdier fra det, der findes i databasen, dvs. vi skal hente dataene fra tabellen på en praktisk måde.
- Hvordan vil du gå frem, og hvilke elementer i koden har vi brug for?
- Hvordan vil du sende identifikatoren for det aktuelt redigerede element til formularen?
- Hvordan indstiller du standardværdien for formularelementet?
- Hvordan kan vi kontrollere, at en bruger forsøger at redigere et emne, der ikke findes i databasen, og hvordan kan vi informere brugeren om dette på passende vis?

9 Overvej følgende data hentet fra en database (ved hjælp af en almindelig Nette Database):

```php
$questions = $this->db->questions()->fetchAll();
```

- Hvordan kan vi opføre teksten til alle spørgsmål som en liste med punktopstillinger?
- Hvordan sender vi dataene fra tabellen til Latte-skabelonen?
- Hvilke Latte-makroer skal vi bruge til at opføre varerne? Giv en specifik implementering af en liste over kolonnerne `id` og `name` i formatet:

	*1024: Hvordan har du det?
	*1025: Hvad fik du til frokost i dag?

10. Angiv et eksempel på mindst 3 forskellige formularfelter, der er skrevet i formularen:

```php
$form->add(tady bude příklad);
```

og for hver enkelt forklarer du, hvad den bruges til, og hvilket output den returnerer (datatype + eksempel).


11. Lad os få en indsendt Nette-formular.
- Hvordan får vi sendt alle felter (navne og værdier)?
- Giv et eksempel på udskrivning af et felt med navnet `question`.
- Tilvejebring en konkret implementering af kode, der gennemgår arrayet af værdier og nøgler og returnerer en enkelt streng, der indeholder den samlede liste over nøgler og værdier, så vi f.eks. kan gemme denne streng i en database eller sende den pr. e-mail (lagring og afsendelse er ikke genstand for opgaven og vil ikke blive evalueret).


12. Beslut for hver betingelse, om resultatet er SAND eller FALSK:
- `1 > 0`
- `1 == 1`
- `1 == "1"`
- `1 === "1"`
- `1 == sand`
- `1 === sand`
- `1 === falsk`
- `1 == "1" && 1=== sand`


13. Hvilke datatyper kender vi til i PHP?
- Giv mindst 5 eksempler på datatypebetegnelser
- Angiv for hvert eksempel mindst 3 mulige værdier, der kan lagres i datatypen (hvis datatypen ikke kan lagre så mange mulige værdier, skal du skrive det)
- Giv for hver datatype et typisk eksempel på dens anvendelse (hvad lagres der i den i praksis)
- Hvad er forskellen på tilstanden mellem `==` (to ligemænd) og `===` (tre ligemænd)?
- Forklar ulempen ved at bruge `==` i betingelser, og hvordan `==` løser dette problem (eksempel hvor `==` kan mislykkes, og `==` redder situationen)


14. Lad os have en koordineringstabel (koordineringstabel), der opregner alle koordineringer mellem 2 personer. Den ene af dem organiserer koordineringen, og den anden er gæst. Skriv et databasevalg, der returnerer alle rækker med koordineringer, der involverer mig (er jeg arrangør af koordineringen, eller er jeg gæst i koordineringen). Tabellen har kolonnerne `id`, `id_user_organizer` (id for arrangør), `id_user_quest` (id for gæst). Mit ID er gemt på den sædvanlige måde i `Presenter`.


15. Gruppe af spørgsmål om Latte:
- Hvad er Latte?
- Hvad er forskellen mellem `variable`, `makro`, `filter` og `n:attribut`? Hvad anvendes hvor?
- Hvordan opretter jeg en `DashboardPresenter`-reference til en `default`-handling?
- Hvordan kan jeg generere et link til en specifik redigering (`QuestionPresenter`, `rediger`-handling) af et spørgsmål for at videregive ID'et for det aktuelle spørgsmål på listen? Skriv en specifik Latte-kode.

Symbolsk skrevet (eksempel i PHP, oversættes til Latte):

```php
foreach ($questions as $question) {
   echo $question->id; // Spørgsmåls-ID
   echo $question->question; // spørgsmålstekst
}
```

- Hvordan skriver man et fast HTML-rum?


16. Hvad bruges tjenester til i Nette?
- Hvordan oprettes den?
- Hvad skal en tjeneste gøre for at blive brugt?
- Hvordan indlæser jeg en tjeneste i Presenter?
- Tag for eksempel tjenesten `StatisticManager`, som har en offentlig metode `getStatistics()`, der ikke accepterer nogen parametre. Hvordan indlæser jeg denne tjeneste i Presenter og kalder den offentlige metode `getStatistics()` i standardhandlingen og sender resultatet til skabelonen?
- Hvad er forskellen mellem `object`, `class` og `service`?
- Hvad er `model`, `entity` og `value object`?
- Alle tjenester har en master manager, der kender dem og kan hentes i denne manager. Hvad er navnet på denne leder? Hvordan registrerer jeg en ny tjeneste i den?


17. Neon
- Hvad er Neon-filer?
- Hvilke typer findes der, og hvad er de sorteret efter?
- Hvad indeholder Neon-filer? Hvilke data er gemt i dem?
- Giv et eksempel på, hvordan man registrerer følgende felt (opskriften er i PHP, du skal oversætte den), så det kan bruges som parameter:

```php
$imageGenerator = [
   "point" => [
      480: [910, 30, 1845, 1150],
      600: [875, 95, 1710, 910],
      768: [975, 130, 1743, 660]
   ]
];
```

- Giv et eksempel på registrering af en tjeneste i Neon, som vi sender parameteren `imageGenerator`, som vi registrerede i den foregående opgave, så tjenesten modtager den i konstruktøren og kan bruge den i tjenesten (i betydningen konfiguration). For tjenesten skal du give et eksempel på en implementering af konstruktøren, så den første inputparameter behandles som datatype for arrayet.


18. Genstande i almindelighed
- Hvad er `metode`, `egenskaber` og `konstanter`? Hvad er forskellen på dem?
- Både metoder og egenskaber har 3 grundlæggende tilgængelighedstilstande (`public`, `private`, `protected`), forklar forskellen og et specifikt eksempel på brug og hvem der kan se hvad og hvornår.
- Jeg har en klasse `course`, hvor der er en privat egenskab `currentCourse`, hvor det aktuelle kursus er gemt. Hvordan kan man gøre ejendommen skrivebeskyttet og ikke skrive udefra?


19. Når jeg opretter tabeller i en database, der er logisk relateret (f.eks. en tabel for en bruger og derefter en tabel for hans artikler), skal jeg behandle, at dataene bliver forbundet korrekt.
- Hvad sikrer, at dataene i tabellerne er forbundet korrekt i databasen?
- Hvad er referentiel integritet, og hvad er dens rolle i databasen?
- Hvilke typer af sessioner har vi? Hvad er formålet med de enkelte typer?
- Hvilke parametre skal vi indstille for sessioner for at håndtere sletning eller ændring af data på forskellige måder? Giv 3 eksempler og specifikke anvendelser + en beskrivelse af, hvordan det fungerer.


20. Hvad er formålet med factories (`OOP design pattern`)?
- Giv et eksempel på en anvendelse.
- Er Nette-tjenesterne fabrikker?
- Hvad er formålet med injektion af afhængighed?
- Hvad er forskellen mellem `DI` og `DIC`?
