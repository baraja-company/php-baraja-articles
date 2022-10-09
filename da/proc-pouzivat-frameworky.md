Hvorfor og hvordan man bruger rammer og biblioteker
===================================================

> id: '5ea6cdde-f67c-4f3f-8871-37b27ee8e23a'
> slug:
> 	cs: proc-pouzivat-frameworky
> 	da: hvorfor-og-hvordan-man-bruger-rammer-og-biblioteker
> 
> publicationDate: '2020-02-16 22:13:46'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '0926b7ec4f59904c008388431ff22eb5'

Der er en berømt vittighed om, at programmører først begynder at bruge frameworks, når de skriver deres eget og opdager, at det ikke fører nogen steder hen. Det sjove ved dette er, at det er sandt. Jeg har selv oplevet det. To gange, endda.

Men selv på forsiden af <a href="https://nette.org">Nette</a> står der:

> Rigtige programmører **bruger ikke frameworks**. De skriver webapplikationer via kommandolinjen direkte til serveren. Dette er en hyldest til dem. For resten af os andre vil Nette gøre vores arbejde meget lettere og sjovere.

Rammernes rolle i applikationsudvikling
-----------------------------------

Jeg er sikker på, at du selv ved det:

Du får en idé til et fantastisk projekt, så du begynder at skrive greenfield-kode direkte i ren PHP ... efter et par timer finder du ud af, at meget af arbejdet stadig er gentagende, og du løser grundlæggende systemproblemer. F.eks. hvordan man opretter forbindelse til en database, hvordan man gengiver en formular, hvordan man validerer data, hvordan man sender en e-mail og meget mere.

Du vil sandsynligvis skrive dine egne funktioner, som du kan bruge til disse opgaver. Hvis du skriver flere projekter på én gang, vil du begynde at dele disse funktioner mellem projekterne i én stor, rodet fil, og du vil løbende justere dem, efterhånden som du får brug for dem.

Og når funktionerne er på et vist udviklingsstadie, så man begynder at bygge et nyt projekt ovenpå dem hver gang og udvikler med det samme, så kan man tale om at have skrevet sit eget framework eller i det mindste et bibliotek.

Hvad er og hvad betyder en ramme
-------------------------

Som vi allerede har vist med et praktisk eksempel fra livet, er et framework en samling af mange funktioner (men ideelt set klasser og objekter), som sparer programmørens arbejde. Når han udvikler, behøver han ikke at tænke over, hvordan han skal oprette forbindelse til en database, men han ved, at det allerede er programmeret et sted, og "det virker bare".

Færdige frameworks er således den samlede intelligens fra hundredvis af mennesker, der har udviklet tusindvis af applikationer over en lang periode for at fejlfinde én løsning og ét økosystem til at løse nye projekter.

Med frameworks får du også et sæt **best practices**, som er måder at løse problemer på, uden at du behøver at tænke over det. Hvis du støder på et problem, er der sikkert nogen, der har løst det før dig, og det er altid bedre at finde en løsning i dokumentationen end at skulle finde ud af det på en kompliceret måde selv.

Abstrakt programmering og indkapslingsprincippet
---------------------------------------------

Veludviklede rammer som Nette giver dig mulighed for at programmere på et **meget højt abstraktionsniveau**.

På denne måde behøver programmøren (ramme-brugeren) ikke at forstå, hvad der foregår internt, og præcis hvordan komponenterne fungerer internt, hvilket sparer en masse tid og kræfter. Han kan fokusere på at løse de reelle problemer i hans applikation og få resultater meget hurtigt.

David Grudl selv (forfatteren af Nette framework) sagde engang, at han designede Nette for at kunne programmere et websted for en kunde på en aften, når han kommer sent hjem fra pubben og skal lancere webstedet om morgenen. Det skyldes primært, at udvikleren er helt væk fra de tekniske ting.

For at dette kan fungere, og for at udvikleren bare kan bruge den færdige ramme som helhed, er det meget vigtigt at bruge <a href="/encapsulation">encapsulation-princippet</a> korrekt.
