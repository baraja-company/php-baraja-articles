Refactoring af et ældre PHP-projekt - hvordan du indhenter teknologiske gæld
============================================================================

> id: '8f37305a-e9dd-4b1e-9f53-04f0fca648f7'
> slug:
> 	cs: refaktoring-legacy-php-projektu
> 	da: refactoring-af-et-aeldre-php-projekt-hvordan-du-indhenter-teknologiske-gaeld
> 
> publicationDate: '2021-05-11 20:45:00'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: b13673b23a86bc82a88636e4a9464b4b

Når jeg rådfører mig med kyndige og erfarne projektejere, støder jeg ofte på spørgsmålet om et digitalt projekts bæredygtighed på lang sigt. Mange store projekter, der er længere end 3 års udvikling, begynder at blive internt forældede og ikke længere bæredygtige - forestil dig nu et team af udviklere med forskellige niveauer af viden, erfaring og vigtigst af alt, flid.

For at holde dit digitale projekt teknisk set på TOP-niveau skal du have:

- **Kompetente udviklere og en projektleder**, der har en god kommunikation, og som alle ved, hvad de laver, og hvordan det påvirker de andre,
- **Funktionelle værktøjer og arbejdsgange**, som hele teamet kender og tilpasser deres arbejde til andre i overensstemmelse hermed,
- **Automatiserede opgaver**, der giver en daglig rutine, som er så let at glemme, at det er ekstremt vigtigt.

Hvis du er i gang med at udvikle et stort projekt, har du det simpelthen ikke let. Det er vigtigt at gøre tingene rigtigt, men det er endnu vigtigere at gøre de rigtige ting.

Hvad er refactoring, og hvorfor har du brug for det?
---------------------------------------

Begrebet refactoring omfatter en række aktiviteter, der ændrer udseendet og den interne logik i et programs kode uden at påvirke det omgivende miljø. Du kan tænke på refaktoreringsprocessen som en juleoprydning - det forbliver det samme, men det er meget mere organiseret, og de unødvendige ting er smidt væk. Med refactoring er det også vigtigt at huske på, at det ikke er en engangsbegivenhed, men en langsigtet proces, som du skal gentage i regelmæssige cyklusser. Hvis du ikke gør det, kan du risikere alle mulige mærkelige fejltagelser, økonomiske tab, problemer med onboarding og skrigende skuffelser.

Hvis du kan holde projektet stabilt og opdateret, vil du få nogle store fordele:

- Projektet vil være **sikkerere**. Der udgives regelmæssigt rettelser til fejl og sikkerhedssårbarheder i de biblioteker, du bruger. Du skal tage fat på sikkerheden, det er en af de grundlæggende livsfunktioner i et sundt projekt.
- Koden og den interne arkitektur vil blive enklere og bedre gennemtænkt. Du vil være bedre i stand til at finde fejl, og mange fejl kan endda forebygges.
- Med forenklet kode kan du også ansætte juniorudviklere for at <a href="https://www.youtube.com/watch?v=1wZtdIb-Ppk">reducere </a> dit samlede budget betydeligt.
- Du vil måske opdage, at du gør nogle ting for komplicerede og overkomplicerede - projektet vil blive forenklet i det hele taget.

For at holde sig på sporet med et stort digitalt projekt er du nødt til at løbe. Men du ønsker at vokse.

Hvordan man refaktoriserer sikkert
-------------------------

Enhver refaktorering er en stor satsning, som måske ikke betaler sig.

Jeg sørger altid for et stabilt miljø, længe før jeg går i gang med refaktorering.

Af hensyn til stabiliteten har du især brug for **funktionel fejllogning**. Til enkle projekter har du blot brug for <a href="https://tracy.nette.org">Tracy</a>, som logger fejl i en HTML-fil. Til mere avancerede projekter kan du bruge værktøjer som <a href="https://sentry.io/welcome/">Sentry</a> eller <a href="https://rollbar.com">Rollbar</a>. Personligt bruger jeg Tracy til alle projekter, andre værktøjer afhænger af projektets art.

Omkring en måned før refaktoriseringen begynder jeg at spore fejl og opretter et regneark med kendte fejl, som jeg kan fokusere på senere. Det er altid vigtigt at vide, hvilke fejl der blev introduceret ved refaktorisering, og hvilke fejl projektet allerede indeholdt tidligere. Dette vil gøre det nemmere at forsvare resultaterne af arbejdet over for kunden.

Når jeg takket være loggen ved, at jeg arbejder i et relativt stabilt miljø, kan vi gå i gang med det hårde arbejde. Andre overskrifter omfatter beskrivelser af metoderne efter, hvor stor risiko der ligger bag dem.

Fastsættelse af kodningsstil og kodningsstandard
-----------------------------------

De fleste projekter har ikke en konsekvent kodeformateringsstil. Det er en stor fejltagelse. Et velskrevet projekt ligner én persons projekt, hvor alt er skrevet ens.

Grundlæggende formateringsfejl er godt korrigeret af <a href="https://doc.nette.org/en/3.1/code-checker">Nette Code Checker</a>, som jeg regelmæssigt kører over hele projektet.

Kodningsstil dækker på den anden side, hvordan koden er formateret og indrykning af de enkelte sprogudtryk. <a href="https://www.php-fig.org/psr/psr-12/">PSR-12</a>-standarden bruges meget i PHP-verdenen, og rigtig mange andre standarder er baseret på den. Jeg anbefaler at bruge.

Nogle projekter kombinerer <a href="/spaces-and-tabs">tabs og mellemrum</a> på en besværlig måde. For effektivt at undgå at bryde whitespace-konventionen (og andre fejl) skal du oprette en fil `.editorconfig` på projektroden, som fortæller din editor, hvordan nyskrevet kode skal formateres.

Personligt bruger jeg denne konfiguration:

```txt
root = true

[*]
charset = utf-8
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true

[*.{php,phpt}]
indent_style = tab
indent_size = 4
```

Ret også alle <a href="/apostrophes-and-carriage-notes">carriage-notes til apostroffer</a>. Det kan gøres automatisk.

Du kan også kontrollere formateringen af din kode automatisk, jeg har udarbejdet en <a href="https://github.com/baraja-core/sandbox/blob/0db1f41068b6cedf79500c6a8b0ac26eae94a8eb/.github/workflows/coding-style.yml">fuldt funktionel demo på GitHub</a> til dette formål. Hvis du også har brug for automatisk korrektion af koden, kan du gøre det med det samme værktøj. PhpStorm er også rigtig god til automatisk at rette kode direkte, selv i bulk over hele projektet.

Hvordan du retter kodningsstilen for store projekter
----------------------------------------------

I store projekter anbefaler jeg, at du laver automatisk kodeformatering for et modul ad gangen, da dette kan skabe et stort antal konflikter i Git. Derfor er den bedste strategi at rette så mange linjer som muligt med et enkelt stort commit, skubbe dette commit direkte til master og derefter distribuere ændringen til de andre grene.

Hvis konflikterne er for store, giver det mening at formatere koden på master først og derefter igen på hver stor gren, hvor der er mange ændringer. Meget mange ændrede linjer vil ophæve hinanden (lav en hurtig fremadspoling), så du vil løse meget få konflikter eller nogle gange ingen konflikter.

Hvis du ikke gør noget, vil koden stadig være dårlig. Iterative omskrivninger over mange år fungerer bestemt ikke, fordi hver sammenlægning medfører et behov for at rette snesevis af linjer, hvor der faktisk ikke er sket nogen ændring, og det komplicerer unødigt både kodegennemgang og potentielle ændringer, der skal tilbageføres.

PhpStan - statisk kodeanalyse og korrektion af typefejl
------------------------------------------------------

Før du går i gang med en omfattende logikrettelse, er det meget vigtigt at gennemgå og rette **grundlæggende funktioner i projektets livscyklus**. Det drejer sig bl.a. om de indlysende ting, som man forventer af ethvert projekt, men som alligevel ikke altid opfyldes.

For eksempel:

- Alle klasser, metoder og funktioner skal eksistere
- Arv af klasser må ikke brydes
- Klasser skal implementere den anvendte grænseflade eller forfædregrænseflade. Samtidig må du ikke arve `final`-klasser
- Du må ikke kalde usikre funktioner såsom `eval()`, `shell_exec()`, `var_dump()` osv. Og hvis du alligevel ringer til dem, skal det udtrykkeligt angives i kommentaren
- Du skal altid fange undtagelser og ikke lade hele programmet gå ned

Løsningen på dette problem er at installere <a href="https://phpstan.org">PhpStan</a> i projektet og rette det **i det mindste til niveau 1**. Ja, det er hårdt, og ja, det er meget arbejde. Men hvis du ikke gør det, bliver hver refaktorering russisk roulette, og udvikleren håber bare, at skaden bliver så minimal som muligt.

PhpStan's basisniveau kræver f.eks. ikke, at datatyper og andre formelle ting findes overalt, hvilket vi vil tage fat på i fremtiden. På den anden side er det ikke nødvendigvis tilfældet, at en funktion eller metode returnerer en bestemt datatype, men hævder noget andet i typehintet.

Rektor - sikker iterativ reparation
-----------------------------------

Et velkendt programmeringsdiktum siger, at før vi tilføjer en ny fejl til systemet (f.eks. ved at kaste en undtagelse), bør vi først tilføje fejlen som en advarsel og først derefter gøre den til en fatal fejl, hvis den ikke viser sig i lang tid.

Det er det samme med datatyper. Når jeg refaktoriserer ukendt kode, lader jeg automatiske værktøjer som <a href="https://getrector.org">Rector</a> tilføje kommentar-annotationer til koden først, hvilket ikke ødelægger noget, men hjælper med at tydeliggøre, hvad der skal være hvor senere. Disse kommentarer bliver derefter lyttet til af PhpStan, som kan bruges til at kontrollere, at vi ikke har ødelagt noget, og at det er en sikker ændring.

Jeg tilføjer normalt kommentarer til egenskaber og argumenter i én commit, venter derefter i måske en måned, og når alt fungerer på lang sigt, tyer jeg til at omskrive til faste datatyper på steder, hvor der ikke har været problemer på lang sigt.

Se artiklen <a href="https://tomasvotruba.com/blog/2019/07/29/how-we-completed-thousands-of-missing-var-annotations-in-a-day/">Hvordan vi afsluttede tusindvis af manglende @var-annotationer på en dag</a>.

Du kan få mere at vide om <a href="https://github.com/rectorphp/rector">Rector på GitHub</a>.

Opdatering af afhængigheder
----------------------

Før du går i gang med at opdatere afhængigheder på pakker eller endda PHP-versioner, er det vigtigt at undersøge og lære at bruge <a href="/github-actions-best-ci-for-2021">automatiserede tests</a>.

Hvis testene fungerer, kan vi gå til <a href="/composer">Composer</a>-værktøjet for at udføre opdateringen. Hvis du kan, kan du få hjælp af en <a href="https://dependabot.com">Dependabot</a> robot, der automatisk opdaterer dit projekts `composer.json` og kan kontrollere for kompatibilitetsændringer.

Hvis du har et stort antal ændringer på vej, skal du altid foretage dem langsomt og øge dem version for version. Opdater aldrig mange pakker på én gang. Efter hver opdatering skal du scanne hele projektet med PhpStan og rette fejl. Det er en lang proces, der tager flere timer, men der er meget på spil.

Hvor skal vi hen næste gang?
-------

De næste skridt er individuelle afhængigt af projektets type og status. Generelt er veludformet kode skrevet af erfarne udviklere generelt meget bedre vedligeholdt end kode købt billigt af en junior, der har arbejdet på et mediebureau, som så at sige har webudvikling mere som en sidegevinst.

Vi krydser fingre! Det bliver hårdt, men du skal nok komme igennem det.
