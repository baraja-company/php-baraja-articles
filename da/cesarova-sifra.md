Cæsars ciffer - hvordan det fungerer
====================================

> id: '662dd627-c106-406e-ad6a-7ae9a492ad92'
> slug:
> 	cs: cesarova-sifra
> 	da: caesars-ciffer-hvordan-det-fungerer
> 
> publicationDate: '2019-08-23 15:43:02'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '892ad0746be469b5cdbb4de72d8e85b9'

> **Varsel:** Denne artikel blev skrevet for mange år siden, og nogle oplysninger kan være forældede eller forkerte. Vær opmærksom på dette, når du læser.

Caesar cipher er en af de letteste hashing-funktioner. I sin tid var den praktisk talt ubrydelig, men i de moderne computeres tidsalder tager det kun nogle få ti sekunder, ja, endda nogle få minutter, at bryde den. Den er baseret på en nøgle, som beskeden krypteres efter, og som derefter kan udvides igen efter. Nøglen er derfor hemmelig. På krypteringstidspunktet kan meddelelsen ses og vil ikke betyde noget (blot et sammensurium af tegn). Den eneste måde at bryde krypteringen på er ved at gætte nøglen.

Den vigtigste
--------------------------

Nøglen kan være et vilkårligt heltal med færre cifre end selve meddelelsen. Normalt er der 3 gyldige cifre (så der er 999 kombinationer). Hvert ekstra ciffer øger sikkerheden. For at 2 parter kan kommunikere, skal begge parter kende denne hemmelige nøgle (så de skal på en eller anden måde videregive den sikkert til hinanden). Hvis nøglen er kendt af dem og ikke af den anden part, kan meddelelsen spredes selv på usikker vis og potentielt ikke kompromittere indholdet, fordi den potentielle angriber ikke kender proceduren for at få meddelelsen tilbage.

Til demonstrationsformål bruger jeg nøglen **123**, normalt bruges et andet tilfældigt tal, som ikke formodes at være så let at gætte.

Krypteringsprocedure
--------------------------

Princippet er baseret på idéen om at erstatte tegnene i en meddelelse med nogle andre ved hjælp af en nøgle. Jeg kalder det "karakterforskydning".

Lad os f.eks. have denne meddelelse, som vi ønsker at kryptere:

```php
TAJNA ZPRAVA
```

Giv nu hvert tegn et nummer. Typisk efter alfabet (dens serienummer). Jeg vil bruge det engelske alfabet, så denne serie af tegn:

```php
A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
```

Hvis vi tildeler hvert tegn et tal, får vi noget i stil med:

```php
20-1-10-14-1   26-16-18-1-22-1
```

Nu kommer nøglen. Vi tager hvert enkelt nummer og tilføjer nøglen til det. Jeg fremhæver i farver, hvad jeg har tilføjet hvor:

`1 2 3`

```php
21 3 13 15 3   3 17 20 4 23 3
```

Bemærk, at når jeg krypterer **Z**-tegnet, skriver jeg cifferet 3. Det skyldes, at **Z** er det sidste tegn i alfabetet, så når det når slutningen, tæller det igen fra begyndelsen af linjen.

Overførsel af meddelelsen
--------------------------

Nu kan vi sende meddelelsen på den måde, vi ønsker. Nogle gange endda offentligt. Andre vil kun se en ulogisk række af tal, så de vil sandsynligvis ikke engang vide, at der er tale om en slags ciffer. Sikkerheden øges også af selve nøglen, som er hemmelig. Nogle gange er det hensigtsmæssigt at sende meddelelsen som en række tal, andre gange er det hensigtsmæssigt at konvertere disse tal til tegn (igen efter den samme alfabetiske række) og derefter sende en sekvens af tegn. Meget afhænger af omstændighederne. Generelt er det dog bedre med en numerisk sekvens, da kun få mennesker vil have mistanke om, at der er tale om en kodet meddelelse.

Dekryptering hos modtageren
--------------------------

Modtageren dekrypterer ved hjælp af samme procedure. Den tager hvert enkelt tegn og trækker tallene fra i henhold til nøglen og konverterer derefter de resulterende værdier tilbage til tegn ved hjælp af alfabetet. Det er blot en omvendt krypteringsprocedure. Det vigtigste er at kende **nøglen** og **tegnsættet** - dvs. hvordan tegnene skal rækkefølges.

Knækning og dekryptering
--------------------------

Den eneste mulige metode til at knække koden er at prøve alle tænkelige kombinationer af alle mulige nøgler. Hvis vi ikke kender længden af nøglen, bliver hele processen endnu mere kompliceret. Men generelt kan computere i dag prøve omkring 100 nøgler på 1 sekund, så det tager omkring et minut at knække en tilfældig nøgle med 3 tegn at knække.

Men hvis nøglen er lige så lang eller længere end den oprindelige meddelelse, kan den ikke knækkes, fordi hvert enkelt tegn har sin egen nøgle, så alle kombinationer skal prøves for hvert tegn.

Hvis jeg havde en besked "**TAKE MESSAGE**", ville den være 11 tegn lang (mellemrum tæller ikke med). Hvis jeg ville have en nøgle, der også var 11 tegn lang, og jeg brugte en streng af 26 tegn i det engelske alfabet, så er der **11^26** = 1.191817654*10²⁷ kombinationer, og den gennemsnitlige computer ville knække denne nøgle på 1.310999419×10²⁶ sekunder = 10^20 dage :)
