Returnering fra UUID til heltal
===============================

> id: d842517b-c4f6-4cef-a839-d08ff3804fca
> slug:
> 	cs: navrat-z-uuid-na-integer
> 	da: returnering-fra-uuid-til-heltal
> 
> publicationDate: '2021-05-02 17:00:00'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: b7c136f3e3ca8a4563230c557ab1fa9c

I softwareudvikling vil en programmør ofte komme i en blindgyde, når han eller hun står over for en arkitektonisk beslutning, der vil få stor betydning for fremtiden for hans eller hendes arbejde i årtier fremover. Samtidig er det en beslutning, der ikke kan omgøres, og der er en høj pris at betale for enhver fejltagelse. Databaser er et typisk eksempel på en arkitektonisk beslutning, hvor der ruller hoveder ved hver eneste lille fejltagelse.

En af de store beslutninger i den seneste tid har været at finde ud af, hvordan man lagrer primære nøgler i databasetabeller. Selv om dette synes at være et trivielt problem, er der meget mere bagved, end du måske tror.

Muligheder for primærnøgle
-------------------------

Der er grundlæggende fire muligheder:

- heltal
- heltal uden fortegn
- big int
- <a href="/uuid-performance">UUUID</a>

Integer er simpelthen et heltal (i tilfælde af `unsigned` er det et ikke-fortegnet tal, så det er altid positivt, og i tilfælde af `big int` kan det nå ekstremt store værdier). Et meget enkelt koncept. Et UUID er en tekststreng (f.eks. af formen `c4a760a8-dbcf-5254-a0d9-6a4474bd1b62`), der består af flere dele, som hver især kan have visse egenskaber og er nyttige til opbygning af store multiserver- eller decentrale applikationer.Der findes et stort økosystem af nyttige teknologier omkring UUID'er, der løser problemer, som du måske ikke engang ved, at du har, eller vil få i fremtiden.

Brug den rigtige hammer
-------------------------

For ikke så længe siden (vinter 2020) forklarede min ven Paul begrebet om at anvende den rette løsning på et problem af en given størrelse. Dette er en god og vigtig idé, som mange udviklere gerne glemmer - det skaber enormt komplekse løsninger, når de ikke behøver det. På engelsk har vi et fint udtryk for denne **over-engineering**.

UUID-størrelse og entydighed
--------------------------

Den grundlæggende fordel ved UUID er, at hvis programmet bliver for stort, og vi opdeler databasen på mange webservere, hvor en databasetabel er så stor, at den ikke kan rummes på disken på én maskine, kan vi opdele den på mange fysiske maskiner, som hver især kender deres egen del af tabellen og spørger deres kolleger om resten. UUID løser også det grundlæggende problem med at indsætte nye rækker, når vi i ekstremt store programmer skal skrive rækker parallelt mange steder, og vi ikke ønsker at vente på, at hovedserverens skrivekapacitet bliver ledig.

Konceptet med at skrive mange steder på samme tid anvendes f.eks. i chatprogrammer. Når du sender en besked via Messenger, sendes den til den nærmeste Facebook-databaseserver, som tildeler et UUID og et tidsstempel til beskeden og skriver den til sin lokale database. Din ven på den anden side af jorden skriver til gengæld beskederne til sit lokale datacenter, og i mellemtiden sørger hele cloud-infrastrukturen for synkronisering på tværs af kloden. Lyder cool, ikke? :)

For at en sådan parallel skrivning kan fungere, skal problemet med kollision af poster løses. Hvis de enkelte lokale databaser anvender et simpelt heltal, vil to uafhængige servere meget hurtigt skrive to forskellige poster under den samme identifikator. Når disse poster synkroniseres, opstår der en kollision. Der er normalt ingen løsning - du kan ikke omnummerere ID'erne, fordi andre sessioner kan føre til det.

UUID løser dette problem ved f.eks. at give hver server et aftalt præfiks, som den indsætter i begyndelsen af hvert UUID, derefter indsætter den et tidsstempel og derefter selve identifikatoren.

> **Interessant kendsgerning:** Når vi skriver så store mængder data, er vi ikke så meget interesseret i, hvornår en post blev skrevet, men i hvilken rækkefølge den blev skrevet (så vi f.eks. ikke bytter om på rækkefølgen af meddelelser for brugeren).

Du spørger måske, hvordan du håndterer en situation, hvor serverne ikke kan blive enige med hinanden om, hvilket præfiks de skal bruge. Dette problem opstår f.eks. i decentraliserede eller offline applikationer. I dette tilfælde kan UUID'et endda genereres tilfældigt.

Spørgsmålet er så, hvor stor er chancen for, at der opstår en konflikt, når <a href="https://stackoverflow.com/questions/1155008/how-unique-is-uuid">tilfælde genereres tilfældigt et UUID</a>. Det vil sandsynligvis ikke ske for dig. Der er ca. `2^122` unikke UUID'er (fordi det er et `128-bit tal`). I praksis er chancen for en konflikt ca. 0,0000000000000000006 (6 × 10-11). I praksis betyder det, at hvis vi genererer **1 milliard UUID'er** hvert sekund i de næste 100 år, vil chancen for konflikt være `50%`. Så der er større sandsynlighed for, at der ikke opstår konflikter, og UUID er den endelige løsning på dine databaseproblemer.

Er der behov for en så robust løsning?
-------------------------------

Hvis du ikke ved det, er svaret **nej**.

Hvis primærnøglen er indstillet til `int` med `unsigned`-flaget, er der `4.294.967.295` mulige værdier (4 milliarder). Du kan finde en sammenligning af heltalsstørrelser i <a href="https://dev.mysql.com/doc/refman/8.0/en/integer-types.html">MySql-dokumentationen</a>.

Når du gemmer 4 milliarder poster i en tabel, løber du sandsynligvis hurtigere tør for diskplads.

Integer og join-ydelse
----------------------

Hele tal er virkelig meget hurtige. Der er indbyggede optimeringer for dem i MySql. Indekser fungerer korrekt (og de er desuden meget mindre), de tager kun 4 bytes, de er meget hurtige at sammenføje, og de vil være fine i de fleste tilfælde.

Hvis du har et problem med databasereplikering, er den bedste løsning måske at placere hele databasen i en sky som MS Azure og forespørge den eksternt. Selv når der lagres titusindvis af millioner af poster, er hastigheden for adgang via integer til en specifik række i størrelsesordenen millisekunder (under 3 ms på en velkonfigureret server), og med et clusteret indeks kan tiden være bæredygtig, selv med et stort antal forespørgsler.

Hvis du virkelig har brug for at bruge UUID'er, bør du forlade MySql-verdenen og vælge Postgres-databasen, som i modsætning til MySql har sin egen datatype for <a href="https://www.postgresql.org/docs/9.1/datatype-uuid.html">UUUUID'er</a>. Det er et stort problem med UUID'er og MySql at arbejde med joints, og når man samler blot 3 tabeller (med hver tabel med kun titusindvis af poster), kan det tage flere hundrede millisekunder til få sekunder at behandle hele forespørgslen. Og det er desværre et MySql-problem, som du sandsynligvis ikke kan løse.
