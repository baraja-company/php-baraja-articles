Hur väljer man ut teknik? När övergår vi till JavaScript?
=========================================================

> id: d3ea7ea7-3e2f-455d-a424-cce374bcd1d2
> slug:
> 	cs: senior-9-vyber-technologii
> 	sv: hur-vaeljer-man-ut-teknik-naer-oevergar-vi-till-javascript
> 
> publicationDate: '2023-02-11 14:40:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: '72807451867478e1a7b6d67f4e5c31b3'

Att välja rätt teknik är en förutsättning för att bli en ledande utvecklare. Dessa beslut är ofta inte lätta, eftersom du måste ta hänsyn till applikationens nuvarande tekniska status, vart du är på väg utvecklingsmässigt, vilka kunskaper ditt nuvarande team har, vilka kunskaper som är vanliga på arbetsmarknaden, vad varje teknik kostar, vilka risker den medför för din verksamhet, hur säker och stabil tekniken är, och sist men inte minst, vad kommer utvecklarna att vara intresserade av, låt oss säga, om fem år när 80 % av ditt nuvarande team har bytts ut.

Jag har gått igenom sex stora företag som utvecklar i PHP. Endast två av dem försöker byta till en annan teknik på lång sikt, de andra stannar kvar. Det har många problem. Jag försöker till exempel hitta en senior PHP-utvecklare till ett företagsprojekt som jag utvecklar för O2, med krav på att pendla till kontoret i Prag, och jag kan se hur marknaden för PHP-utvecklare har rensats under de senaste fem åren. PHP är helt enkelt inte coolt längre och det är inte många som vill göra det. Det finns inte tillräckligt många juniorer i den.

När jag har intervjuat yngre människor har jag uppfattat att React och allmänt "tunn" teknik är mycket populärt nuförtiden. Ur ett applikationsarkitekturperspektiv är det vettigt om du upptäcker denna riktning tidigt och har tid att anpassa dig. Istället för den komplexa skapandet av webblayouter och formulär i Latte, för vilket du behöver en praktiskt taget medioker utvecklare för ett redan något mer komplext uppdrag, behöver du i React bara en junior som började för i princip en månad sedan, och som fortfarande inte gör för många misstag i den framtida lösningen.

Med React kan du kasta bort en stor del av den backend som skrevs bara för att fronten skulle kunna existera överhuvudtaget. Kort sagt, det gör utvecklingen billigare, och som en bonus får du snabbare nya funktioner eftersom utvecklarna inte behöver ta itu med de komplexa problem som uppstår i PHP-utvecklingsspråket om och om igen.

De flesta webbapplikationer behöver inte ens en backend längre, eller bara en minimal backend. När du exponerar API-slutpunkter i Node.js (som också är en teknik som bygger på JavaScript) kan en utvecklare som tidigare bara arbetade med React plötsligt skriva delar av backend också, eftersom det är samma språk.

Vid en djupare analys av de projekt som jag har utvecklat under de senaste fem åren finns det bara några få saker som saknas i Node.js som gör att jag fortfarande använder PHP för vissa operationer.

Nämligen:

- Doktrin (och i allmänhet tillgång till relationsdatabaser som bygger på objektsenheter).
- Skicka och hantera e-postmeddelanden
- SOAP API (tyvärr finns det fortfarande ibland)
- Sessioner (du måste till exempel ersätta den med en JWT-token).
- Komplicerad äldre logik som skrevs i PHP och som jag inte enkelt kan omarbeta.
- Snabb behandling av komplexa datastrukturer där data måste ändras.
- Befintliga personer i teamet som du måste lära dig att göra något nytt.

Men sedan kom Node.js, som klarar resten bättre. Till exempel:

- Möjligheten att avlasta appen direkt till molnet
- Mycket (kanske till och med dubbelt) billigare utveckling av samma funktionalitet.
- Samma logik på BE och FE utan att behöva skriva kod två gånger.
- REST API-slutpunkter
- Parallella anrop till flera koder samtidigt
- Möjlighet att skicka ett HTTP-svar, men koden fortsätter att köras
- Kronojägare
- Biblioteken ska arbeta med molntjänster
- Betydligt bättre svarstid eftersom du inte behöver starta upp ett stort program.
- Fullt fungerande paradigm (du blir av med DIs som du inte behöver i JS, till exempel).
- Arbeta med formulär och data
- Lätta uppdateringar och en aktiv utvecklargemenskap

**Kommentar om Doctrine:** Jag vet att JS har många bibliotek för arbete med databaser. Eller till och med nya paradigmer som Mongo. Jag gillar den riktning som databehandlingen tar. Å andra sidan tror jag att tabulära relationsdatabaser aldrig kommer att försvinna. När du genomför ett riktigt stort projekt där du hanterar uppåt tiotals miljoner poster behöver du helt enkelt traditionell teknik som du är mycket bekant med och vet vad du kan förvänta dig. Tanken på att jag vill lägga till en kolumn (egenskap) och att det skulle innebära att jag måste göra om alla enheter med ett migrationsskript är till exempel ganska skrämmande för mig.
