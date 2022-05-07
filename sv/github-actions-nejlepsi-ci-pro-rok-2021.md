GitHub Actions - det bästa CI för 2021
======================================

> id: c1fe3b77-a506-4f20-bf1e-90ee82817c7d
> slug:
> 	cs: github-actions-nejlepsi-ci-pro-rok-2021
> 	sv: github-actions---det-baesta-ci-foer-2021
> 
> perex:
> 	- 'Protože už nějaký čas vyvíjíte webové aplikace, určitě jste si všimli, že se pro vás celá řada věcí rutinně opakuje, i když by nemusela. Velmi často jde o technickou správu projektů, verzování souborů, automatická kontrola kódu, zpracování různých dat, nebo třeba deploy na server a bezpečnostní věci kolem toho.'
> 	- 'Eftersom du har utvecklat webbapplikationer ett tag har du förmodligen märkt att många saker är rutinmässigt repetitiva för dig, även om de inte borde vara det. Ofta handlar det om teknisk projektledning, versionering av filer, automatiserad kodgranskning, olika typer av databehandling, eller kanske om att distribuera till servern och säkerhetsfrågor kring detta.'
> 
> publicationDate: '2021-02-07 15:46:39'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: d3d151927a02dde744d1202449c904dc

Eftersom du har utvecklat webbapplikationer ett tag har du förmodligen märkt att många saker är rutinmässigt repetitiva för dig, även om de inte borde vara det. Ofta handlar det om teknisk projektledning, versionering av filer, automatiserad kodgranskning, olika typer av databehandling, eller kanske om att distribuera till servern och säkerhetsfrågor kring detta.

När jag arbetar som konsult på företag stöter jag ofta på ett problem där förebyggande åtgärder är mycket underskattade. Orsaken är ofta att utvecklare uppfattar vissa saker som mycket utmanande och att det kommer att innebära mer arbete för dem. Men sanningen är att du oftast bara behöver ställa in hela installationen en gång och sedan bara skörda de långsiktiga fördelarna.

Vad är CI (Continuous Integration)
----------

CI/CD är ett verktyg som kan automatisera rutinuppgifter som har en liknande grund och som upprepas i projekt. Den vanligaste användningen av CI är för att köra automatiska tester, kodgranskning, automatiserad distribution (distribution av en applikation till en webbserver), projektledning eller kanske säkerhetsgranskningar.

Tänk på CI som ett virtuellt operativsystem som utför fördefinierade kommandon till terminalen eller kör specifika program. CI:s resultat är antingen en framgång eller ett misslyckande, vilket visas direkt i projektet, men skickas också till administratörer som kan reagera på det. En bra metod är att köra CI-jobb efter en specifik händelse (t.ex. en överföring till förvaret, en begäran om sammanslagning/uttag som tagits emot, en tagg/version/release eller en cron-timerloop som körs).

Hur CI specifikt fungerar
------------

Grunden för CI är en konfigurationsfil där vi definierar dess beteende och svar på händelser. När det gäller GitHub behöver du bara skapa en hjälpkatalog `.github/workflows` och skapa en fil med tillägget `.yml` i den. GitHub analyserar den vid varje överföring och utför den vid behov.

Låt oss tänka oss att vi vill definiera en ny konfigurationsfil med en specifik uppgift. Eftersom det här är en uppgift som kommer att köras direkt av GitHub eller en annan miljö på den virtuella maskinen måste vi definiera grundläggande saker om miljön, bland annat:

- Uppgiftens namn (t.ex. testets namn).
- Utlösande händelser (vilken händelse som ska utlösa uppgiften baserat på, till exempel varje gång du lägger in en push till arkivet eller tar emot en pull-förfrågan).
- Definitionen av själva uppgifterna (det kan finnas många uppgifter som körs parallellt).

När det gäller arbetstillfällen definierar vi ytterligare:

- Arbetsnamn
- Driftsmiljö (vanligtvis namnet på operativsystemet)
- Steg för att bygga behållaren

Containern ovan är redan den första förberedelsen för **fullständig dockerisering av projektet**. Den grundläggande användningen av CI är relativt enkel och du behöver inte förstå Docker alls, det räcker med att känna till kommandona i Terminal.

Som en del av de specifika stegen i uppgiften måste vi köra de enskilda kommandon som kommer att bygga upp installationen av det operativsystem vi just installerat. När det gäller PHP är det viktigt att installera bara PHP-språket (och ange version), klona projektförrådet till runneren, installera projektets beroenden (oftast [Composer](/composer)) och köra själva testet.

Varför använda GitHub Actions (CI)?
--------

Det finns en vanlig vidskepelse inom IT-branschen om att Microsoft är det onda företaget som tillverkar saker av låg kvalitet. När det gäller GitHub Actions är detta dock inte alls sant, eftersom de objektivt sett har världens bästa CI-plattform.

Den grundläggande fördelen med GitHub CI är enkelheten och elegansen i notationen. Med bara några få rader kan du konfigurera din egen testmiljö och hantera den automatiskt, även utan förkunskaper. Samtidigt ser jag hastigheten i behandlingen av specifika uppgifter som en stor fördel. Ett test av codestyle (kodformatering) och PhpStan (kontroll av datatyper, logik och allmän korrekthet) kan till exempel utvärderas av GitHub Actions för ett vanligt projekt på mindre än 50 sekunder, medan GitLab till exempel utvärderar samma test på nästan 8 minuter! Andra lösningar som Travis är relativt dyra och de flesta utvecklare uppskattar ändå inte fördelarna med deras specialfunktioner.

Förberedda åtgärder
-----

GitHub har en stor databas med färdiga åtgärder och paket som du kan använda direkt. Dessa är vanligtvis operativsystem, programmeringsspråk eller bibliotek från gemenskapen.

Du kan hitta en databas med åtgärder direkt på [Marketplace] (https://github.com/marketplace?type=actions), min favoritåtgärd är till exempel [Skicka SMS vid fel via Twillio] (https://github.com/marketplace/actions/twilio-sms).

Färdiga exempel
------

Det finns väldigt många exempel [jag publicerar i mina egna arkiv] (https://github.com/baraja-core) där du kan se vilken specifik konfiguration jag använder.

I februari 2021 använde jag till exempel den här konfigurationen för att kontrollera kodstilen i ett projekt:

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

Den här konfigurationsfilen installerar det senaste Ubuntu (`runs-on: ubuntu-latest`), klonar projektets arkiv (`uses: actions/checkout@v2`), installerar PHP (`uses: shivammathur/setup-php@v2`) version 7.4 (`php-version: 7.4`), installerar paketet code-checker och kör sedan jobbet via PHP.

Några ytterligare kommentarer för användare som inte är så erfarna:

- CI startar en virtuell maskin med ett komplett operativsystem varje gång den körs, som kan användas för att anropa kommandon i Terminal precis som till exempel din server.
- För att kontrollera testresultaten används så kallade returkoder som definieras av Linux självt. När ett program returnerar "0" (noll) betyder det att det är lyckat och alla andra siffror betyder misslyckande. Skalskript fungerar till exempel på samma sätt
- När vi vill skriva ett anpassat test eller mer komplex logik kan vi till exempel använda PHP och köra det som en CLI-uppgift (kommandot "php file.php"), som körs och kan göra allt som den har rätt att göra (arbeta med filer, kommunicera över internet, visa på skärmen, ...).
- Medan CI körs registreras all produktion på skärmen och kan ses direkt i webbläsaren.
- CI är inte interaktivt, även om Terminal kan utföra interaktiva operationer tillsammans med användaren, stöder CI inte detta och måste utformas som en uppgift som ibland startar och ibland avslutas.
- En timeout på 1 timme har definierats för att CI-behållaren ska kunna köras. Om allt inte behandlas inom den tiden (t.ex. om programmet stannar upp) kommer hela systemet att avslutas med våld och ett fel kommer att rapporteras.
- GitHub kan köra CI-jobb automatiskt med cron, men ofta körs de inte vid rätt tidpunkt, utan med en fördröjning.
- Du kan också lägga all CI-logik i en Docker-behållare och köra den lokalt på alla maskiner eller på en server, t.ex.
- Om du behöver få tillgång till hemlig information (t.ex. databaslösenord) finns det ett alternativ för att definiera hemliga variabler direkt i GitHub och CI har dem tillgängliga vid körning.
- Det är alltid bäst att distribuera till en webbserver via SSH.
- Den totala körtiden för CI-jobben är begränsad, vanligtvis till 2 000 minuter per månad (ofta räcker detta för dig, jag har aldrig använt det eftersom ett jobb pågår i högst en minut), eller så kan du öka tiden mot en avgift.
- Du kan också vara värd för CI-runners på din server och kringgå minutgränsen, men det är mycket troligt att din server inte kommer att vara mycket snabbare eftersom Microsoft har investerat mycket i Azure där GitHub Actions finns och det körs på mycket kraftfulla servrar.
- Ofta är det praktiskt att installera specifika paket bara för CI (vanligtvis PhpStan och olika säkerhetsfunktioner), så lägg dem i avsnittet `composer.json` i filen `require-dev` så att de inte installeras i ett specifikt projekt som ett obligatoriskt beroende.

GitHub-appar och robotar
-----

Till skillnad från andra arkivvärdar stöder GitHub så kallade GitHub Apps och automationsrobotar. Detta är en stor databas med färdiga robotar som kan utföra automatiserade operationer på dina projekt (även utan CI) och när de stöter på något kommer de till exempel att skicka in ett problem eller skicka en pull request med en korrigering.

Jag använder den personligen och rekommenderar den till dig:

- [Dependabot] (https://dependabot.com) - automatisk kontroll av beroenden i Composer, t.ex. när en ny version av de paket du använder kommer ut och är kompatibla, skickas automatiskt en pull request.
- [Codecov] (https://github.com/marketplace/codecov) - kontrollerar kodtäckning med automatiska tester och visar vilka delar av koden som ännu inte har täckts.
- [Snyk] (https://github.com/marketplace/snyk) - kontrollerar automatiskt säkerhetsbrister
- [Imgbot](https://github.com/apps/imgbot) - automatisk optimering av bilddatastorlek

Många andra program finns på [Marketplace] (https://github.com/marketplace?type=apps).

Fler praktiska tips och tricks
-----

Jag har blivit väldigt förtjust i att använda automatiseringsuppgifter i GitHub.

Jag har automatiserade kontroller i alla mina arkiv, så när jag (eller någon annan i samhället) skickar in en commit får jag återkoppling i realtid om kodkvaliteten (codestyle och PhpStan), säkerheten med mera har överträtts.

Jag har några tester som körs automatiskt varje timme. Om det till exempel finns en säkerhetssårbarhet eller en CVE får jag reda på det senast inom en timme, till och med när det gäller specifika projekt och vad som exakt hände.

Efter att ha testat olika konfigurationer har jag med tiden kommit fram till att jag anser att följande beroenden till Composer är den idealiska minimiuppsättningen:

- `phpstan/phpstan` - kontrollerar koden för korrekthet och logik
- `tracy/tracy` - felrapportering och loggning på hög nivå
- `phpstan/phpstan-nette` - Phpstan-tillägg och specialiteter i Nette
- `spaze/phpstan-disallowed-calls` - ett lysande bibliotek av Michal Spacek som hanterar (o)användning av farliga saker i kod (från glömda variabeldumpningar till möjligheten att köra en användarsträng som ett skalkommando).
- `roave/security-advisories` - kontrollerar installation av osäkra versioner av paket (om en säkerhetssårbarhet hittas i ett visst paket eller om en CVE publiceras kommer detta paket att förhindra installation av den skadade versionen).

Helst bör du ha den grundläggande säkerhetsinställningen inställd så att den automatiskt körs minst var 8:e timme och att felmeddelanden skickas till din e-post. När en projektinstallation misslyckas eller en ny sårbarhet upptäcks vill jag veta det så snart som möjligt och reagera omedelbart.

Kom ihåg att allt fungerar automatiskt, så du kan alltid vara säker på att även om du använder komplicerade processer för att kontrollera säkerheten och andra saker, kostar det dig inte extra arbete och du behöver bara en väl utförd första konfiguration.

För det finns en enorm potential i förebyggande och automatisering!
