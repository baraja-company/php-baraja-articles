Vad jag skulle ändra på Nette om jag var David Grudl
====================================================

> id: bad8fe4f-92a4-4396-8698-7ec42bb2aef8
> slug:
> 	- null
> 	sv: vad-jag-skulle-aendra-pa-nette-om-jag-var-david-grudl
> 
> cs: co-bych-zmenil-na-nette
> publicationDate: '2022-11-18 17:45:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '953e6c78b9dff185bbf8e3e9582d24c3'

Jag har aktivt använt Nette Framework i nästan 6 år för kommersiell programvaruutveckling. Under de första åren var jag mycket entusiastisk och ramverket svarade perfekt på alla de behov vi hade som team, och det fanns i princip ingen anledning att leta efter ett annat verktyg.

Sedan ungefär 2019 har jag börjat se att den ursprungliga entusiasmen för Nette har minskat och jag börjar sakna några av de avancerade funktionerna. Ofta handlar det om saker som ligger utanför själva ramverket, så jag förväntar mig inte ens att de någonsin kommer att implementeras, men å andra sidan måste en utvecklare fatta ett beslut om hur han eller hon ska fortsätta utvecklingen och kanske välja en annan teknik.

Databasskikt
-----------------

Jag anser att Nette Database tyvärr är ett av ramverkets största misstag.

Varje vecka har vi intervjuer på företaget där juniorer direkt från skolan, eller till och med personer på mellanstadiet som går över från ett annat ramverk, ansöker och upptäcker Nette Database som en del av sin utforskning av Nette. Som databaslager är det inte så dåligt, men problemet är den praktiska användningen i verkliga projekt.

Detta beror på att Nette Database inte anstränger sig för att tvinga utvecklare att använda strikta datatyper från början, att namnge kolumner och relationer, att separera sökmetoder (repository) från modeller, och andra små nyanser som gör mycket nytta totalt sett.

I många år använde jag biblioteket Dibi, som spelade en stor roll på sin tid. Den gjorde det möjligt att söka i databasen på ett bra sätt och förenhetligade gränssnittet. Å andra sidan har tiderna gått vidare och när databaser returnerar otyperade objekt är PHPStan inte nöjd av principiella skäl.

Personligen skulle jag antingen införliva stöd för enheter och arkiv i Nette Database eller ge officiellt stöd för Doctrine i Nette. Om du programmerar program på samma nivå som ett stort företag måste du ta utvecklingen på allvar, och Doctrine är särskilt meningsfullt. Samtidigt vill du utbilda nykomlingar i bättre teknik direkt och du vill inte lära dem gamla vanor.

Loggning av spår och fel
---------------------

Jag anser fortfarande att Tracy är den bästa felsökaren för PHP.

När jag kom till en icke namngiven digitalbyrå 2017 och såg det tekniska tillståndet i deras Nette-projekt blev jag förskräckt. Hur många gånger hade de webbplatser som var skrivna i ren PHP utan något försök att logga fel. En ganska enkel lösning var att installera Tracy på projektet, anropa `Debugger::enable()` och inte göra något mer. Buggarna loggades automatiskt i en katalog som bara behövde plockas upp manuellt med några dagars mellanrum.

När det gäller Tracy verkar det för mig som om det är en färdig produkt som David inte har någon större anledning att förbättra. Å andra sidan ser jag fortfarande utrymme för förbättringar i en ny riktning.

Varför har Tracy till exempel inga betalfunktioner för företag eller privatpersoner som menar allvar med säkerhet?

Tracy kan implementera ett anpassat webbgränssnitt av Sentry-typ där du skapar ett konto och där produktionsfel automatiskt skickas via ett API där du kan spåra dem i alla projekt. Appen skulle också kunna begära in enskilda webbplatser och automatiskt upptäcka att de är otillgängliga och meddela ägaren på andra sätt (eftersom Tracy inte logiskt kan upptäcka en serverkrasch). Ibland stöter jag också på problemet att Tracy oavsiktligt har aktiverats på en produktionsanläggning, och du kan ta reda på via DIC hur du får tillgång till databasen eller någon annanstans. Tracy bör också uppmärksamma dig på detta.

Tracy kan också implementera andra små saker som jag känner till från Node.js. Till exempel skulle det vid visning av källkod kunna vara möjligt att välja det teckenintervall vid vilket linjen markeras på ett särskilt sätt, varför det är möjligt att markera en specifik symbol inom en rad. Samtidigt skulle det vara trevligt att lägga till stöd för en andra (kanske blå) markeringslinje för att visa sammanhanget i nästa del av programmet.

När jag visar en bit av källkoden skulle jag inte vara rädd för att lägga till en knapp som kan återge resten av koden i Ajax så att jag kan utforska den direkt i webbläsaren från callstack. Detta är vad Symfony gör, och det är en fantastisk funktion. Om detta kompletterades med en grundläggande statisk analys med möjlighet att söka igenom metoder, klasser och arv skulle det spara mycket felsökningsarbete för hela teamet.

Om callstack går igenom ett paket skulle det vara bra att visa versionen av paketet jag arbetar med för en viss fil. Det händer ofta att jag får ett fel i ett paket som jag håller på att utveckla, och jag kan lika gärna veta visuellt att det är en äldre version.

Statisk routning
----------------

Inom Nette Router skulle jag tillåta att definiera en ny typ som kallas statisk router. Det skulle vara en efterföljare till den grundläggande routern, men den skulle bara kunna acceptera string-literal, som inte skulle vara ett reguljärt uttryck utan en rak väg. Många sidor fungerar statiskt (kontakter, olika artiklar, förvånansvärt ofta även hemsidan), och routern måste därför utvärdera regex-regler för matchning och URL-sammansättning varje gång.

Om en statisk väg används i en Latte-mall kan den till exempel översättas till en statisk sträng `{$basePath}/path` vid kompileringen av mallen, så att det inte finns något behov av att kompilera de 100 URL:erna som finns i layouten vid varje begäran.

Jag förstår att jag kan uppnå samma funktionalitet genom caching på mallsidan, men jag skulle uppskatta en systemlösning mycket mer.

I Next.js ramverket föll jag helt för konceptet statisk routing. Det finns inget sådant som `n:href`, kort sagt måste du känna till URL:n i förväg, vilket gör att du inte behöver göra så många omdirigeringar, tänka på URL-format i förväg och hålla dem beständiga.

PSR-gränssnitt
------------

Jag tycker att det är synd att Nette inte har ett naturligt genomförande av PSR-gränssnittet. Som paketutvecklare leder detta dig vilse om du vill definiera ett Composer-beroende av Nette överhuvudtaget. Å ena sidan löser Nette många problem för dig, å andra sidan vet du att du helt enkelt inte kommer att kunna ersätta den efteråt. Mer än en gång har jag upptäckt att jag föredrar att använda Symfony, Laravel eller ett helt annat generiskt gränssnitt på grund av bristen på ett generiskt gränssnitt.

Bristen på stöd för generiska standarder för framtida portabilitet är den främsta anledningen till att jag lämnade Nette helt och hållet år 2022 och att vi utvecklar nya tillämpningar i team uteslutande i generiska standarder.

API-skikt
----------

Vad händer om du vill använda Nette som en backend för att hantera REST API-förfrågningar? Bygger du en presentatör och skickar svaret som `$this->sendJson()`? Ska du installera ett bibliotek från tredje part? Eller är du David Grudl som löser behovet av ett bibliotek som saknas genom att skriva ett nytt på en gång?

Varför kan Nette inte implementera något så vanligt som REST API direkt från start? Idag behöver nästan alla program kommunicera via ett API - till exempel för att tillhandahålla data till frontend.

För nykomlingar leder detta återigen till att de använder de inbyggda Latte och snippets i stället för att upptäcka att det finns ett bättre alternativ, nämligen att bygga hela frontend separat vid sidan av och bara tillhandahålla data till den.

Att lita på vad som kommer att hända om 10 år
-------------------------

Den sista punkten är mycket pragmatisk och kommer från en debatt som vi då och då hör från affärsavdelningar i teamet.

Vad skulle hända om David Grudl slutade utveckla Nette helt och hållet? Vad händer om någon slutar utveckla den? Vad skulle vi byta till? Och hur utmanande skulle det vara?

Många utvecklare (som ofta inte har någon erfarenhet från företag) hånar denna oro och säger att det är okej. Ja, men det är det inte.

När du står inför frågan om du ska investera tid i Nette eller React och Node.js för att utbilda unga utvecklare på ditt företag, vinner tyvärr det senare alternativet.

Nette har inte missat tåget ännu, men i de dussintals intervjuer som jag har haft under de senaste sex månaderna känner jag det ganska starkt.
