Lokalt känslig hashing
======================

> id: '76e09294-08ea-4dde-a431-2116595d9f04'
> slug:
> 	cs: lokalne-senzitivni-hashovani
> 	sv: lokalt-kaenslig-hashing
> 
> publicationDate: '2022-07-11 10:45:00'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: c23def9586aa05d73c4f1cf1fd1dda43

Principen för de flesta hashfunktioner för fingeravtryck av dokument är att de alltid ger samma resultat för varje enskild inmatning. Detta kallas deterministiskt beteende. Samtidigt kan en liten förändring i inmatningen orsaka en stor förändring i utmatningen (vilket i praktiken ger en helt annan hash). Detta är särskilt användbart när vi vill kontrollera om indatadokumentet har ändrats, och om det har ändrats får vi något helt annat. Ett exempel på en sådan funktion är MD5 eller SHA1.

Om vi vill härleda innehållet i den ursprungliga inmatningen från hashen finns det inget enkelt sätt att göra detta och vi har inget annat val än att använda brute force för varje alternativ tills vi får en träff. Detta beror på att vi på grund av den stora förändringen i resultatet inte kan avgöra genom iterationer om vi närmar oss målet eller inte.

Vissa hash-funktioner, t.ex. BCrypt, ger för samma indata en annan utgång varje gång, vilket gör dem robusta när det gäller hashning av lösenord. I själva verket innehåller utdata direkt salt, vilket gör en så kallad ordboksattack omöjlig. Den här funktionen är användbar för hashning av lösenord, men är mycket olämplig för granskning av dokument.

Kontroll av dokumentlikhet och sökning efter innehållsdubblering
-----------------------------------------------------------

Föreställ dig att vi löser en algoritm för en sökmotor som vill kontrollera hur mycket en webbsida har ändrats. Alternativt vill man snabbt kontrollera om texten på en sida liknar texten på en annan sida, eller till och med är helt duplicerad.

Ett sätt att kontrollera detta är att jämföra varje sida med varandra, vilket är mycket resurskrävande för systemet. Ett annat alternativ är att beräkna en SHA1-hash, men detta hashar innehållet i hela dokumentet, och om sidan ändras med bara ett tecken får vi en annan hash - så det är inte lämpligt för dessa ändamål.

En möjlig lösning är därför att beräkna hashen från olika delar av dokumentet på olika sätt och sedan leta efter förändringar i hashfunktionens resultat.

Principen för lokal känslig hashning
----------------------------------

Vi har laddat ner HTML-koden för en webbsida som vi vill beräkna en hash för. Vi delar in dokumentet i olika regioner (celler) med hjälp av en algoritm som måste konfigureras korrekt.

Vi hashar varje region oberoende av varandra med hjälp av en gemensam algoritm och sammanfogar resultatet till en enda sträng.

Resultatet kan då vara till exempel `3791744029724361934` (denna hash för innehållet tillhandahölls av Ahrefs-verktyget).

Om vi till exempel vet att innehållet i rubriken representerar de första 5 tecknen och att sidfoten representerar de sista 5 tecknen, vet vi att mitten av strängen representerar sidans innehåll. Om innehållet i sidfoten ändras på alla sidor (t.ex. om webbansvarig lägger till en ny länk, uppdateringsdatumet ändras osv.), ändras endast några tecken i hash-koden i det högra området något när den nya versionen av sidan laddas ner, och vi vet att endast sidfoten har ändrats, men att innehållet är oförändrat. Vi kan alltså ignorera en sådan ändring och behöver inte gå igenom hela webbplatsen.

Hur optimerar Google krypningen av webben?
----------------------------------------

Internetrobotar måste spara där de kan. Webbsökning är mycket dyrt och uppdateringar kostar beräkningstid.

När jag till exempel observerade hur Googles robotalgoritm beter sig, såg jag att den bara reagerar på stora förändringar i innehållet. Om sidan förändras lite, kryper den på normalt sätt. Men när t.ex. sidans sidfot och rubrik ändras avsevärt, bedöms det som en omarbetning och större delen av webbplatsen behandlas på en gång för att få det nya utseendet så snart som möjligt.

Den upptäcker också dubbletter mellan webbplatser på ett liknande sätt. När en grupp sidor är mycket lika eller har samma innehåll får de en mycket likartad hash som roboten kan använda för att snabbt kontrollera att dokumenten är lika och välja den kanoniska sidan och ignorera resten.

Implementering av hash-funktionen
-----------------------------

Det finns ingen färdig implementering av den lokalkänsliga hashfunktionen i PHP. Samtidigt känner jag inte till något fritt tillgängligt paket som implementerar denna funktion på ett bra sätt.
