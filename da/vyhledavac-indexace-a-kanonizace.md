Algoritme for søgemaskiner på internettet - indeksering og kanonisering
=======================================================================

> id: daa97e66-e77f-4741-ade2-905c1cfdbd56
> slug:
> 	cs: vyhledavac-indexace-a-kanonizace
> 	da: algoritme-for-sogemaskiner-pa-internettet-indeksering-og-kanonisering
> 
> publicationDate: '2016-09-11 11:00:00'
> mainCategoryId: '367f936c-073f-44bd-b399-30738e93137a'
> sourceContentHash: d73a30f413e4b8b77dbceda60b3cf173

I dagens lektion vil vi se på indeksering og kanonisering af dokumenter på internettet.

Indeksering
--------

Indekseringsprocessen udføres af en komponent, der kaldes indekseringsenheden. Dette er et specielt designet program, der gør de downloadede data (de data, som crawleren har downloadet) til en særlig datatype til søgning - tønder.

Problemet med indeksering er, at man ikke kan gennemse dokumenterne "intelligent", men sekventiel læsning (læsning af hele teksten fra start til slut) er uundgåelig, så det er en krævende disciplin, og søgemaskinerne bruger de mest kraftfulde servere til denne aktivitet. Ingen anden opgave i søgeprocessen er så krævende som indeksering, hvor almindelig tekst bliver til indekser.

Tag f.eks. en side om katte, som er hentet fra Wikipedia. Indekseringsenheden får hele sidens tekst og skal fjerne unødvendige ting (f.eks. brugerstyringsmenuer, annoncer, sidefødder, ...) og analysere siden for at få den rene tekst. Teksten kunne f.eks. være:

> Huskatten (Felis silvestris f. catus) er en domesticeret form af den vilde kat, som har været menneskets følgesvend i tusindvis af år. Ligesom sin vilde slægtning tilhører den underfamilien af småkatte og er en typisk repræsentant for denne gruppe. Den har en smidig og muskuløs krop, der er perfekt tilpasset til jagt, skarpe kløer og tænder samt et fremragende syn, hørelse og lugtesans.

Uddrag fra [Wikipedia](http://cs.wikipedia.org/wiki/Ko%C4%8Dka_dom%C3%A1c%C3%AD).

Søgemaskinen analyserer nu de enkelte ord fra teksten og skriver dem ind i individuelle tønder. Den kan dog ikke skrive tønder direkte (primært fordi hver tønde findes i mange kopier, og fordi det ville forstyrre søgningen), så den opretter en liste over krav til, hvordan den fremtidige tønde skal se ud, og gemmer dem i midlertidig hukommelse. I vores tilfælde kan en sådan liste se sådan ud (nogle uvæsentlige ord kan ignoreres eller markeres som stopord):

| Dokument-ID | Word | Position | Position | Type |
|--------------|-------|--------|-----------|
| 1 | Cat | 1 | Normal |
| 1 | Indenlandsk| 2 | Normal |
| 1 | Felis | 3 | Normal |
| 1 | Is | 7 | StopWord |
| 1 | Domesticeret| 8 | Normal |

En sådan liste er enorm (meget større end de oprindelige tekster), men den er stadig hurtigere at søge, fordi vi ikke behøver at læse alle teksterne i rækkefølge, men kan søge efter indeks i hver kolonne, og ordene fra en side sorteres i rækker efter hinanden.

Når der er gået et stykke tid (eller et vist antal dokumenter er blevet udfyldt), stopper indekseringen med at opbygge denne liste over anmodninger (til en fremtidig tønde) og begynder at læse dataene igen og genopbygge de enkelte tønder (denne liste omfatter gamle poster, der er i en allerede fungerende tønde). Hvis der tilføjes nye poster fra kendte adresser, opdaterer denne proces dem, mens den medtager nye dokumenter for nye dokumenter.

På denne måde vil indekseringsenheden gennemgå listen igen og langsomt oprette nye tønder, der indeholder alle elementerne (de vil se ud som vist i eksemplet i de foregående kapitler), og når alle tønderne er færdige, vil de blive sendt til de enkelte søgeservere. Opdatering af tønderne på serverne er tidskrævende (hovedsagelig på grund af den store mængde data, der flyttes), så under denne operation er serverne ikke tilgængelige, og dataene opdateres på forskellige servere på forskellige tidspunkter. Dette resulterer f.eks. i, at nogle brugere kan få forskellige resultater, fordi hver bruger søger på en anden udleveringsserver (på grund af belastningsdekomposition). Når opdateringen er færdig, er alting tilbage til det normale, og alle brugere kan finde alle dokumenter på samme måde.

Indekseringsprocessen er vigtig for alle søgemaskiner, og den, der gør det oftest og mest omhyggeligt, er den, der har det mest opdaterede overblik over internettet. Google udfører denne operation med få timers mellemrum, Seznam en gang om ugen (og har en million gange færre data).

Kanonisering af dokumenter
--------------------

I det oprindelige design af fuldtekst-søgemaskinen var der ikke behov for noget som kanonisering, fordi internettet var et medie, der konstant skabte nyt indhold. Med tiden er der imidlertid opstået duplikering (dvs. at det samme indhold vises på flere forskellige URL'er), og søgemaskinerne må tilpasse sig dette. Et typisk eksempel er Wikipedia, som har mange artikler. Nogle forfattere af andre sider overtager disse tekster (delvist eller helt) og skaber dermed dobbeltarbejde. I de fleste tilfælde er det dog ligegyldigt, fordi kildesiden har en meget højere rang (linkkvalitet) end den plagierede side, men det kan nogle gange ske, at det forringer originalen på bekostning af piraten.

Søgemaskinerne har været nødt til at tilpasse sig til denne mangel, og begrebet "kanonisering" er blevet opfundet, hvilket kan forstås som "fjernelse" af visse sider fra indekset. Det gør i øvrigt indeksene mindre, og søgemaskinen behøver ikke at gennemsøge det samme indhold hele tiden unødvendigt.

Duplikater er internt opdelt i 2 store kategorier af hver søgemaskine:

Naturlige dubletter
-------------------

De opstår som følge af internettets naturlige adfærd og dets karakteristika.

For eksempel vil URL-adressen `http://mathematicator.cz` sandsynligvis have det samme indhold som URL-adressen `http://www.mathematicator.cz` eller `http://mathematicator.cz/index.php`, da dette er standardadfærd på Apache-serveren (og internettet generelt).

Hvis der findes en naturlig duplikering, opretter søgemaskinen et "canonical set", som er en gruppe af sider, hvorfra søgemaskinen vælger en repræsentant, der skiller sig ud i søgningen. Hvis et link fører til en side fra det kanoniske sæt, vil dens placering automatisk blive overført til hovedrepræsentanten.

Det er ofte en god idé at hjælpe søgemaskinen med at oprette dette sæt og sætte redirectet korrekt op på webstedet, hvilket vil få søgemaskinen til at se bedre på webstedet, og hovedrepræsentanten vil blive valgt bedre.

Duplikater, der fører til plagiat
----------------------------

Plagiering er et problem på internettet i dag. Plagiering i sig selv ville ikke have den store betydning, men brugerne er særligt generet af det, fordi de hele tiden finder de samme resultater for den samme forespørgsel. Har du også nogensinde fundet flere sider med identisk tekst til en forespørgsel? Det er præcis den adfærd, som søgemaskinerne forsøger at forhindre.

Det største problem er at finde ud af, hvilken side der er den oprindelige kilde - og at gøre det maskinelt. Igen sætter søgemaskinerne alle lignende sider sammen i et kanonisk sæt og vælger en hovedrepræsentant fra dette sæt. Hvis kilderne er fra forskellige domæner, kan situationen ikke betragtes som en naturlig duplikering (og enhver kandidat vælges), men alle sider skal vurderes kvalitativt og den bedste skal udvælges objektivt - og ideelt set den oprindelige kilde til indholdet.

Søgemaskinerne træffer ofte beslutninger baseret på hele domænets rang og styrken af linknetværket til et givent dokument, men selv dette er en ret upålidelig fremgangsmåde. Den anden faktor er normalt også tidspunktet for dokumentets oprettelse (indeksering). Hver søgemaskine holder derfor øje med, hvilke sider der ofte genererer nyt indhold, og besøger disse sider ofte, så den ideelt set opdager den nye side med det samme og derfor ikke vælger en anden side som proxy.

En detaljeret beskrivelse af metoderne til at foretage denne udvælgelse ligger uden for rammerne af denne artikel og kunne være genstand for en hel bog.
