Hvordan vælges teknologier? Hvornår skifter vi til JavaScript?
==============================================================

> id: d3ea7ea7-3e2f-455d-a424-cce374bcd1d2
> slug:
> 	cs: senior-9-vyber-technologii
> 	da: hvordan-vaelges-teknologier-hvornar-skifter-vi-til-javascript
> 
> publicationDate: '2023-02-11 14:40:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: '72807451867478e1a7b6d67f4e5c31b3'

At vælge de rigtige teknologier er en forudsætning for at blive seniorudvikler. Disse beslutninger er ofte ikke nemme, fordi du skal tage hensyn til applikationens nuværende tekniske tilstand, hvor du er på vej hen udviklingsmæssigt, hvad dit nuværende team ved, hvilken viden der er almindelig på arbejdsmarkedet, hvad de enkelte teknologier koster, hvilke risici de vil medføre for din virksomhed, hvor sikker og stabil teknologien er, og sidst men ikke mindst, hvad udviklerne vil være interesseret i om f.eks. 5 år, når 80 % af dit nuværende team er udskiftet.

Jeg har været igennem 6 store virksomheder, der udvikler i PHP. Kun 2 af dem forsøger at skifte til en anden teknologi i det lange løb, de andre bliver her. Det har en masse problemer. Jeg forsøger f.eks. i øjeblikket at finde en senior PHP-udvikler til et virksomhedsprojekt, som jeg udvikler for O2, med krav om at pendle til kontoret i Prag, og jeg kan se, hvordan markedet for PHP-udviklere er blevet mere attraktivt i de sidste fem år. PHP er bare ikke cool længere, og der er ikke mange, der har lyst til at gøre det. Der er ikke nok juniorer i den.

Fra mine interviews med yngre mennesker kan jeg se, at React og generelt "tynde" teknologier er meget populære i dag. Set fra et applikationsarkitekturperspektiv giver det mening, hvis du opdager denne retning tidligt og har tid til at tilpasse dig. I stedet for den komplekse udmøntning af weblayouts og formularer i Latte, hvor du skal bruge en praktisk talt middelmådig udvikler til en i forvejen lidt mere kompleks opgave, skal du i React blot bruge en junior, der startede for en måned siden, og som stadig ikke laver for mange fejl i den fremtidige løsning.

React giver dig mulighed for at smide en stor del af backenden væk, som blev skrevet for at frontend'en overhovedet kunne eksistere. Kort sagt gør det udviklingen billigere, og som en bonus får du en hurtigere levering af nye funktioner, fordi udviklerne ikke behøver at skulle forholde sig til de komplekse problemer, der opstår i forbindelse med PHP-designsproget, igen og igen.

De fleste webapplikationer har ikke engang længere brug for en backend eller kun en minimal backend. Når du udsætter API-endpoints i Node.js (også en teknologi, der er bygget på javascript), kan en udvikler, der tidligere kun arbejdede med React, pludselig også skrive dele af backend'en, fordi det er det samme sprog.

I en dybere analyse af de projekter, jeg har udviklet i løbet af de sidste 5 år, er der kun få ting, der mangler i Node.js, som stadig får mig til at bruge PHP til nogle operationer.

Nemlig:

- Doctrine (og generelt adgang til relationelle databaser baseret på objekt-enheder)
- Afsendelse og forvaltning af e-mails
- SOAP API (desværre findes det stadig nogle gange)
- Sessioner (du skal f.eks. erstatte det med et JWT-token)
- Kompleks legacy-logik, der er skrevet i PHP, og som jeg ikke nemt kan ombygge den
- Hurtig behandling af komplekse datastrukturer, hvor data skal muteres
- Eksisterende personer på holdet, som du skal omskole til at gøre noget nyt

Men så kom Node.js, som klarer resten af tingene bedre. For eksempel:

- Muligheden for at overføre appen direkte til skyen
- Meget (måske endda dobbelt) billigere udvikling af den samme funktionalitet
- Samme logik på BE og FE uden at skulle skrive kode to gange
- REST API endpoints
- Parallelle kald til flere koder på én gang
- Mulighed for at sende et HTTP-svar, men koden fortsætter med at køre
- Crony
- Biblioteker skal arbejde med cloud-tjenester
- Betydeligt bedre responstid, fordi du ikke behøver at starte et stort program op
- Fuldt funktionelt paradigme (du slipper for DI'er, du ikke har brug for i JS, for eksempel)
- Arbejde med formularer og data
- Nemme opdateringer og et aktivt udviklerfællesskab

**Kommentar til Doctrine:** Jeg ved, at JS indeholder en masse biblioteker til at arbejde med databaser. Eller endda nye paradigmer som Mongo. Jeg kan godt lide den retning, som databehandlingen bevæger sig i. På den anden side tror jeg, at tabulære relationelle databaser aldrig vil forsvinde. Når du er i gang med et virkelig stort projekt, hvor du skal administrere op mod flere millioner poster, har du simpelthen brug for traditionel teknologi, som du er meget fortrolig med og ved, hvad du kan forvente. For eksempel er tanken om at tilføje en kolonne (egenskab), og at det ville betyde en ny konvertering af alle enhederne med et migrationsscript ret skræmmende.
