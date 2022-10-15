Asynkron världsbild
===================

> id: '7ed25269-659f-4d17-a642-d07480cbfafa'
> slug:
> 	- null
> 	sv: asynkron-vaerldsbild
> 
> cs: asynchronni-pohled-na-svet
> publicationDate: '2022-10-15 11:30:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '24d4ec106e7febec5ee84cdf86bd8f28'

Jag har länge märkt att vår värld är asynkron och decentraliserad. När du inser detta och börjar fundera på hur du kan använda det till din fördel kan du lätt få ett helt nytt koncept för hur du ska se på hur du löser komplexa problem. I det här inlägget vill jag förklara några av de idéer som jag redan använder. Jag får dem inte från någon särskild källa, utan det är mer en kombination av olika erfarenheter och mina egna idéer. Dessa principer fungerar inte i alla fall.

Fastställande av miljö och mål
-------------------------

Nästan alla projekt som jag någonsin har tagit itu med (antingen ensam eller i grupp) har haft ett ganska specifikt definierat mål. Detta tillvägagångssätt är mycket logiskt eftersom det är viktigt att veta vad vi vill ha. Å andra sidan tror jag att det är omöjligt att definiera ett specifikt mål i en komplex miljö, och ofta upptäcker vi under genomförandet att vi egentligen vill komma någon annanstans.

I stället för ett specifikt mål bygger jag upp ett område med olika mål där det finns alternativ, eller så kan till och med ett helt nytt isolerat oberoende mål vara det rätta målet om det är vettigt.

Exempel från praktiken:

- Jag måste gradvis expandera till andra marknader. Ett av de delmål som jag upptäckte under genomförandet kan vara maskinöversättning av webben.
- Jag måste hämta 6 försändelser på olika platser i Prag och jag vet inte vilken väg som är kortast. Jag vill inte ta reda på det på ett komplicerat sätt, så jag går bara mot den mest avlägsna punkten och ändrar min plan mot andra vägar längs vägen. När jag till exempel byter i Anděl bestämmer jag mig för att ta den spårvagn som anländer först, vilket dynamiskt påverkar nästa ruttplan.
- Jag måste skriva det här inlägget, men jag har inte tid en timme i sträck. Därför kan jag skriva den i delar när jag åker tunnelbana som enskilda kapitel. Jag kommer sedan att göra den slutliga sammanslagningen i denna form i framtiden.
- Jag vet inte hur algoritmen fungerar för att beräkna inlösen av varor för min kund, som jag måste programmera om. Därför ställer jag frågan till flera personer samtidigt och löser något annat medan jag gör det. Asynkront under två dagar kommer flera olika svar att komma in, som jag först senare kommer att fatta ett beslut baserat på.
- Jag måste bli av med PHP på servern under lång tid, så jag skriver gradvis om en sida i taget till React. Jag flyttar gradvis webbplatserna till molnet och bygger dem på statiskt genererade Nect.

Ingen av destinationerna är slutdestinationer. Om du arbetar med en yta och inte med en punkt är det mycket troligare att du träffar den. Samtidigt fungerar motivation bra här, eftersom det värsta som kan hända är att nå målet och sedan inte ha något att se fram emot i framtiden.

Mycket av det är också viktigt att säga att för att få ett liknande tankesätt måste man ha en ganska stabil miljö där man kan göra misstag. Detta tillvägagångssätt kan inte garantera en specifik tidsfrist för att lösa en specifik uppgift. Å andra sidan kan den optimera lösningen av så många parallella uppgifter som möjligt på så kort tid som möjligt, men inte nödvändigtvis i den ordning som de kom in i kön.

Principen kan jämföras med hur beräkningar fungerar i parallellprogrammering, till exempel. Kort sagt, du delar upp uppgifterna i enskilda trådar som löser olika deluppgifter samtidigt, och när du har fått alla svaren kan du komponera lösningen.

Distribuera nya programvaruversioner
--------------------------------

Jag kämpar ofta med detta överallt.

Det vanligaste sättet att utveckla programvara är att man har en stabil version som körs i produktion för användarna, sedan har man en testmiljö där ny utveckling sker, och då och då tar man ut nyheter från testmiljön till produktion, vilket kallas en release. Denna process är relativt säker och långsam.

Eftersom jag gradvis har gått över till en mikro-frontend-arkitektur, där applikationens frontend (det som användaren ser) är helt separerad från backend (där beräkningarna och databehandlingen sker), kan releaseprocessen fungera annorlunda.

Jag har fortfarande en produktionsmiljö som alltid fungerar. Från den skapas en ny gren för varje uppgift för att ta itu med en specifik funktion. Eftersom jag använder Vercel (en mycket bra molnleverantör) som värd för frontend kan jag ha en egen URL för varje utvecklingsgren.

När en ny funktion har testats sammanfogas den omedelbart och distribueras till produktionen i en asynkron process. Därför kan det hända att det kanske sker 15 nya versioner varje dag.

Mycket av detta hänger samman med en idé som jag fick på LMC - "En funktion som du tillhandahåller en kund i morgon, även om den är ofullständig, är mycket mer värdefull för dem än en funktion som är perfekt felsökt men som inte kommer att vara tillgänglig förrän om ett år". Men detta kan bara fungera om du har ett sätt att sprida nyheterna snabbt - vanligtvis webbutveckling.

Det jag gillar mycket med Next.js-ramverket (som bygger på Recto) och Vercel är principen att du ansluter ditt GitHub-förråd direkt till Vercel, och med varje överföring sker ett automatiskt byggande, testning och sedan distribution till produktion när allt är okej. Utvecklaren behöver inte oroa sig för någonting och appen distribueras varje timme utan ansträngning. Det är mycket viktigt att formalisera rutinmässiga saker och sedan automatisera dem.

Publicering av innehåll
----------------

Jag har brutit ner högre dussintals ämnen och inlägg med mig i Apple Notes. För varje ämne får jag ständigt nya idéer att skriva ner och sortera dem efter taggar. När flera anteckningar genereras för ett ämne omvandlar jag dem till kapitel och sedan omvandlar jag gruppen av kapitel till artiklar och FB-inlägg.

Egenskaperna hos denna princip:

- Jag vet inte i förväg hur många artiklar jag kommer att publicera,
- Men å andra sidan vet jag att det kan vara mycket,
- Jag kan se till att varje inlägg är fullt av information, insikter och idéer som har mognat med tiden (det här inlägget tog ungefär tre månader att mogna),
- Det är hållbart och jag kommer att njuta av det eftersom jag inte behöver skriva en massa text på en gång och riskera att glömma något.

Och sedan publiceringsprocessen.

Jag publicerar mycket innehåll i tysthet på webben först, så att Google upptäcker det först och sidan indexeras. När det har en bra täckning av nyckelord och jag är nöjd med det i stort sett, går ämnet till exempel ut på sociala medier.

För många ämnen vet jag att de inte kommer att vara av intresse och att de snarare kommer att irritera dig. Men samtidigt vill jag ha dem på nätet eftersom någon kanske kommer att söka efter dem i framtiden. I så fall finns artikeln bara på webben. Ett exempel på dessa artiklar är de olika överliggande artiklarna, som länkar till hela ämnesområdet så att så många ämnen som möjligt täcks in på webbplatsen. Omslagsartiklar är ofta mer tekniska och tråkiga. Eller så är det maskinellt genererade kategorier där jag bara organiserar flera artiklar i ämnen och på så sätt täcker in så många nyckelord som möjligt, som någon kanske vill söka efter.

Kunskap, utbildning, testning
------------------------------

Jag gillar att leka med teknik där det inte är klart från början vad den kommer att vara bra för en dag.

Som maskinöversättning. Vid första anblicken verkar det inte vettigt att översätta dussintals artiklar till främmande språk för läsare som jag inte har någon kontakt med. Å andra sidan kan det vara ett av stegen för att fatta ett beslut i framtiden för vilket förutsättningarna måste vara uppfyllda.

Generellt sett vet ingen hur framtiden kommer att se ut. Därför tycker jag att det är mycket vettigt att täcka in så många möjligheter som möjligt som man vill förstå, åtminstone ytligt, och kanske ta itu med i framtiden. Det är bra att tänka på stegen inte bara som en löst uppgift, utan som en evig evolutionär process som inte har någon slutdestination.

Fungerar detta tillvägagångssätt för att leverera anpassade projekt?
--------------------------------------------------------

För det mesta inte.

Du måste ha en klient som är din partner och ni vill båda leverera den bästa möjliga lösningen, även om ni vet att ni inte får reda på vad det är förrän i slutet.

I vår verksamhet kallas det för agil utveckling, och dessa principer bygger på detta. Å andra sidan har jag konstaterat att agil utveckling som jag känner till den inte passar mig direkt, och jag gillar att hitta på krångliga alternativ.

När det gäller smidighet ser jag mycket av principen om engagemang när det gäller vem som löser vad inom en viss sprint. Jag tycker att det är vettigare att bygga upp miljön så att du har en enorm backlog av uppgifter som löses utifrån vad som behövs mest just nu eller vad jag har den mentala kapaciteten att göra. När jag till exempel är på väg tenderar jag att ta itu med mer utvecklande tankeuppgifter som jag kan göra utomhus utan dator. När jag är på kontoret igen tar jag itu med mer algoritm- och matematikintensiva uppgifter eftersom jag kan fokusera fullt ut och investera mycket mental energi.

Jag rekommenderar inte denna princip om du startar en helt ny e-butik eller om ditt företag håller på att gå i konkurs. Du behöver en stabil miljö som körs av sig själv, och du kommer bara att vara en juvel. Men samtidigt vet du att om du inte gör något kommer din stabila miljö att upphöra en dag.
