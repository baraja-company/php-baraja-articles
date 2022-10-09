Algoritme for søgemaskiner på internettet - Knap en crawler
===========================================================

> id: '5ad042bf-7fda-4b2e-a38c-762169d0b594'
> slug:
> 	cs: vyhledavac-barely-a-crawler
> 	da: algoritme-for-sogemaskiner-pa-internettet-knap-en-crawler
> 
> publicationDate: '2016-09-11 12:00:00'
> mainCategoryId: '367f936c-073f-44bd-b399-30738e93137a'
> sourceContentHash: '5daf3854688c39f9e0071a93ab11dd4e'

I dagens lektion vil vi diskutere datatønder, deres struktur, StopSlovas og til sidst vil vi beskrive crawlere.

Datatønder
-------------

Dette er en særlig datatype, der ligger på flere servere samtidig i flere kopier. Det er normalt dataintensive filer på flere hundrede GB, som er langsomme at læse (hvilket er grunden til, at de er opdelt i dele) og næsten umulige at redigere. Hvis vi ønsker at foretage bare en minimal ændring, skal vi beregne hele tønden på ny. F.eks. kan søgemaskinen Seznam højst genberegne datatønder hver anden dag eller uge, mens Google genberegner dem hver anden time (og kun nogle dele, aldrig det hele på én gang).

Tønder indeholder ord og deres forekomst på internettet. Man kan sige, at der er tale om rå data for søgeresultater i en endnu ikke sorteret form (nogle gange er de delvist sorteret efter relativ betydning) og forberedt på forhånd. Selve søgningen finder sted længe før brugeren stiller søgeforespørgslen, og det er her, at alle resultaterne bliver udarbejdet. Selve søgningen ville være umulig i realtid, så søgemaskinen læser grundlæggende de resultater, der allerede er forberedt her (søgningen udføres af indekseringskomponenten, som vil blive behandlet i et separat kapitel).

Tønderne indeholder alle de grundlæggende oplysninger, der er nødvendige for at opbygge søgeresultater for hvert ord, der forekommer på internettet, så der skal tages højde for problemer med ydeevnen, når de læses i rækkefølge. I praksis er der en tendens til at lagre tønder i partier på flere servere og også i RAM for at opnå hurtigere adgang. Google-søgemaskinen læser f.eks. slet ikke tønderne fra disken, men opbevarer dem helt og holdent i RAM og beholder kun en kopi af dem på disken i tilfælde af, at data går tabt fra RAM.

Store søgemaskiner med tilstrækkelige ressourcer har også såkaldte "gyldne tønder", som indeholder de vigtigste sider på internettet, og som gennemsøges i tilfælde af en stor søgning. Hvis f.eks. flere søgeservere går ned, kan guldtønderne midlertidigt tage sig af søgninger. Søgemaskinen finder måske ikke alle resultater, men i det mindste vil søgningen ikke blive afbrudt helt, men kun antallet af søgte dokumenter vil blive midlertidigt begrænset. Det er også nødvendigt at opdatere tønderne regelmæssigt, da antallet af sider på internettet stiger konstant. Da tønderne normalt er i størrelsesordenen gigabyte, er det ikke muligt at foretage inkrementel vedligeholdelse, men det er nødvendigt at genopbygge det hele en gang hver gang. Derfor har søgemaskinen en ekstra tønde, hvor alle data er i form af en stor tabel, hvorfra tønderne bygges op - Google bygger f.eks. tønderne op ca. hver 12. time. Den store udfordring er netop, at tønderne skal være i det mindste delvist sorteret og afspejle den faktiske tilstand på internettet. Så indtil søgemaskinen opdaterer tønderne, vil brugeren ikke finde nye websider, før den har opdateret tønderne.

Tøndekonstruktion
----------------

I praksis gemmes en tønde som en stor tekstfil, der indeholder ID'et for hvert dokument, hvor søgeordet forekommer, og positionen for det aktuelle søgte ords forekomst.

Et eksempel på en meget lille tønde med tre sider:

```txt
[1:5,8,12,27,30;5:1,3,6;12:4,8,10]
```

Denne prøve tønde indeholder sider med identifikatorerne `1`, `5` og `12`. De tilhørende tal angiver ordets placering i dokumentet. En sådan tønde opfanger kun forekomster af et enkelt ord, så hvert ord på internettet har sin egen tønde. Når du søger efter flere nøgleord, er det nødvendigt at læse flere tønder (en anden for hvert ord) og derefter udføre operationerne i henhold til det binære træ (mere i de foregående kapitler).

Krydsningen sker normalt ved at gennemgå et af dokumenterne og søge i det andet. Hvis tønderne er store, kan de ikke læses helt, og søgemaskinerne kan ofte kun læse en del af dem og skal derefter returnere resultatet, så det giver mening at sortere tønderne i det mindste delvist, så de vigtigste sider (som brugeren sandsynligvis leder efter) ligger i begyndelsen af tønden, og alt andet ligger i slutningen af tønden.

StopLead
---------

Du tænker måske, at nogle ord kan være meget store og derfor vanskelige at arbejde med. F.eks. findes ordet `a` eller ordet `1` i mere end halvdelen af dokumenterne, og alligevel har de ingen betydning for søgeren (fordi de sjældent søges efter og kræver flere omgivende ord). Sådanne ord kaldes stopord.

Problemet med StopWords er, at de ikke alle kan søges efter, og at søgemaskinerne derfor kun viser en forvrænget virkelighed af, hvordan internettet faktisk ser ud for søgeordet.

Et eksempel på en søgning med StopSlovy er forespørgslen `Prague 1`.

Søgemaskinen giver kun `3.020.000 resultater` for denne forespørgsel. I virkeligheden findes der dog mange flere af disse websteder. Af hensyn til ydeevnen gennemsøges de dog ikke alle sammen.

Søg adgangsmuligheder med StopSlovy:

- De indekserer dem slet ikke og laver ikke tønder til dem (det gør små søgemaskiner)
- Indeksering af dem, men kun lagring af relevante sider i tønder (dette gøres f.eks. af Google og Seznam)
- indeksere dem som almindelige ord og oprette komplette tønder (denne løsning har det problem, at tønder nemt kan overstige størrelsen af en almindelig harddisk og derfor ikke kan gemmes og kan tage sekunder at læse - denne løsning bruges derfor ikke i praksis eller kun til små og specialiserede søgemaskiner)

Generelt er der ikke nogen universelt korrekt tilgang, og selv Google bruger den ikke perfekt. Dette er et så stort problem, at det kan tage hele livet at løse det. I forbindelse med denne artikel er denne generelle beskrivelse dog tilstrækkelig.

Crawler (robot, også edderkop)
---------------------------

En crawler er et program, der systematisk gennemtrawler internettet og holder styr på, hvilke ord der forekommer på hvilke sider, og som også indsamler supplerende oplysninger (også kaldet metainformationer). En crawler kører normalt på mange servere, fordi crawling af internettet er krævende med hensyn til netværksbåndbredde, og derfor skal mange sider crawles samtidig. Google anslår, at op til 30.000 sider bliver gennemgået samtidigt på et hvilket som helst tidspunkt.

Før Crawler åbner en side, kigger Crawler på en database over de adresser, den planlægger at gå til i fremtiden. Hvis den allerede har været på adressen, opdaterer den kun sine registreringer (den besøger store websteder med få timers mellemrum, nyhedssider med få minutters mellemrum og almindelige websteder højst en gang om måneden).

Hvis robotten når frem til en ny adresse, som den ikke kender, vil den se på de indeholdte links til andre dokumenter (sider). For hvert enkelt link beregnes dets betydning, og hvis det er et vigtigt link, kommer det også senere på siden.

List gennemtrawler f.eks. kun nogle få niveauer af links på hvert websted på denne måde, mens Google forsøger at gennemtrawle hele webstedet, også uvæsentlige dokumenter, hvilket gør Google til den mest omfattende søgemaskine på internettet.
Robotten forsøger også at rangordne siden, mens den kravler rundt og beregner dens "rang", som er en numerisk repræsentation af, hvor vigtig siden er på internettet. Tidligere har Google anvendt PageRank, som har et interval fra 0 til 10. Google beregner kun PageRank 10 for sig selv og for websteder som microsoft.com eller w3c.org. I dag beregner den værdien på en anden måde (kunstig intelligens og vektormatematik).

Den højeste PageRank i Tjekkiet er Seznam.cz med en værdi på 7. Ranken har stor indflydelse på, hvor ofte en robot vil besøge siden, og hvor højt den vil ligge i søgeresultaterne. Rangeringen beregnes udelukkende ud fra backlinks, der peger på den ønskede side.

Crawleren gør ikke andet med dataene, den downloader dem blot fra internettet og gemmer dem i en database, hvor de venter på indekseringsprocessen.
