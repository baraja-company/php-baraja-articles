Algoritm för sökmotorer på Internet - indexering och kanonisering
=================================================================

> id: daa97e66-e77f-4741-ade2-905c1cfdbd56
> slug:
> 	cs: vyhledavac-indexace-a-kanonizace
> 	sv: algoritm-foer-soekmotorer-pa-internet---indexering-och-kanonisering
> 
> publicationDate: '2016-09-11 11:00:00'
> mainCategoryId: '367f936c-073f-44bd-b399-30738e93137a'
> sourceContentHash: d73a30f413e4b8b77dbceda60b3cf173

I dagens lektion tittar vi på indexering och kanonisering av dokument på Internet.

Indexering
--------

Indexeringsprocessen utförs av en komponent som kallas indexerare. Detta är ett specialutformat program som gör de nedladdade uppgifterna (de uppgifter som Crawler har laddat ner) till en särskild datatyp för sökning - fat.

Problemet med indexering är att man inte kan bläddra i dokumenten på ett "smart" sätt, men sekventiell läsning (att läsa hela texten från början till slut) är oundviklig, så det är en krävande disciplin och sökmotorerna använder de mest kraftfulla servrarna för denna verksamhet. Ingen annan uppgift i sökprocessen är så krävande som indexering, där vanlig text blir till index.

Ta till exempel en sida om katter, hämtad från Wikipedia. Indexeraren får hela sidans text och måste ta bort onödiga saker (t.ex. menyer för användarkontroll, annonser, sidfötter, ...) och analysera sidan för att få fram den rena texten. Texten kan till exempel vara:

> Tamkatten (Felis silvestris f. catus) är en domesticerad form av den vilda katten som har varit människans följeslagare i tusentals år. Liksom sin vilda släkting tillhör den underfamiljen småkatter och är en typisk representant för gruppen. Den har en smidig och muskulös kropp som är perfekt anpassad för jakt, vassa klor och tänder samt utmärkt syn, hörsel och luktsinne.

Utdrag från [Wikipedia] (http://cs.wikipedia.org/wiki/Ko%C4%8Dka_dom%C3%A1c%C3%AD).

Sökmotorn analyserar nu enskilda ord ur texten och skriver dem i enskilda tunnor. Den kan dock inte skriva tunnorna direkt (främst för att varje tunna finns i många exemplar och för att det skulle störa sökningen), så den skapar en lista med krav på hur den framtida tunnan ska se ut och lagrar dessa i ett tillfälligt minne. I vårt fall kan en sådan lista se ut så här (vissa oviktiga ord kan ignoreras eller markeras som stoppord):

| Dokument-ID | Word | Position | Typ |
|--------------|-------|--------|-----------|
| 1 | Cat | 1 | Normal |
| 1 | Inrikes| 2 | Normal |
| 1 | Felis | 3 | Normal |
| 1 | Is | 7 | StopWord |
| 1 | Domesticerad | 8 | Normal |

En sådan lista är enorm (mycket större än de ursprungliga texterna), men det går ändå snabbare att söka eftersom vi inte behöver läsa alla texter i följd, utan kan söka efter index i varje kolumn och orden från en sida sorteras i rader efter varandra.

När en viss tid har gått (eller ett visst antal dokument har färdigställts) slutar indexeraren att arbeta med att bygga upp denna lista över förfrågningar (för en framtida tunna) och börjar läsa uppgifterna igen och bygga upp de enskilda tunna (denna lista innehåller gamla poster som finns i en tunna som redan är i arbete). Om nya poster läggs till från kända adresser kommer processen att uppdatera dem, medan nya dokument kommer att inkluderas.

På detta sätt går indexeraren igenom listan igen och skapar långsamt nya tunnor som innehåller alla element (de ser likadana ut som i exemplet i föregående kapitel) och när alla tunnor är färdiga skickas de till de enskilda sökservrarna. Uppdateringen av tunnorna på servrarna är tidskrävande (främst på grund av den stora mängden data som flyttas), så under denna operation är servrarna inte tillgängliga och data uppdateras på olika servrar vid olika tidpunkter. Detta leder till exempel till att vissa användare kan få olika resultat eftersom varje användare söker på en annan server (på grund av belastningsdelning). När uppdateringen är klar är allt åter normalt och alla användare hittar alla dokument på samma sätt.

Indexeringsprocessen är viktig för alla sökmotorer, och den som gör den mest frekvent och omsorgsfullt är den som har den mest uppdaterade bilden av Internet. Google gör detta med några timmars mellanrum, Seznam en gång i veckan (och har miljontals gånger mindre data).

Kanonisering av dokument
--------------------

I den ursprungliga utformningen av fulltextsökmotorn fanns det inget behov av något som kanonisering eftersom Internet var ett medium som ständigt skapade nytt innehåll. Med tiden har dock duplicering (dvs. att samma innehåll visas på flera olika webbadresser) uppstått, och sökmotorerna måste anpassa sig till detta. Ett typiskt exempel är Wikipedia, som har många artiklar. Vissa författare till andra sidor tar över dessa texter (delvis eller helt) och skapar på så sätt dubbelarbete. I de flesta fall spelar detta dock ingen roll, eftersom källsidan har en mycket högre rank (länkkvalitet) än den plagierade sidan, men det kan ibland hända att det försämrar originalet på piratkopierarens bekostnad.

Sökmotorerna har varit tvungna att anpassa sig till denna brist och termen "kanonisering" har myntat, vilket kan förstås som att vissa sidor tas bort från indexet. Detta gör för övrigt indexen mindre, och sökmotorn behöver inte gå igenom samma innehåll hela tiden i onödan.

Duplikat är internt uppdelade i två stora kategorier av varje sökmotor:

Naturliga dubbletter
-------------------

Dessa skapas av Internets naturliga beteende och av dess egenskaper.

Till exempel är det troligt att URL-adressen `http://mathematicator.cz` har samma innehåll som URL-adressen `http://www.mathematicator.cz` eller `http://mathematicator.cz/index.php` eftersom detta är standardbeteende för Apache-servern (och Internet i allmänhet).

Om en naturlig duplicering upptäcks skapar sökmotorn en "kanonisk uppsättning", vilket är en grupp sidor från vilka sökmotorn väljer en representant som sticker ut i sökningen. Om en länk leder till en sida från den kanoniska uppsättningen kommer dess rang automatiskt att överföras till huvudrepresentanten.

Det är ofta en bra idé att hjälpa sökmotorn att skapa denna uppsättning och att ställa in omdirigeringen korrekt på webbplatsen, vilket gör att sökmotorn ser bättre på webbplatsen och att huvudrepresentanten väljs bättre.

Duplikat som leder till plagiat
----------------------------

Plagiering är ett problem på Internet idag. Plagiering i sig skulle inte spela någon större roll, men användarna är särskilt besvärade av det eftersom de hela tiden hittar samma resultat för samma fråga. Har du också någonsin hittat flera sidor med samma text för en sökning? Det är just detta beteende som sökmotorerna försöker förhindra.

Det största problemet är att avgöra vilken sida som är originalkällan - och att göra det maskinellt. Sökmotorerna samlar alla liknande sidor i en kanonisk uppsättning och väljer en huvudrepresentant från denna uppsättning. Om källorna kommer från olika domäner kan situationen inte betraktas som en naturlig duplicering (och vilken kandidat som helst kan väljas), utan alla sidor måste utvärderas kvalitativt och den bästa måste väljas objektivt - och helst den ursprungliga källan till innehållet.

Sökmotorer fattar ofta beslut som baseras på hela domänens rang och styrkan i länknätverket till ett visst dokument, men även detta är ett ganska opålitligt tillvägagångssätt. Den andra faktorn är vanligtvis också den tidpunkt då dokumentet skapades (indexering). Därför håller varje sökmotor reda på vilka sidor som ofta används för att skapa nytt innehåll och besöker dessa sidor ofta, så att den nya sidan helst märks direkt och därför inte väljs som proxy.

En detaljerad beskrivning av metoderna för hur detta urval fungerar ligger utanför ramen för denna artikel och skulle kunna bli föremål för en hel bok.
