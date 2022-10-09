Composer - komplet oversigt over avancerede funktioner
======================================================

> id: a74d8d59-91ce-4602-ad52-80cf89a647bd
> slug:
> 	cs: composer
> 	da: composer-komplet-oversigt-over-avancerede-funktioner
> 
> perex:
> 	- Composer je pokročilý správce balíků a závislostí pro vaše PHP aplikace. Článek popisuje jeho všechny výhody a možnosti použití.
> 	- Composer er en avanceret pakke- og afhængighedshåndtering til dine PHP-programmer. Denne artikel beskriver alle dens fordele og anvendelsesmuligheder.
> 
> publicationDate: '2020-03-10 20:18:19'
> mainCategoryId: '4f1d7d70-c5b0-45f1-b1d2-d03c22aa4154'
> sourceContentHash: '68340d4b4d3c8a6ed143ede176fbf04e'

Som du allerede ved, er [Composer] (https://getcomposer.org/) en robust pakke- og afhængighedsadministrator til PHP, hvor du på elegant vis kan administrere hundredvis af projekter på én gang og distribuere kode, når den er skrevet, til alle programmer samtidigt.

Denne vejledning tjener som en detaljeret og omfattende guide for udviklere. Vi vil dække alle vigtige avancerede teknikker til at arbejde med Composer og forklare de tekniske detaljer og relevante afhængigheder.

Installation af Composer
-------------------

Uanset platformen downloader vi fra [det officielle Composer-websted] (https://getcomposer.org/).

Internt hentes PHP-filen `composer-setup.php`, som, når den køres i CLI-tilstand, kan installere Composer. Det er også vigtigt at vide, at Composer ikke fungerer uden PHP, så først skal du kontrollere, at du har PHP kørende på din computer (det skal kun være tilgængeligt for Terminal).

På Mac og Linux fungerer Composer lige efter installationen, og du skal blot kalde kommandoen `composer -v` for hurtigt at kontrollere, at Composer er installeret korrekt.

På Linux kan følgende kommando bruges til at installere: `/usr/bin/php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"`

På Windows er det en god idé at installere værktøjet [Git Bash for Windows](https://gitforwindows.org/), som giver dig mulighed for at åbne en særlig Terminal, der opfører sig næsten som Linux og giver dig mulighed for at arbejde i det samme miljø som på Linux.

Installationen til servere er den samme som i et lokalt miljø. Du skal blot sikre dig, at du har den korrekte version af PHP, som Composer bruger internt.

Tilgængelige kommandoer
----------------

Composer implementerer en række kommandoer.

Brugen er: `composer <kommando>`, for eksempel: `composer update`.

Oversigt for `1.10.0`:

| Kommando | Beskrivelse / Betydning |
|-----------------------|----------------|
| `about` | Viser korte oplysninger om Composer. |
| `archive` | Opretter et arkiv med indholdet af den valgte Composer-pakke. |
| `browse` | Åbner startsiden for den valgte pakke, forfatter eller anden relateret side i en webbrowser. Ofte kan de indeholde dokumentation om, hvordan de skal bruges. |
| `cc` | Rydder den interne Composer-cache med versioner af pakker, der er hentet tidligere. |
| `check-platform-reqs` | Kontroller, om installationskravene er opfyldt for den aktuelle platform. |
| `clear-cache` | Ryd den interne Composer-cache. |
| `clearcache` | Ryd den interne Composer-cache. |
| | `config` | Indstiller et konfigurationsdirektiv.
| `create-project` | Opretter et nyt projekt baseret på den valgte pakke og opretter automatisk en mappe til at placere projektet i.
| `depends` | Viser, hvilke pakker der har forårsaget, at den valgte pakke er blevet installeret. |
| `diagnose` | Diagnosticerer systemet for at identificere almindelige fejl. Behandlingen af output er op til udvikleren, det er blot en liste. |
| `dump-autoload` | Genererer en ny <a href="/autoloading-trid">autoloader</a>. |
| `dumpautoload` | Genererer en ny <a href="/autoloading-trid">autoloader</a>. |
| `exec` | Udfører binære filer og scripts fra Vendor. |
| `fund` | Udforsker, hvordan du laver/ændrer dine afhængigheder. |
| `global` | Giver dig mulighed for at køre globale Composer-kommandoer fra variablen `$COMPOSER_HOME`. |
| `help` | Udskriver hjælp til kommandoer. |
| `home` | Åbner startsiden for en specifik pakke i browseren. |
| `i` | Installerer alle projektafhængigheder i henhold til filen `composer.lock`, hvis den findes og er gyldig. Hvis der opstår et problem, anvendes oplysningerne fra `composer.json`, og `composer.lock` gendannes til sin oprindelige tilstand. |
| `info` | Viser oplysninger om de aktuelt installerede pakker i projektet. Viser navnene på alle pakker, deres aktuelle versioner og en kort beskrivelse. |
| `init` | Opretter en grundlæggende `composer.json`-funktion i den aktuelle mappe. |
| `install` | Installerer alle projektafhængigheder i henhold til filen `composer.lock`, hvis den findes og er gyldig. Hvis der opstår et problem, anvendes oplysningerne fra `composer.json`, og `composer.lock` gendannes til sin oprindelige tilstand. |
| `licenses` | Viser en liste over alle pakker, deres versioner og aktuelle licens. |
| `list` | Viser en liste over tilgængelige kommandoer. |
| | `outdated` | Viser en liste over alle pakker, for hvilke der er en nyere version tilgængelig til installation, og som opfylder afhængighederne. For hver pakke vises den seneste kompatible version, som Composer foreslår til installation, for hver pakke. |
| `prohibits` | Viser hvilke pakker og afhængigheder der forhindrer installationen af den ønskede pakke eller version. |
| `remove` | Fjerner pakken fra konfigurationsafsnittet `require` eller `require-dev`. |
| `require` | Tilføjer den ønskede pakke til `composer.json` og installerer den. Hvis afhængighederne ikke kan opfyldes, vil den vende tilbage til den oprindelige tilstand. |
| `run` | Kører de definerede scripts i `composer.json`. |
| `run-script` | Kører de definerede scripts i `composer.json`. |
| `search` | Søger pakker efter nøgleord eller søgeforespørgsel. |
| `self-update` | Opdaterer den interne `composer.phar` til den nyeste version. |
| `selfupdate` | Opdaterer den interne `composer.phar` til den nyeste version. |
| | `show` | Viser detaljerede oplysninger om de aktuelt installerede pakker. |
| `status` | Viser en oversigt over lokale ændringer, der er foretaget manuelt i pakker, baseret på en sammenligning med kilden til den pakke, hvorfra den oprindeligt blev installeret. |
| `suggests` | Viser forslag til pakker. Forslagene kan omfatte forskellige former for handlinger, f.eks. installation af sikkerhedsopdateringer osv. |
| `update` | Opdaterer hele projektet i henhold til afhængighederne, så de altid er opfyldt af `composer.json`. Hvis det lykkes, opdaterer den `composer.lock`, hvor den skriver de aktuelt installerede versioner. |
| `upgrade` | Alias til `update`. |
| `u` | Alias til `update`. |
| `validate` | Kontrollerer `composer.json` og `composer.lock` for syntaksfejl. |
| `why` | Viser hvilke pakker der forårsagede, at den aktuelt valgte pakke blev installeret, inklusive alle afhængigheder. |
| `why-not` | Viser hvilke pakker og versioner der forhindrer installationen af den valgte pakke eller version. |

Oprettelse og definition af et projekt
----------------------------------

Hvert projekt, der administreres af Composer, er defineret af en `composer.json`-fil i roden, som definerer alle afhængigheder. Filen kan oprettes enten manuelt for et eksisterende projekt eller automatisk, når du opretter et projekt.

Da alt i Composer er en pakke, kan selve projektet være baseret på en pakke. Så hvis du f.eks. opretter snesevis eller hundredevis af meget ens projekter, giver det mening at lægge deres grundlæggende konfiguration og struktur i en separat pakke, som installationen kan baseres på.

Et eksempel på en sådan pakke er min [Baraja Sandbox] (https://github.com/baraja-core/sandbox), som er baseret på ren Nette 3.0 og tilføjer en grundlæggende afhængighed til min [Package Manager] (https://github.com/baraja-core/package-manager), som jeg bruger til alle projekter og håndtering af afhængigheder i Nette-konfigurationen.

Sandkassen installeres derefter ganske enkelt med kommandoen:

```shell
$ composer create-project baraja/sandbox <your-project-name>
```

Baseret på projektnavnet opretter Composer automatisk en mappe til at installere projektet (kopier indholdet af pakken og installer afhængighederne).

I `vendor`-mappen administrerer Composer derefter alle pakkerne (deres fysiske filer er der) og genererer en autoload af klasser, som vi ideelt set sætter direkte ind i `index.php` som en linje:

```php
require __DIR__ . '/vendor/autoload.php';

// Selve programkoden
```

Installation af yderligere pakker og afhængigheder
-------------------------------------

I et funktionelt projekt kan vi meget nemt installere nye pakker og tilføje afhængigheder.

Der er 2 måder at gøre dette på:

- Med kommandoen `composer require ...`,
- Ved at tilføje en afhængighed direkte til filen `composer.json` i afsnittet `require` og derefter bruge kommandoen `composer update`.

Prøv f.eks. at installere PackageManager: `composer require baraja-core/package-manager`, eller [Doctrine](https://github.com/baraja-core/doctrine): `composer require baraja-core/doctrine`.

Hvis den valgte pakke ikke kan installeres, kan der spørges om specifikke årsager, og Composer vil opregne de afhængigheder, der forhindrer dette. Ofte er det nok at rette en afhængighed af en bestemt version eller fjerne inkompatibel kode. Du kan få detaljerede oplysninger ved at bruge kommandoen: `composer why baraja-core/doctrine`.

Opdatering af projekter og pakker
-----------------------------

Et veldesignet projekt er udviklet således, at du nemt kan hente opdateringer over tid og altid har de nyeste versioner af alle pakker. Den største fordel er, at du får alle fejl rettet, ofte forbedringer af ydeevnen og masser af nye funktioner. Desuden vil den gradvise overgang gøre det mindre kompliceret at opdatere efter lang tid, da du løser problemerne løbende i mindre omfang og undgår inkompatibiliteter.

Du kan opdatere alle pakker og afhængigheder ved at bruge kommandoen `composer update` for at opdatere alle pakker og afhængigheder.

Selve opdateringsprocessen kan i nogle tilfælde mislykkes. Årsagen er normalt enten en brudt afhængighed eller en inkompatibel pakke.

For detaljerede oplysninger om, hvorfor en pakke ikke kan installeres, skal du bruge kommandoen: `composer why-not baraja-core/doctrine`. Hvis vi allerede har pakken, og kun den specifikke version ikke virker (den vil ikke installeres), kan vi også bede om den specifikke version: `composer why-not baraja-core/doctrine:v1.0.20`.

I filen `composer.json` kan vi også angive afhængigheden af en bestemt runtime. Dette er især nyttigt, når vi udvikler et projekt med flere personer og ønsker at kontrollere, at de har alle udvidelserne installeret.

Typisk kontrolleres versionen af PHP (skal være `7.1.0` eller nyere):

```json
{
   "require": {
      "php": ">=7.1.0"
   }
}
```

Eventuelt andre systemudvidelser:

```json
{
   "require": {
      "php": ">=7.1.0",
      "ext-json": "*",
      "ext-session": "*",
      "ext-PDO": "*"
   }
}
```

Der tages derefter hensyn til disse regler, når der installeres pakker eller opgraderinger. Dette er med til at forhindre problemer, som ville blive synlige ved kørselstid. Typisk skal f.eks. en betalingsgatewaypakke kommunikere med API'et, så den skal indrømme afhængigheder af udvidelserne `curl` og `json`.

Fejlfinding af afhængigheder
-----------------------------

Ofte opstår afhængighedskrænkelser på grund af en dårlig version af PHP. I dette tilfælde sender Composer en meddelelse, f.eks. "Din PHP-version (7.3.11), der er overstyret af "config.platform.php"-versionen (7.1), opfylder ikke dette krav.".

Meget ofte skyldes fejlen indstillingerne direkte i projektet `composer.json`, hvor følgende afsnit er:

```json
"config": {
   "platform": {
      "php": "7.2"
   }
}
```

Ændringen **skal foretages direkte i filen**. I tilfælde af et globalt projekt (før installation eller med en global afhængighed) kan du fremtvinge Composer-versionen med `composer config -g platform.php 7.2.14` (`-g`-switchen betyder `global`).

I nogle tilfælde ønsker vi at installere de nyeste pakkeversioner og ignorere de lokale miljøindstillinger. I dette tilfælde kan vi bruge den avancerede kommando:

```shell
$ composer update --ignore-platform-reqs
```

**Brug switch'en `ignore-platform-reqs` på eget ansvar, den kan forårsage skade på projektet!** Brug den ikke, hvis du ikke forstår konsekvenserne.

Manual Composer-opkald, parametre og hukommelsesstyring
------------------------------------------------------

Composer er faktisk et PHP-script pakket ind i en såkaldt PHAR, som er en kompileret version af et PHP-program. Det kan være nyttigt at kende disse oplysninger, f.eks. for at forbedre parametriseringen af selve opkaldet.

I virkelig store projekter sker det nogle gange, at vi løber tør for RAM og skal allokere meget mere eller øge behandlingstiden for scripts.

På Windows kan du f.eks. bruge denne kommando til det:

```shell
$ php -d memory_limit=-1 C:/xampp/htdocs/composer.phar update
```

Med `memory_limit=-1` skifter Composer til ikke at være modtagelig over for RAM-grænsen og bruge så meget hukommelse, som den har brug for.

Brugerdefinerede brugermanuskripter efter Composer-handling
--------------------------------------------

Når du har kørt Composer-handlinger, kan du påkalde automatisk udførelse af brugerdefinerede scripts, der enten udfører specifikke handlinger på projektet eller f.eks. genererer en konfiguration efter implementering. Jeg bruger primært denne fremgangsmåde på en lokal server for at tilbyde et automatisk database-konfigurationsværktøj, generere et databaseskema osv.

Vi registrerer de nødvendige scripts i afsnittet `scripts` i `composer.json`:

```json
"scripts": {
   "post-autoload-dump": "Baraja\\PackageManager\\PackageRegistrator::composerPostAutoloadDump"
}
```

I dette tilfælde kaldes den statiske metode `composerPostAutoloadDump` i klassen `Baraja\PackageManager\PackageRegistrator` automatisk. Composer har denne klasse til rådighed, fordi den først genererede <a href="/autoloading-trid">autoloader-klasserne</a>.

Hvis vi blot ønsker at køre scripts og ikke udføre unødvendige handlinger med Composer, er kommandoen `composer dump` meget nyttig, da den blot genererer en ny autoloader (hvilket jeg anbefaler for altid at holde den opdateret) og derefter kører scripts med det samme. Hvis du vil prøve at bruge scripts, har jeg udarbejdet en færdig pakke [Baraja PackageManager] (https://github.com/baraja-core/package-manager), som implementerer smarte scripts og interaktiv grænseflade til Nette-rammen.

Versionering af Vendor i Git?
------------------------

Et spørgsmål, som vi ofte diskuterer med udviklere, er, om indholdet af `vendor`-mappen skal versioneres i Git, eller om den skal genereres på ny ved hver installation.

Generelt synes den renere løsning at være at **ikke versionere** indholdet overhovedet og installere det hele hver gang. Men virkeligheden er, at en udvikler fra tid til anden stopper med at udvikle en pakke eller helt fjerner den. Den konstante downloading af pakker komplicerer også lokale installationer og opdateringer og gør også udrulninger langsommere og kan nogle gange forårsage korte nedbrud på webstedet, når nye pakkeversioner downloades forkert.

Jeg ser sælgerens suspension som en form for "sikkerhed". Hvis filerne er fysisk i versioneringssystemet, har vi i det mindste en rudimentær sikkerhed på produktionsserveren for, at pakkerne vil fungere, og at deres kode er nøjagtig som i den lokalt kørende installation. Desuden fylder Vendor ofte kun nogle få MB, hvilket bestemt er værd at sikre et fungerende websted i betragtning af de nuværende diskkapaciteter.

**Praktisk note:** For det gennemsnitlige websted, jeg vedligeholder, fylder `vendor` ikke mere end `30 MB`, hvilket er en acceptabel kapacitet for Git. Når vi kloner et repository med junior, behøver vi ikke at træne ham i at få webstedet op og køre, og det virker bare "med det samme".

Brugerdefinerede Composer-pakker
-----------------------

Du kan oprette dine egne pakker i Composer, enten offentligt tilgængelige (registreret på [Packagist](https://packagist.org/)) eller private (du skal have din egen pakkeserver, f.eks. [Satis](https://getcomposer.org/doc/articles/handling-private-packages.md)).

Spørgsmålet om oprettelse, vedligeholdelse, udvikling og versionering af pakker er meget komplekst og vil blive behandlet i en separat artikel.

I mellemtiden kan du læse artiklen [Semantic Versioning] (https://semver.org/lang/cs/), som jeg har oversat for dig.
