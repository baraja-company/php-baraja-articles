Algoritme for søgemaskiner på internettet - sortering og beskrivelser
=====================================================================

> id: ca4aaac5-0cd8-41db-b860-c32661c43d82
> slug:
> 	cs: vyhledavac-trideni-a-popisovac
> 	da: algoritme-for-sogemaskiner-pa-internettet-sortering-og-beskrivelser
> 
> publicationDate: '2016-09-11 13:00:00'
> mainCategoryId: '367f936c-073f-44bd-b399-30738e93137a'
> sourceContentHash: '541310dfdc96b75a320c7cad1b03e734'

I denne lektion om principperne for en søgemaskine på internettet vil vi forstå, hvordan en søgemaskine sorterer, beskriver og evaluerer resultaterne.

Sortering af resultater
----------------

Lad os forestille os en færdig tønde, der er klar på søgeserveren. Vores første søgeforespørgsel kommer ind fra brugeren, og nu skal vi lave den første "grove" sortering, som vil blive yderligere forfinet.

Lad os tage følgende eksempel på en indtastningsforespørgsel:

```txt
[O (and) pejskovi (and) a (and) kočičce]
```

Ja, det er den form, hvor søgeserveren modtager den behandlede forespørgsel fra brugeren og nu venter på at få resultatet tilbage. I alt har vi mindre end `300 ms` til at gøre dette, så hurtigt :)

I det første trin får vi tønder fulde af fremtidige resultat-id'er (også kaldet kandidater) og operationer, der skal udføres. Denne hentede tønde er i nogle tilfælde ikke komplet, men indeholder kun de værdier, som serveren nåede at læse fra disken i et forudbestemt tidsinterval (også kaldet **timeout**). Vi får f.eks. følgende talserier (som er delvist sorteret på forhånd):

| Tønder | Dokumenter |
|-------|---------|
| o | 2,3,4,5,9,12,15,19,23,26,28,34,39,42,58,67,83,91,115 |
| hund | 5,19,23,42,46,58 |
| a | 2,3,4,5,9,12,15,19,23,26,28,34,39,42,58,67,83,91,115 |
| fisse | 9,19,42,57,58,62,62,68,83 |

Vi udfører nu en grov skæring af alle sættene og får en liste over dokument-ID'er, der er de samme i alle tønder. I praksis gennemgår vi den korteste liste og søger i de andre.

De fælles ID'er i alle tønder er "19, 42, 58", så den fremtidige resultatside indeholder 3 elementer i en endnu ukendt rækkefølge. Vi ser stadig på dokumenterne som ID'er, fordi det sparer en enorm mængde data. I søgeresultater ønsker vi dog generelt ikke at angive dokumentnumre for brugeren, men snarere dokumentets titel (overskrift), beskrivelse, URL-adresse og andre oplysninger. Dette leveres af "descriptor"-komponenten.

Deskriptor
---------

Fra det foregående kapitel står vi tilbage med en liste over kandidater til fremtidige resultater. Vi hentede den i sorteret form i forhold til systemet, dvs. som søgemaskinen sekventielt har rangeret dem i indekset. På dette tidspunkt kan vi ikke længere udføre en sekventiel læsning af big data, men skal sekventielt gennem en eller anden form for relationel database for at finde yderligere oplysninger til en anden form for sortering, der er mere forståelig for brugeren.

Et databasesnit kan f.eks. se således ud:


| ID | Titel | Etiket | URL | Rank | Rank |
|----|---------|---------|-----|------|
| 19 | Josef Čapek: En fortælling om en hund og en kat | Det var dengang, hunden og katten stadig var landmænd sammen; de havde deres lille hus ved skoven og boede der sammen og ville gøre alting på samme måde som store mennesker gør det | www.troglodytarium.cz/webz/axf/.../Capek_J_Pejsek_a_kocicka.htm | 72 |
| 42 | Snak om hunden og katten | "Okay," sagde hunden, og katten tog sæbe og en gryde med vand, knælede ned på gulvet, tog hunden som børste og skurede hele gulvet med hunden. Gulvet var helt vådt | www.capek.narod.ru/povidani.htm | 86 |
| 58 | Hvordan hunden og katten lavede en kage til ferien| I morgen var det hundens ferie og kattens fødselsdag. Børnene vidste det og ville overraske hunden og katten til deres fødselsdag. De tænkte på, hvad de kunne gøre for hunden og | a.da.mek.sweb.cz/capek.j/dort.htm | 34 |

Beregning af relevans
-----------------

Det sidste trin er at beregne relevansen af svaret. For de fleste resultater er det tilstrækkeligt at repræsentere relevansen som et enkelt tal med en fast radius, som resultaterne sorteres efter. Resultaterne sorteres igen "groft" efter rang i flere grupper (antallet af grupper afhænger af rangernes varians), og der arbejdes videre med disse grupper.

Grupperne med den højeste rangering bliver taget først, og ofte er det nok til at sortere den første side af resultaterne, og vi behøver ikke at tage os af andet. Men hvis flere forskellige dokumenter har samme rang, skal vi foretage en mere detaljeret analyse og beregne yderligere hjælpe-rangeringer for at hjælpe med at rangere siden. Den yderligere rangering kan f.eks. være kvaliteten af links, dokumentets alder, længden af titlen, URL'ens "look and feel" og mange andre faktorer.

Returnering af resultater til brugeren
---------------------------

Hurra! Vi har resultaterne, og nu mangler vi kun at gengive siden for brugeren. Resultaterne sendes tilbage til serveren, som indpasser resultaterne i en forberedt skabelon, indsætter reklamebannere rundt om på siden og sender siden tilbage til brugeren. Samtidig kan den også udføre andre hjælpeoperationer, f.eks. caching af resultaterne. Hvis en anden person søger på den samme forespørgsel på et tidspunkt i den nærmeste fremtid (f.eks. inden for den sidste time), vil resultaterne ikke blive søgt igen, men kun læst fra den midlertidige hukommelse - hvilket ofte er med til at forbedre søgemaskinens generelle ydeevne.

Konklusion og indsigt i semantik
---------------------------

Formålet med denne artikel var at beskrive de grundlæggende principper for, hvordan søgemaskiner fungerer internt, og hvordan de formår at finde et teoretisk set ubegrænset antal dokumenter på en stadig rimelig tid. Der er tale om enorme matematiske optimeringer, som er blevet udviklet i mange år af de bedste programmeringshold.

En fuldstændig beskrivelse af detaljerne ligger uden for rammerne af denne artikel. Det hele kompliceres også af, at de fleste af de andre trin ikke er beskrevet i detaljer af søgemaskinerne, fordi de er deres forretningshemmeligheder.

Det er også vigtigt at bemærke, at søgemaskinerne stadig ikke forstår sidernes indhold og betydningen af resultaterne, og at det stadig kun er en statistisk beregning baseret på rå computerkraft og folks evne til at linke til kvalitetstekster, og dette system er også ret sårbart (tag eksemplet med "Google-bomben", hvor en snu webmaster tvinger indhold på en søgning, der er irrelevant).

Dette problem skulle til dels kunne løses med kunstig intelligens, som alle søgemaskiner efterhånden forsøger at integrere. Dette er dog stadig en fjern fremtidsmusik, da det ikke er en let opgave og for det meste blot et spørgsmål om at forbedre de statistiske beregningsmetoder. Lad Google grafsøgning være et eksempel, som forsøger at besvare forespørgslen direkte (selv om den i sin interne struktur stadig kun søger i en foruddefineret vidensbase).

Så næste gang du stiller en forespørgsel til din yndlingssøgemaskine, skal du huske, hvor meget arbejde søgemaskinen har måttet udføre, og hvor mange TB data den har måttet læse fra disken for at finde de 10 bedste dokumenter til din forespørgsel.
