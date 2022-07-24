Hur du effektivt optimerar sidans laddningshastighet
====================================================

> id: '72f1d564-cb70-431c-a3dd-a3fdcaf14292'
> slug:
> 	- null
> 	sv: hur-du-effektivt-optimerar-sidans-laddningshastighet
> 
> cs: optimalizace-rychlosti-nacitani
> publicationDate: '2021-10-15 15:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '013c8f29c213d3f3f1df3fbe4be8d572'

När jag reser stöter jag ofta på dåliga internetuppkopplingar, vilket som webbdesigner får mig att tänka på designprinciper för att lösa webbhastigheten även på en dålig uppkoppling.

Jag har kommit på några användbara knep genom att öva:

Viktiga landningssidor är ofta enkla och det lönar sig att använda anpassad CSS-formgivning.
-----------------------------------------------------------------------------------

Till exempel är inloggningssidan till CMS ofta mycket enkel. Behöver ett enkelt formulär verkligen ladda ner hela Bootstrap och andra CSS/JS-frameworks? Viktiga sidor är värda att optimera och skriva inhemsk kod.

När sidan är helt renodlad får vi några sekunder på oss att låta användaren fylla i sina uppgifter, och vi kan ladda ner och cachelagra de återstående stilarna i webbläsaren i bakgrunden. När användaren loggar in har han/hon förmodligen redan laddat ner Bootstrap (och om han/hon använder Edge har han/hon inte laddat ner Bootstrap...).

Istället för ikoner kan vi ofta använda emoji.
-----------------------------------

Verkligen! Emoji har många praktiska fördelar:

- Den tar bara upp 4 bytes, så den övertrumfar alla bilder.
- Den misslyckas aldrig med att ladda ner, så användaren ser alltid åtminstone en ikon i stället för ett tomt utrymme.
- Visar i användarens visuella stil
- Har för närvarande redan ett brett stöd för enheter
- Sparar serverförfrågan och vi behöver inte ta itu med varifrån bilden ska hämtas (detta kan optimeras via CDN, men varför, eller hur?).
- Det finns många ikoner som du förmodligen inte vill ha med att göra. Var kan du till exempel hitta flaggikoner för alla länder som du vill visa i administrationen? Använd emoji: https://github.com/bara.../country/blob/main/flag-emoji.json

Använd kritiska stilar direkt i HTML när webbplatsen laddas.
------------------------------------------------------

Ibland lägger jag in viktiga CSS-stilar som definierar sidlayouten och den grundläggande layouten direkt i HTML. Jag sätter en gräns på 1 KB, som jag försöker få in så mycket som möjligt. Nackdelen med detta tillvägagångssätt är att stilar måste överföras vid varje begäran och inte kan lagras i cacheminnet, men å andra sidan laddas de ner ojämförligt mycket snabbare än en bild.

Jag är ingen expert på laddningshastighet, [Martin Michalek] (https://www.programia.cz/rozhovor-martin-michalek-rychlost-webu/) skulle kunna berätta mer om det.

Använd ajax!
--------------

Jag använder alltid Ajax för att hämta oviktigt eller långsamt innehåll. Det ökar de tekniska kraven för applikationen lite, men jag har råd med det.

Ett exempel på ett bra ställe att använda Ajax är nästan allt i administrationen. Ofta finns det mycket data att hämta, men användaren är inte intresserad av allt. När användaren bara har en tunn javascriptklient nedladdad och all data hämtas via Vue och json, hämtas endast en minimal mängd data och svaren är omedelbara.

Hur man gör detta i Vue: https://vue.baraja.cz/api-a-ajax-ve-vue-js

På baksidan använder jag biblioteket för Nette: https://github.com/baraja-core/structured-api

Använd en lämplig CDN
---------------------

För distribution av statiskt innehåll rekommenderar jag att du använder en CDN för alla typer av webbplatser. Om du inte vet hur man konfigurerar ett CDN kan du åtminstone använda Cloudflare i proxyläge. Den cachelagrar automatiskt statiskt innehåll till sig själv baserat på HTTP-huvuden. Det är inte alla webbhotell som har Pops väl inställt, och för långa rutter kan detta spara hundratals millisekunder i varje begäran.

Lämpligt bildformat
---------------------

En av mina juniorer lade nyligen upp en PNG-bild på webbplatsens huvudsida, som innehöll ett foto och tog upp 3 MB. Han sa att det var okej eftersom sidan laddades snabbt på hans anslutning (det gör den på hans optik hemma, eller hur?), och han sa att vi överför mycket data nuförtiden, till exempel video.

Jag är gammalmodig och optimerar vad jag kan.

Foton till JPG, eller bättre WEBP. Men att spara en bild till JPG betyder ingenting, du måste köra den genom en optimeringstjänst: https://tinyjpg.com

Om du har bilder i Git kan det här verktyget automatiskt komprimera dem och skicka en pull request: https://imgbot.net. När en ny bild läggs till skickas PR-meddelandet på nytt.

Om du behöver komprimera tusentals bilder (t.ex. ett helt galleri med produkter på en webbplats) använder jag ett datorprogram för Mac för att göra det: https://imageoptim.com/mac

Genom att komprimera bilder på lämpligt sätt sparas vanligtvis mest data. Ibland till och med 50 procent.
