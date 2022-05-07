Algoritmen för sökmotorer på Internet - knappt en crawler
=========================================================

> id: '5ad042bf-7fda-4b2e-a38c-762169d0b594'
> slug:
> 	cs: vyhledavac-barely-a-crawler
> 	sv: algoritmen-foer-soekmotorer-pa-internet---knappt-en-crawler
> 
> publicationDate: '2016-09-11 12:00:00'
> mainCategoryId: '367f936c-073f-44bd-b399-30738e93137a'
> sourceContentHash: '5daf3854688c39f9e0071a93ab11dd4e'

I dagens lektion kommer vi att diskutera datafatöljer, deras struktur, StopSlovas och slutligen kommer vi att beskriva crawlers.

Datatunnlar
-------------

Detta är en speciell datatyp som finns på flera servrar samtidigt i flera kopior. Dessa är vanligtvis dataintensiva filer på hundratals GB som är långsamma att läsa (vilket är anledningen till att de delas upp i delar) och praktiskt taget omöjliga att redigera. Om vi vill göra ens en minimal förändring måste vi räkna om hela tunnan. Sökmotorn Seznam kan till exempel räkna om datatunnor högst en gång varannan dag eller vecka, medan Google räknar om en gång varannan timme (och då bara vissa delar, aldrig allt på en gång).

Barriärerna innehåller ord och deras förekomst på Internet. Man kan säga att detta är rådata för sökresultat i en ännu inte sorterad form (ibland är de delvis sorterade efter relativ betydelse) och förberedda i förväg. Själva sökningen äger rum långt innan användaren ställer sökfrågan och det är här som alla resultat förbereds. Själva sökningen skulle vara omöjlig i realtid, så sökmotorn läser i princip de resultat som redan förberetts här (sökningen utförs av indexeringskomponenten, som kommer att diskuteras i ett separat kapitel).

Tunnlarna innehåller all den grundläggande information som behövs för att skapa sökresultat för varje ord som förekommer på Internet, så prestandaproblem måste lösas när de läses i följd. I praktiken tenderar tunnor att lagras i partier på flera servrar och även i RAM för snabbare åtkomst. Google-sökmotorn läser till exempel inte alls tunnorna från disken, utan behåller dem helt och hållet i RAM och behåller bara en kopia av dem på disken om data skulle gå förlorade från RAM.

Stora sökmotorer med tillräckliga resurser har också så kallade "gyllene tunnor" som innehåller de viktigaste sidorna på Internet och som söks upp vid stor sökbelastning. Om till exempel flera sökservrar kraschar kan guldtunnorna tillfälligt tillgodose sökningarna. Sökmotorn kanske inte hittar alla resultat, men sökningen avbryts åtminstone inte helt utan antalet sökta dokument begränsas tillfälligt. Det är också nödvändigt att uppdatera tunnorna regelbundet, eftersom antalet sidor på Internet ständigt ökar. Eftersom tunnorna vanligtvis är i storleksordningen gigabyte är det inte möjligt att utföra inkrementellt underhåll, utan det är nödvändigt att bygga om allting en gång i taget. Därför har sökmotorn en extra barrel där alla data finns i form av en stor tabell från vilken barrelerna byggs - Google bygger t.ex. barreler ungefär var 12:e timme. Den stora utmaningen är just att tunnorna åtminstone delvis måste vara sorterade och återspegla det faktiska läget på Internet. Så tills sökmotorn uppdaterar tunnorna kommer användaren inte att hitta nya webbsidor.

Struktur av tunnor
----------------

I praktiken lagras en fatning som en stor textfil som innehåller ID för varje dokument där sökordet förekommer och positionen för det sökta ordets förekomst.

Exempel på en mycket liten tunna med tre sidor:

```txt
[1:5,8,12,27,30;5:1,3,6;12:4,8,10]
```

Detta exempel innehåller sidor med identifierare `1`, `5` och `12`. De tillhörande siffrorna anger ordets position i dokumentet. En sådan barrel fångar endast upp förekomster av ett enda ord, så varje ord på Internet har sin egen barrel. När du söker efter flera nyckelord är det nödvändigt att läsa flera tunnor (en annan för varje ord) och sedan utföra operationerna enligt det binära trädet (mer om detta i de föregående kapitlen).

Korsningen sker vanligtvis genom att man går igenom ett av dokumenten och söker i det andra. Om tunnorna är stora kanske de inte kan läsas helt och hållet, vilket innebär att sökmotorerna ofta bara lyckas läsa en del av dem och sedan måste returnera resultatet. Det är därför vettigt att sortera tunnorna åtminstone delvis, så att de viktigaste sidorna (som användaren troligen letar efter) finns i början av tunnan och allt annat i slutet av tunnan.

StopLead
---------

Du kanske tänker att för vissa ord kan tunnorna vara mycket stora och därför svåra att arbeta med. Ordet `a` eller ordet `1` finns till exempel i mer än hälften av dokumenten, men de har ingen betydelse för sökaren (eftersom de sällan söks och kräver fler omgivande ord). Sådana ord kallas stoppord.

Problemet med stoppord är att de inte alla kan sökas efter och att sökmotorerna därför bara visar en förvrängd bild av hur Internet faktiskt ser ut för sökordet.

Ett exempel på en sökning med StopSlovy är sökfrågan `Prague 1`.

Sökmotorn ger endast `3 020 000 resultat` för denna fråga. I verkligheten finns det dock många fler sådana webbplatser. Det är dock inte alla som söks av prestandaskäl.

Sök efter åtkomstalternativ med StopSlovy:

- inte indexera dem alls och inte göra fat för dem (små sökmotorer gör detta)
- Indexera dem, men endast lagra relevanta sidor i tunnor (detta görs till exempel av Google och Seznam).
- Indexera dem som vanliga ord och skapa kompletta tunnor (problemet med denna lösning är att tunnorna lätt kan överstiga storleken på en vanlig hårddisk och därför inte kan lagras och kan ta sekunder att läsa - denna lösning används därför inte i praktiken eller endast för små och specialiserade sökmotorer).

I allmänhet finns det ingen universellt korrekt metod och inte ens Google använder den perfekt. Detta är ett så stort problem att det kan ta hela livet att lösa. För den här uppsatsen räcker dock denna allmänna beskrivning.

Crawler (robot, även spindel)
---------------------------

En crawler är ett program som systematiskt går igenom Internet och håller reda på vilka ord som förekommer på vilka sidor och samlar även in ytterligare information (även kallad metainformation). En crawler körs vanligen på många servrar, eftersom det är krävande med nätverksbandbredd att crawla på Internet och därför måste många sidor crawlas samtidigt. Google uppskattar att upp till 30 000 sidor kryssas samtidigt vid varje tidpunkt.

Innan Crawler öppnar en sida tittar han på en databas med adresser som han planerar att besöka i framtiden. Om den redan har besökt adressen kommer den bara att uppdatera sina register (den besöker stora webbplatser med några timmars mellanrum, nyhetssajter med några minuters mellanrum och vanliga webbplatser högst en gång i månaden).

Om roboten kommer till en ny adress som den inte känner till, tittar den på de länkar till andra dokument (sidor) som finns där. För varje länk beräknas hur viktig den är, och om det är en viktig länk kommer den också till sidan senare.

List till exempel går bara igenom några få nivåer av länkar på varje webbplats på detta sätt, medan Google försöker gå igenom hela webbplatsen, även oviktiga dokument, vilket gör den till den mest omfattande sökmotorn på Internet.
Roboten försöker också rangordna sidan när den kryper och beräknar dess "rank", som är en numerisk representation av hur viktig sidan är på Internet. Tidigare har Google använt PageRank, som har ett intervall från 0 till 10. Google beräknar endast PageRank 10 för sig själv och för webbplatser som microsoft.com eller w3c.org. Numera beräknas värdet på ett annat sätt (artificiell intelligens och vektormatematik).

Den högsta PageRank i Tjeckien är Seznam.cz med ett värde på 7. Rankingen har stor betydelse för hur ofta en robot besöker sidan och hur högt den hamnar i sökresultaten. Rankningen beräknas enbart utifrån de backlinks som pekar på den önskade sidan.

Crawlern gör inget annat med uppgifterna, utan laddar bara ner dem från Internet och lagrar dem i en databas där de väntar på indexeringsprocessen.
