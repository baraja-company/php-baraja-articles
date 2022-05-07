Återgå från UUID till heltal
============================

> id: d842517b-c4f6-4cef-a839-d08ff3804fca
> slug:
> 	cs: navrat-z-uuid-na-integer
> 	sv: aterga-fran-uuid-till-heltal
> 
> publicationDate: '2021-05-02 17:00:00'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: b7c136f3e3ca8a4563230c557ab1fa9c

Vid programvaruutveckling kommer en programmerare ofta att hamna i en återvändsgränd när han eller hon ställs inför ett arkitektoniskt beslut som kommer att ha en enorm inverkan på framtiden för hans eller hennes arbete under flera decennier framöver. Samtidigt är det ett beslut som inte kan ändras, och varje misstag har ett högt pris att betala. Databaser är ett typiskt exempel på ett arkitektoniskt beslut där huvuden rullar vid varje litet misstag.

Ett av de stora besluten på senare tid har varit hur man ska lagra primära nycklar i databastabeller. Även om detta verkar vara ett trivialt problem finns det mycket mer bakom det än du kanske tror.

Alternativ för primärnycklar
-------------------------

Det finns i princip fyra grundläggande alternativ:

- heltal
- heltal utan förtecken
- big int
- <a href="/uuid-performance">UUUID</a>

Integer är helt enkelt ett heltal (om det är `unsigned` så är det unsigned, alltså alltid positivt, och om det är `big int` så kan det nå extremt stora värden). Ett mycket enkelt koncept. Ett UUID är en textsträng (till exempel av formen `c4a760a8-dbcf-5254-a0d9-6a4474bd1b62`) som består av flera delar, som var och en kan ha vissa egenskaper och som är användbara för att bygga enorma multiserver- eller decentraliserade tillämpningar.Det finns ett stort ekosystem av användbar teknik kring UUID:er som löser problem som du inte ens vet om att du har, eller som du kommer att ha i framtiden.

Använd rätt hammare
-------------------------

För inte så länge sedan (vintern 2020) förklarade min vän Paul begreppet att tillämpa den lämpliga lösningen på ett problem av en viss storlek. Detta är en bra och viktig idé som många utvecklare glömmer bort - det skapar enormt komplexa lösningar när de inte behöver det. På engelska har vi ett fint uttryck för detta **over-engineering**.

UUID-storlek och unikhet
--------------------------

Den grundläggande fördelen med UUID är att om programmet blir för stort och vi delar upp databasen på många webbservrar, där en databastabell är så stor att den inte får plats på en enda maskins disk, kan vi dela upp den på många fysiska maskiner som var och en känner till sin egen del av tabellen och frågar sina kollegor om resten. UUID löser också det grundläggande problemet med att infoga nya rader, när vi i extremt stora tillämpningar måste skriva rader parallellt på många ställen och inte vill vänta på att huvudserverns skrivkapacitet ska bli ledig.

Konceptet att skriva på många ställen samtidigt används till exempel i chattprogram. När du skickar ett meddelande via Messenger skickas det till närmaste Facebook-databasserver som tilldelar meddelandet ett UUID och en tidsstämpel och skriver det till sin lokala databas. Din vän på andra sidan jordklotet skriver i sin tur meddelandena till sitt lokala datacenter, och under tiden säkerställer hela molninfrastrukturen synkroniseringen över hela världen. Låter häftigt, eller hur? :)

För att en sådan parallellskrivning ska fungera måste problemet med kollisioner mellan poster lösas. Om de enskilda lokala databaserna använder ett enkelt heltal kommer två oberoende servrar mycket snart att skriva två olika poster under samma identifierare. När dessa poster är synkroniserade uppstår en kollision. Det finns oftast ingen lösning - du kan inte numrera om ID:erna, eftersom andra sessioner kan leda till detta.

UUID löser detta genom att till exempel ge varje server ett överenskommet prefix som den infogar i början av varje UUID, sedan infogar den en tidsstämpel och därefter själva identifieraren.

> **Interessant faktum:** När vi skriver en så stor mängd data är vi inte så mycket intresserade av när en post skrevs, utan i vilken ordning den skrevs (så att vi inte byter ordning på meddelanden för användaren, till exempel).

Du kanske undrar hur man hanterar en situation där servrarna inte kan komma överens med varandra om vilket prefix de ska använda. Detta problem uppstår till exempel i decentraliserade eller offline-tillämpningar. I det här fallet kan UUID även genereras slumpmässigt.

Frågan är då hur stor är risken för att en konflikt uppstår när <a href="https://stackoverflow.com/questions/1155008/how-unique-is-uuid">tillfälligt genererar ett UUID</a>. Det kommer förmodligen inte att hända dig. Det finns ungefär `2^122` unika UUID:er (eftersom det är ett `128-bitars nummer`). I praktiken är risken för en konflikt ungefär 0,000000000000006 (6 × 10-11). I praktiken innebär detta att om vi genererar **1 miljard UUID:er** varje sekund under de kommande 100 åren kommer risken för konflikter att vara 50 %. Det är alltså mer sannolikt att konflikter inte uppstår, och UUID är den definitiva lösningen på dina databasproblem.

Finns det ett behov av en sådan robust lösning?
-------------------------------

Om du inte vet det är svaret **nej**.

Om primärnyckeln är inställd på `int` med flaggan `unsigned` finns det `4 294 967 295` möjliga värden (4 miljarder). En jämförelse av heltalsstorlekar finns i <a href="https://dev.mysql.com/doc/refman/8.0/en/integer-types.html">MySql-dokumentationen</a>.

När du lagrar 4 miljarder poster i en tabell kommer du förmodligen att få slut på diskutrymme tidigare.

Heltal och sammanfogning
----------------------

Heltal är verkligen mycket snabba. Det finns optimeringar för dem i MySql. Index fungerar korrekt (och de är dessutom mycket mindre), de tar bara upp 4 bytes, de är mycket snabba att ansluta och de kommer att fungera bra i de flesta fall.

Om du har problem med replikering av databaser kan den bästa lösningen vara att lägga hela databasen i molnet, t.ex. MS Azure, och fråga den externt. Även vid lagring av tiotals miljoner poster är hastigheten för åtkomst till en specifik rad via integer i storleksordningen millisekunder (mindre än 3 ms på en välkonfigurerad server), och med ett klusterindex kan tiden vara väl hållbar även vid ett stort antal förfrågningar.

Om du verkligen behöver använda UUID:er är det bäst att lämna MySql-världen och välja Postgres-databasen, som till skillnad från MySql har en egen datatyp för <a href="https://www.postgresql.org/docs/9.1/datatype-uuid.html">UUUUID:er</a>. Att arbeta med sammanfogningar är ett stort problem med UUID:er och MySql, och när man sammanfogar bara tre tabeller (där var och en bara har några tiotusentals poster) kan hela frågan ta flera hundra millisekunder till några sekunder att bearbeta. Detta är tyvärr ett MySql-problem som du förmodligen inte kan lösa.
