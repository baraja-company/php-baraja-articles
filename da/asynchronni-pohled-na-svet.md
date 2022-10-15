Asynkront verdenssyn
====================

> id: '7ed25269-659f-4d17-a642-d07480cbfafa'
> slug:
> 	- null
> 	da: asynkront-verdenssyn
> 
> cs: asynchronni-pohled-na-svet
> publicationDate: '2022-10-15 11:30:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '24d4ec106e7febec5ee84cdf86bd8f28'

Jeg har længe bemærket, at vores verden er asynkron og decentraliseret. Når du indser dette og begynder at tænke over, hvordan du kan bruge det til din fordel, kan du nemt få et helt nyt koncept for, hvordan du skal se på løsningen af komplekse problemer. I dette indlæg vil jeg gerne forklare nogle af de idéer, som jeg allerede bruger. Jeg får dem ikke fra nogen bestemt kilde, det er mere en kombination af mange forskellige erfaringer og mine egne idéer. Disse principper gælder ikke i alle tilfælde.

Fastlæggelse af miljø og mål
-------------------------

Næsten alle de projekter, jeg nogensinde har taget fat på (enten alene eller som en del af et team), har haft et ret specifikt defineret mål. Denne fremgangsmåde giver meget god mening, fordi det er vigtigt at vide, hvad vi vil have. På den anden side mener jeg, at det er umuligt at definere et specifikt mål i et komplekst miljø, og ofte finder vi under gennemførelsen ud af, at vi faktisk ønsker at nå et andet sted hen.

Så i stedet for et specifikt mål opbygger jeg et område med forskellige mål, hvor der er alternativer, eller selv et helt nyt, isoleret og uafhængigt mål kan være det rigtige mål, hvis det giver mening.

Eksempler fra praksis:

- Jeg er nødt til gradvist at udvide til andre markeder. Et af de delmål, som jeg opdagede under gennemførelsen, kan være maskinoversættelse af internettet.
- Jeg skal hente 6 forsendelser forskellige steder i Prag, og jeg kender ikke den korteste rute. Jeg ønsker ikke at finde ud af det på en kompliceret måde, så jeg går bare mod det fjerneste punkt og ændrer min plan i retning af andre ruter undervejs. Når jeg f.eks. skifter i Anděl, beslutter jeg mig for at tage den sporvogn, der ankommer først, hvilket dynamisk påvirker den næste ruteplan.
- Jeg er nødt til at skrive dette indlæg, men jeg har ikke en time i træk. Derfor kan jeg skrive den i dele, mens jeg kører i metroen, som enkelte kapitler. Jeg vil derefter foretage den endelige sammenlægning til denne formular i fremtiden.
- Jeg ved ikke, hvordan algoritmen fungerer til at beregne indløsningen af merchandise for min kunde, som jeg skal omprogrammere. Så jeg stiller forespørgslen til flere personer på samme tid og løser noget andet, mens jeg gør det. Asynkront over to dage vil der komme flere forskellige svar ind, som jeg først træffer en beslutning på baggrund af senere.
- Jeg skal slippe af med PHP på serveren i lang tid, så jeg omskriver gradvist en side ad gangen til React. Jeg flytter gradvist webstederne til skyen og bygger dem på statisk genereret Nect.

Ingen af destinationerne er endelige destinationer. Hvis du arbejder med en overflade og ikke med et punkt, er der større sandsynlighed for at ramme den. Samtidig fungerer motivation godt her, for det værste, der kan ske, er at nå sit mål og derefter ikke have noget at se frem til i fremtiden.

Meget af det er også vigtigt at sige, at for at have en lignende tankegang skal man have et forholdsvis stabilt miljø, hvor man kan begå fejltagelser. Denne fremgangsmåde kan ikke garantere en specifik tidsfrist for at løse en specifik enkelt opgave. På den anden side kan den optimere løsningen af så mange parallelle opgaver som muligt på så kort tid som muligt, men ikke nødvendigvis i den rækkefølge, de ankom i køen.

Princippet kan f.eks. sammenlignes med, hvordan beregninger fungerer i parallelprogrammering. Kort sagt opdeler du opgaverne i individuelle tråde, der løser forskellige delopgaver på én gang, og når du har fået alle svarene, kan du sammensætte løsningen.

Udrulning af nye softwareversioner
--------------------------------

Jeg kæmper meget med dette overalt.

Den måde, som softwareudvikling normalt håndteres på, er, at du har en stabil version, som kører i produktion for brugerne, så har du et testmiljø, hvor der udvikles nyt, og en gang imellem flytter du nyhederne ud af testmiljøet til produktion, hvilket kaldes en udgivelse. Denne proces er relativt sikker og langsom.

Da jeg gradvist er gået over til en mikro-frontend-arkitektur, hvor applikationens frontend (det brugeren ser) er fuldstændig adskilt fra backend (hvor beregningen og databehandlingen foregår), kan udgivelsesprocessen fungere anderledes.

Jeg har stadig ét produktionsmiljø, der altid fungerer. Herfra oprettes der en ny filial for hver opgave, der vedrører en specifik funktion. Da jeg bruger Vercel (en meget god cloud-udbyder) til at hoste frontend'en, kan jeg have en brugerdefineret URL for hver udviklingsgren.

Når en ny funktion er testet, bliver den straks slået sammen og implementeret i produktionen i en asynkron proces. Derfor kan det ofte ske, at der måske er 15 nye versioner, der implementeres hver dag.

Meget af dette hænger sammen med en idé, som jeg fik på LMC - "En funktion, som du leverer til en kunde i morgen, selv om den er ufuldkommen, er af meget større værdi for dem end en funktion, der er perfekt fejlfinding, men som først vil være tilgængelig om et år". Men det kan kun fungere, hvis du har en måde at distribuere nyhederne hurtigt på - typisk webudvikling.

Det, jeg holder meget af ved Next.js framework (bygget på Recto) og Vercel, er princippet om, at du forbinder dit GitHub-repository direkte til Vercel, og med hvert commit er der automatisk build, test og derefter deployment til produktion, når alt er i orden. Så udvikleren behøver ikke at bekymre sig om noget som helst, og appen implementeres hver time uden nogen anstrengelser. Det er meget vigtigt at formalisere rutineprægede ting og derefter automatisere dem.

Udgivelse af indhold
----------------

Jeg har nedbrudt højere snesevis af emner og indlæg med mig i Apple Notes. For hvert emne får jeg hele tiden nye ideer, som jeg noterer og sorterer dem efter tags. Når der genereres flere noter til et emne, konverterer jeg dem til kapitler og konverterer derefter gruppen af kapitler til artikler og FB-indlæg.

Egenskaber ved dette princip:

- Jeg ved ikke på forhånd, hvor mange artikler jeg nogensinde vil udgive,
- Men jeg ved også, at der kan være mange,
- Jeg er i stand til at sikre, at hvert indlæg er fuld af information, indsigt og idéer, der er modnet over tid (dette indlæg tog ca. 3 måneder at modne),
- Det er bæredygtigt, og jeg vil nyde det, fordi jeg ikke behøver at skrive en masse tekst på én gang og risikere at glemme noget.

Og så udgivelsesprocessen.

Jeg offentliggør meget indhold stille og roligt på nettet først, så Google opdager det først, og siden bliver indekseret. Når det har en god dækning af søgeord, og jeg er tilfreds med det generelt, kommer emnet f.eks. på de sociale medier.

For mange emner ved jeg, at de ikke vil være interessante og snarere vil irritere dig. Men samtidig vil jeg gerne have dem online, fordi nogen måske vil søge efter dem i fremtiden. I så fald forbliver artiklen kun på nettet. Et eksempel på disse artikler er de forskellige overlejringsartikler, som linker til hele emneområdet, så jeg har så mange emner som muligt dækket på webstedet. Forsideartikler er ofte mere tekniske og kedelige. Eller de er maskingenererede kategorier, hvor jeg bare organiserer flere artikler i emner og dermed dækker så mange nøgleord som muligt, som nogen måske vil søge efter.

Viden, uddannelse, testning
------------------------------

Jeg kan godt lide at lege med teknologier, hvor det ikke er klart fra starten, hvad de vil være gode til en dag.

Som maskinoversættelse. Ved første øjekast giver det ikke mening at oversætte snesevis af artikler til fremmedsprog til læsere, som jeg ikke har kontakt med. På den anden side kan det være et af de skridt, der skal tages for at træffe en beslutning i fremtiden, som forudsætningerne skal være opfyldt.

Generelt er der ingen, der ved, hvordan fremtiden vil se ud. Derfor giver det for mig enormt god mening at dække så mange muligheder som muligt, som man ønsker at forstå, i det mindste overfladisk, og som man måske vil tage fat på i fremtiden. Det er godt at tænke på trin ikke bare som en løst opgave, men som en evig udviklingsproces, der ikke har noget slutmål.

Virker denne fremgangsmåde til levering af tilpassede projekter?
--------------------------------------------------------

Det meste af tiden, nej.

Du skal have en klient, som er din partner, og I ønsker begge at levere den bedst mulige løsning, selv om I på forhånd ved, at I ikke finder ud af, hvad det er, før det er slut.

I vores branche kaldes det agil udvikling, og disse principper bygger på det. På den anden side har jeg konstateret, at agil udvikling, som jeg kender den, ikke passer mig direkte, og jeg kan godt lide at finde på alternative metoder.

Med agilitet ser jeg meget af princippet om engagement i form af hvem der løser hvad inden for et bestemt sprint. Jeg synes, det giver mere mening at opbygge miljøet, så man har en stor bagatel af opgaver, der bliver løst på baggrund af det, der er mest nødvendigt lige nu, eller som jeg har den mentale kapacitet til at gøre. Når jeg f.eks. er på farten, har jeg en tendens til at tage fat på mere udviklingsorienterede opgaver, som jeg kan udføre udenfor uden en computer. Når jeg er på kontoret igen, tager jeg fat på mere algoritme- og matematiske opgaver, fordi jeg kan fokusere fuldt ud og investere en masse mental energi.

Jeg anbefaler ikke dette princip, hvis du er ved at oprette en helt ny e-butik, eller hvis din virksomhed er ved at gå konkurs. Du har brug for et stabilt miljø, der kører af sig selv, og så er du bare en juvel. Men samtidig ved du også, at hvis du ikke gør noget, vil dit stabile miljø en dag ophøre.
