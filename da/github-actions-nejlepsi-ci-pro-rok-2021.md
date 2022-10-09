GitHub Actions - det bedste CI til 2021
=======================================

> id: c1fe3b77-a506-4f20-bf1e-90ee82817c7d
> slug:
> 	cs: github-actions-nejlepsi-ci-pro-rok-2021
> 	da: github-actions-det-bedste-ci-til-2021
> 
> perex:
> 	- 'Protože už nějaký čas vyvíjíte webové aplikace, určitě jste si všimli, že se pro vás celá řada věcí rutinně opakuje, i když by nemusela. Velmi často jde o technickou správu projektů, verzování souborů, automatická kontrola kódu, zpracování různých dat, nebo třeba deploy na server a bezpečnostní věci kolem toho.'
> 	- 'Da du har udviklet webapplikationer i et stykke tid nu, har du sikkert bemærket, at mange ting er rutinemæssigt gentagende for dig, selv om de ikke burde være det. Meget ofte er det teknisk projektstyring, filversionering, automatiseret kodegennemgang, forskellige databehandlinger eller måske udrulning til serveren og sikkerhedsspørgsmål i forbindelse hermed.'
> 
> publicationDate: '2021-02-07 15:46:39'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: d3d151927a02dde744d1202449c904dc

Da du har udviklet webapplikationer i et stykke tid nu, har du sikkert bemærket, at mange ting er rutinemæssigt gentagende for dig, selv om de ikke burde være det. Meget ofte er det teknisk projektstyring, filversionering, automatiseret kodegennemgang, forskellige databehandlinger eller måske udrulning til serveren og sikkerhedsspørgsmål i forbindelse hermed.

Når jeg rådgiver virksomheder, støder jeg meget ofte på det problem, at forebyggelse er meget undervurderet. Årsagen er ofte, at udviklerne opfatter nogle ting som meget udfordrende, og at det vil give dem mere arbejde. Men sandheden er, at du som regel kun behøver at opsætte hele systemet én gang, og derefter kan du høste fordelene på lang sigt.

Hvad er CI (Continuous Integration)
----------

CI/CD er et værktøj, der kan automatisere rutineopgaver, der har et lignende grundlag og gentages i projekter. Den mest almindelige brug af CI er til at køre automatiserede tests, kodegennemgang, automatiserede udrulninger (udrulning af et program til en webserver), projektstyring eller måske sikkerhedsrevisioner.

Tænk på CI som et virtuelt operativsystem, der udfører foruddefinerede kommandoer til terminalen eller kører bestemte programmer. Resultatet af CI er enten en succes eller en fiasko, som vises direkte i projektet, men også sendes til administratorer, som kan reagere på det. En god praksis er at køre CI-jobs efter en specifik begivenhed (f.eks. en commit til repositoriet, en modtaget merge request/pull request, et tag/version/release eller en cron timeloop-kørsel).

Hvordan CI specifikt fungerer
------------

Grundlaget for CI er en konfigurationsfil, hvor vi definerer dens adfærd og reaktion på hændelser. I GitHubs tilfælde skal du blot oprette en hjælpemappe `.github/workflows` og oprette enhver fil med udvidelsen `.yml` deri. GitHub analyserer den med hver commit og udfører den efter behov.

Lad os forestille os, at vi ønsker at definere en ny konfigurationsfil med en specifik opgave. Da dette er en opgave, der skal køres direkte af GitHub eller et andet miljø på den virtuelle maskine, skal vi definere grundlæggende ting om miljøet, herunder:

- Navnet på opgaven (f.eks. navnet på testen)
- Triggerbegivenheder (hvilken begivenhed der skal udløse opgaven baseret på, f.eks. hver gang du skubber til arkivet eller modtager en pull request)
- Definitionen af selve opgaverne (der kan være mange opgaver, der kører parallelt)

I tilfælde af job definerer vi yderligere:

- Navn på jobbet
- Driftsmiljø (typisk navnet på operativsystemet)
- Trin til opbygning af beholderen

Ovenstående container er allerede den første forberedelse til **fuldstændig dockerisering af projektet**. Den grundlæggende brug af CI er relativt let, og du behøver ikke at forstå Docker overhovedet, du skal blot kende kommandoerne i Terminal.

Som en del af de specifikke trin i opgaven skal vi køre de enkelte kommandoer, der skal opbygge installationen af det operativsystem, vi netop har installeret. Så i tilfælde af PHP er det vigtigt at installere kun PHP-sproget (og angive versionen), klone projektets repository til runneren, installere projektafhængighederne (oftest [Composer](/composer)) og køre selve testen.

Hvorfor bruge GitHub Actions (CI)?
--------

Der er en udbredt overtro i it-verdenen om, at Microsoft er den onde virksomhed, der laver ting af lav kvalitet. I tilfældet GitHub Actions er dette dog slet ikke sandt, for de har helt objektivt set verdens bedste CI-platform.

Den grundlæggende fordel ved GitHub CI er enkelheden og elegancen i notationen. Med få linjer kan du konfigurere dit eget testmiljø og administrere det automatisk, selv uden forudgående kendskab. Samtidig ser jeg hastigheden i behandlingen af specifikke opgaver som en stor fordel. For eksempel kan en test af codestyle (kodeformatering) og PhpStan (kontrol af datatyper, logik og generel korrekthed) evalueres af GitHub Actions på et almindeligt projekt på under 50 sekunder, mens GitLab for eksempel evaluerer den samme test på næsten 8 minutter! Andre løsninger som Travis er relativt dyre, og de fleste udviklere sætter alligevel ikke pris på fordelene ved deres særlige funktioner.

Forberedte foranstaltninger
-----

GitHub har en stor database med færdige handlinger og pakker, som du kan bruge med det samme. Det drejer sig typisk om operativsystemer, programmeringssprog eller biblioteker fra fællesskabet.

Du kan finde en database med handlinger direkte på [Markedspladsen](https://github.com/marketplace?type=actions), f.eks. er min favorithandling [Send SMS i tilfælde af fejl via Twillio](https://github.com/marketplace/actions/twilio-sms).

Færdige eksempler
------

Meget mange eksempler [jeg offentliggør i mine egne repositories] (https://github.com/baraja-core), hvor du kan se, hvilken specifik konfiguration jeg bruger.

I februar 2021 brugte jeg f.eks. denne konfiguration til kontrol af kodetil i et projekt:

```yaml
name: Coding Style

on:
  push:
    branches:
      - master
  pull_request:
    types: [ assigned, opened, synchronize, reopened ]
  schedule:
    - cron: '1 * * * *'

jobs:
  nette_cc:
    name: Nette Code Checker
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: shivammathur/setup-php@v2
        with:
          php-version: 7.4
          coverage: none

      - run: composer create-project nette/code-checker temp/code-checker ^3 --no-progress
      - run: php temp/code-checker/code-checker --strict-types --no-progress

  nette_cs:
    name: Nette Coding Standard
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: shivammathur/setup-php@v2
        with:
          php-version: 8.0
          coverage: none

      - run: composer create-project nette/coding-standard temp/coding-standard ^3 --no-progress --ignore-platform-reqs
      - run: php temp/coding-standard/ecs check src
```

Denne konfigurationsfil installerer den nyeste Ubuntu (`runs-on: ubuntu-latest`), kloner projektets arkiv (`uses: actions/checkout@v2`), installerer PHP (`uses: shivammathur/setup-php@v2`) version 7.4 (`php-version: 7.4`), installerer code-checker-pakken og kører derefter jobbet via PHP.

Et par yderligere bemærkninger til brugere, der ikke er så erfarne:

- CI starter en virtuel maskine med et komplet styresystem hver gang den kører, som kan bruges til at kalde kommandoer i Terminal ligesom f.eks. din server
- For at kontrollere testresultaterne anvendes såkaldte returkoder, som defineret af Linux selv. Når et program returnerer `0` (nul), betyder det, at det er en succes, og ethvert andet tal betyder en fejl. For eksempel fungerer shell-scripts på samme måde
- Når vi ønsker at skrive en brugerdefineret test eller mere kompleks logik, kan vi f.eks. bruge PHP og køre det som en CLI-opgave (kommandoen `php file.php`), som kører og kan gøre alt, hvad den har rettigheder til (arbejde med filer, kommunikere over internettet, output til skærmen, ...)
- Mens CI kører, registreres alt output på skærmen og kan ses direkte i webbrowseren
- CI er ikke interaktivt, selv om Terminal kan udføre interaktive operationer sammen med brugeren, understøtter CI ikke dette og skal designes som en opgave, der nogle gange starter og nogle gange slutter.
- Der er defineret en timeout på 1 time for CI-containeren til at køre, hvis alt ikke er behandlet inden for denne tid (f.eks. hvis programmet går i stå), vil hele systemet blive tvangsafbrudt, og der vil blive rapporteret en fejl
- GitHub kan køre CI-jobs automatisk via cron, men meget ofte kører de ikke på det rigtige tidspunkt, men kører med en forsinkelse.
- Du kan også pakke al CI-logikken ind i en Docker-container og køre den lokalt på alle maskiner eller på en server, f.eks.
- Hvis du har brug for adgang til hemmelige oplysninger (f.eks. databaseadgangskode), er der mulighed for at definere hemmelige variabler direkte i GitHub, og CI har dem tilgængelige ved kørselstid
- Udrulning til en webserver gøres altid bedst via SSH
- Den samlede køretid for CI-jobs er begrænset, normalt til 2 tusind minutter om måneden (meget ofte vil det være nok for dig, jeg har aldrig brugt det op, fordi et job kører i højst et minut), eller du kan øge tiden mod et gebyr
- Du kan også hoste CI-runnere på din server og omgå minutgrænsen, men din server vil højst sandsynligt ikke være meget hurtigere, fordi Microsoft har investeret kraftigt i deres Azure, hvor GitHub Actions er hostet, og det kører på meget kraftfulde servere.
- Meget ofte er det praktisk at installere specifikke pakker kun til CI (typisk PhpStan og forskellige sikkerhedsfunktioner), så sæt dem i afsnittet `composer.json` i filen `require-dev`, så de ikke bliver installeret i et specifikt projekt som en obligatorisk afhængighed

GitHub-apps og -bots
-----

I modsætning til andre værter for arkiver understøtter GitHub såkaldte GitHub Apps og automatiseringsrobotter. Dette er en stor database med færdige bots, der kan udføre automatiserede operationer på dine projekter (selv uden CI), og når de støder på noget, vil de f.eks. indsende et problem eller sende en pull request med en rettelse.

Jeg bruger den personligt og anbefaler den til dig:

- [Dependabot](https://dependabot.com) - automatisk kontrol af afhængigheder i Composer, for eksempel når en ny version af de pakker du bruger udkommer og er kompatible, sender den automatisk en pull request
- [Codecov](https://github.com/marketplace/codecov) - kontrollerer kodedækning med automatiske tests og viser grafisk hvilke dele af koden der endnu ikke er dækket
- [Snyk] (https://github.com/marketplace/snyk) - kontrollerer automatisk for sikkerhedssårbarheder
- [Imgbot](https://github.com/apps/imgbot) - automatisk optimering af billeddatastørrelse

Du kan finde mange andre programmer på [Markedspladsen] (https://github.com/marketplace?type=apps).

Flere praktiske tips og tricks
-----

Jeg er blevet meget glad for at bruge automatiseringsopgaver i GitHub.

Jeg har oprettet automatiske kontroller i alle mine repositories, så når jeg (eller andre i fællesskabet) indsender et commit, får jeg feedback i realtid om, hvorvidt kodekvaliteten (codestyle og PhpStan), sikkerheden og meget mere er blevet overtrådt.

Jeg har nogle tests, der kører automatisk hver time. Hvis der f.eks. er en sikkerhedssårbarhed eller en CVE, ved jeg det senest om en time, selv på niveauet for specifikke projekter, og hvad der præcist er sket.

Efter at have testet forskellige konfigurationer er jeg med tiden kommet frem til den konklusion, at jeg anser følgende afhængigheder til Composer for at være den ideelle minimumskonfiguration:

- `phpstan/phpstan` - kontrol af koden for korrekthed og logik
- `tracy/tracy` - fejlrapportering og logning på højt niveau
- `phpstan/phpstan-nette` - Udvidelser og specialiteter til Phpstan i Nette
- `spaze/phpstan-disallowed-calls` - et genialt bibliotek af Michal Spacek, der håndterer (u)brug af farlige ting i kode (fra glemte variabel-dumps til evnen til at køre en brugerstreng som en shell-kommando)
- `roave/security-advisories` - kontrollerer installation af usikre versioner af pakker (hvis der findes en sikkerhedssårbarhed i en bestemt pakke eller hvis der udgives en CVE, vil denne pakke forhindre installation af den beskadigede version)

Ideelt set bør du have den grundlæggende sikkerhedsopsætning indstillet til automatisk at køre mindst hver 8. time og få fejl sendt til din e-mail. For når en projektinstallation fejler eller en ny sårbarhed bliver opdaget, vil jeg gerne vide det så hurtigt som muligt og reagere med det samme.

Husk, at alt fungerer automatisk, så du kan altid være sikker på, at selv om du bruger komplicerede processer til at kontrollere sikkerhed og andre ting, koster det dig ikke ekstra kræfter, og du skal blot have en veludført startkonfiguration.

For der er et enormt potentiale i forebyggelse og automatisering!
