Algoritme for søgemaskiner på internettet - Træer og StopLead
=============================================================

> id: '11ec73f2-e505-4338-89aa-8307a55b7ba0'
> slug:
> 	cs: vyhledavac-stromy-a-stopslova
> 	da: algoritme-for-sogemaskiner-pa-internettet-traeer-og-stoplead
> 
> publicationDate: '2016-09-11 10:00:00'
> mainCategoryId: '367f936c-073f-44bd-b399-30738e93137a'
> sourceContentHash: d011d2d6943271ca5e45c3d3cdf03d61

Der tilføjes 5 millioner nye sider til internettet hvert sekund, og denne hastighed stiger hele tiden. For at skabe orden i dette enorme hav af informationer og finde noget i det findes der søgemaskiner. Det følgende arbejde har til formål at introducere spørgsmålet om søgning og at forklare hele processen fra oprettelsen af en ny side til at finde den i en søgemaskine.

Det er ikke let at finde og sortere et sæt af milliarder af dokumenter. Google alene har brug for 300.000 webservere for at klare denne opgave på få timer. Faktisk finder søgningen efter din forespørgsel sted længe før du overhovedet stiller den. Google har allerede de søgeresultater, som du vil bede om i de kommende dage, gemt i hukommelsen.

Søgemaskinearkitektur
------------------------

<img src="{$baseUrl}/images/full-text-schema.png" alt="Fælles søgemaskinearkitektur for op til 50 millioner dokumenter" class="w-100 mb-3">

En korrekt designet søgemaskine indeholder mange komponenter, hvoraf der er i størrelsesordenen flere hundrede. Dette diagram beskriver de mest grundlæggende elementer, som en søgemaskine som minimum skal indeholde for at kunne fungere. Dette er en gammel og ikke længere anvendt arkitektur, som fungerede indtil omkring 2005, da der kun var et par millioner dokumenter på nettet. Denne arkitektur giver ikke mulighed for en "uendelig" mængde indhold og heller ikke mulighed for at søge-serverne (basissøgning) kan gå ned i tid. Hvis en enkelt komponent er ude af drift, vil hele systemet ophøre med at fungere, og søgemaskinen vil ikke være tilgængelig.

En fuldstændig beskrivelse af de aktuelle tendenser i forbindelse med udformningen af nye arkitekturer ligger uden for rammerne af denne artikel, da disse normalt er forretningshemmeligheder for store virksomheder. Formålet med denne artikel er at forklare, hvordan denne grundlæggende arkitektur fungerer. De enkelte komponenter forklares i detaljer i de følgende kapitler.

Indtastning af forespørgsler
------------

Den mest synlige komponent, selv for almindelige brugere, er forespørgselsindtastningen, som i øjeblikket oftest er repræsenteret som et tekstfelt. Indtastningsfeltet er placeret på en særlig side, der normalt kun er beregnet til indtastning.

I den næste fase indtaster brugeren søgestrengen og sender formularen til søgemaskineserverne og indleder dermed kommunikationen med udleveringskomponenten, som undertiden også kaldes hovedsøgning. Dispenseringsservere er bygget til at håndtere store belastninger fra brugerne og skal kunne håndtere alle søgende på én gang.

Arkitekturen for en dispatch-server er normalt en simpel Apache-server med en grundlæggende installation og fremragende netværksbåndbredde. Når en forespørgsel kommer ind på serveren, behandles den gennem regressionsbeslutningstræer, og der oprettes et "forespørgselskort", som indfanger den komplette semantik omkring de andre betydninger, så det bliver klart, hvad brugeren faktisk leder efter. Metoder til omskrivning af forespørgsler ligger for langt uden for rammerne af denne artikel, så vi vil blot vise generelle beskrivelser af, hvordan omskrivning kan se ud og ikke kan se ud.

Overvej følgende forespørgsel:

```txt
"O pejskovi a kočičce"
```

Denne forespørgsel konverteres til et binært træ, der indfanger dens semantiske essens:

<img src="{$baseUrl}/images/fulltext-tree.png" alt="Symbolsk parsetrædiagram" class="w-100 mb-3">

Hele pointen med et parse tree er at fastlægge, hvordan en søgemaskine vil se på søgeresultater. Dette træ sendes sammen med forespørgslen til de ekstra søgeservere (i denne artikel benævnt "basissøgning"), hvor der søges efter individuelle nøgleord (indekslæsning) og derefter sorteres efter regler (ved hjælp af operationer).

| Operatør | Sorteringsoperationer |
|----------|------------------
| AND | Krydsningspunkt |
| OR | Sum | Sum |
| NOT | Komplement |

Der er ingen egentlig sortering af søgeresultater, denne komponent udfører blot hurtige binære operationer på store datamængder (ofte millioner af poster) og sammenligner i bund og grund blot ID'erne på de enkelte dokumenter.

Hvordan selve sorteringen af søgeresultaterne på resultatsiden fungerer, vil blive gennemgået i de følgende afsnit. Output-serveren bruges simpelthen til at kombinere en masse data fra forskellige søgeservere for at øge den samlede ydeevne og sprede belastningen, og derefter føres de fundne værdier ind på resultatsiden (dette kaldes en "webside"), som indeholder sammensatte oplysninger fra snesevis af servere.

Omskrivning til et binært træ
-----------------------

Som nævnt i det foregående kapitel er hele søgningen bygget på binære træer, der fortæller dig, hvordan resultaterne vil se ud, og hvad du skal lede efter. Hele "logikken" og "intelligensen" i en søgemaskine er direkte afhængig af kvaliteten af det program, der skaber træet - nemlig deskriptoren.

Et korrekt konstrueret træ bør indeholde alle de nødvendige metainformationer om forespørgslen i en sådan form, at det let kan behandles selv ved hjælp af sekventiel disklæsning og bør eliminere tilfældige adganger til hukommelsen, så det indeholder normalt de enkelte søgeord (fra brugeren), deres transskriptioner (og stavemåder) og sidst men ikke mindst forbindelserne mellem ordene, dvs. deres relative afstand i teksten.

Metoder til transskription af forespørgsler
---------------------

Den mest grundlæggende måde at transskribere en forespørgsel på er at opdele den i individuelle ord (i henhold til mellemrummene), som der vil blive arbejdet videre med. Det andet trin er at søge efter StopWords og erstatte dem med en variabel pointer (for, som vi vil vise senere, er der ingen grund til at søge efter StopWords overhovedet).

Lad os have følgende forespørgsel: "Om en hund og en kat", hvis transskription i sin mest grundlæggende tilstand ser således ud:

```txt
"O (and) pejskovi (and) a (and) kočičce"
```

Det andet trin er at fjerne ord, der ikke har nogen relevans for søgningen. Selvfølgelig er f.eks. ordet "about" eller "and" meningsfuldt for os mennesker, men ikke for en databasesøgning, fordi det kan erstattes af et andet ord, og resultaterne kan opregnes. Målet bør ikke være at finde et bogstaveligt match mellem forespørgslen og indekset, for det ville føre til dårlige resultater, fordi ord i tekst på internettet ofte har forskellige stavemåder, og denne metode forsøger at finde resultater i andre former end dem, vi har fået fra brugeren. Dette er derfor det første tegn på en "intelligent" søgemaskine.

Disse meningsløse ord kaldes ofte "stopord", og alle søgemaskiner bør have et indeks over sådanne ord, og dette indeks bør opdateres ofte (manuelt).

```txt
["pejskovi (and) * (and) kočičce"]
```

På dette tidspunkt leder vi efter 2 forskellige ord ("hund" og "kat") med et andet ord imellem (internt markerer vi det med en stjerne).

Formålet med denne ændring er ikke at øge kvaliteten af søgningen, men at øge ydeevnen. Det skyldes, at når vi søger efter et ord, der er for hyppigt forekommende, sker der en hurtig belastning, og denne ændring hjælper processen - især ved ikke at søge efter et ord, der alligevel vil være tilgængeligt på den position i de fleste dokumenter.

Det er også vigtigt at bemærke, at vi ikke altid kan foretage denne justering (udelade et ord), så hver søgemaskine bør føre et separat indeks over sætninger, der findes på internettet, for at kontrollere, hvilken justering der fører til gode resultater, og hvilken der ikke gør. Det sker dog af og til, at en søgemaskine foretager denne justering, selv om den ikke har sætningen i sit indeks - især når en bruger søger efter et vigtigt ord, som forventes at have vigtige sider i de første positioner, som tilsidesætter denne "fejl", fordi brugerne normalt alligevel efterspørger dem.

Det sidste skridt er at arbejde med det engelske sprog og "rense" forespørgslen for unødvendige tegn. For eksempel forventer brugeren normalt kun resultater på ordet "vaskemaskine" i forespørgslen "vaskemaskine", og vi kan ignorere kommaet.

De nøjagtige metoder til, hvordan dette system fungerer, er ikke kendt, og hver søgemaskine har sin egen. Man mener, at det for det meste blot er en statistisk model, der foretager disse justeringer på baggrund af viden om milliarder af tekster i databasen.

Engelsk transskription er også en videnskab i sig selv, så her vil vi også kun give en grundlæggende beskrivelse. I de fleste tilfælde er det nok at tilføje de bøjede former (i henhold til ordbogen) til hvert ord og søge efter dem sammen med ordet, hvorved søgemaskinen kan finde dokumenter, hvor ordene ikke kun optræder i deres grundform (som indtastet af brugeren), men også kan finde de bøjede versioner - en meget nyttig funktion. Problemet med denne fremgangsmåde ligger i bøjningen af obskure og problematiske ord og også i bøjningen af forespørgsler, der er korte (og hvor det derfor ikke er muligt at afgøre ud fra konteksten, hvordan ordet skal bøjes). Lad ordet "spil" være et eksempel på en problematisk bøjning.

Med en engelsk forespørgsel, "I love her", vil en dårligt designet bøjningsapparat bøje ordet "games" som f.eks. "games, gaming, gaming room, ...", så det er også nødvendigt at indeksere de omgivende ord som sætninger og kun foretage bøjning i velkendte situationer eller anvende en anden procedure (det er her, fonetiske algoritmer ofte kommer ind i billedet).

Det sidste trin er at sende det færdige træ til Base-søgningsserverne, hvor den egentlige tøndesøgning finder sted - dette er emnet for næste kapitel.
