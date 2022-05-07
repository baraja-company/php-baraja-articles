En dag kommer hackare att attackera din webbplats
=================================================

> id: a11ca0a6-77e1-4ba4-8eca-83449c9cbf48
> slug:
> 	cs: jednou-vam-hackeri-napadnou-web
> 	sv: en-dag-kommer-hackare-att-attackera-din-webbplats
> 
> publicationDate: '2020-08-04 12:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: a74c964823fb0c4231b711d113055a24

Det kommer inte att hända dig? :DDDDDDDDD

När jag förvaltar mer än 300 webbplatser får jag då och då olika nödsituationer. Ibland är de ganska heta, men ofta är det en fullständig trivialitet. Om du, liksom jag, har blivit frestad av programmering tidigare och vet att [programmering är en plåga] (http://borisovo.cz/programming-sucks-cz.html), håller du definitivt med mig.

I händerna på onda hackare
----------------------

När en webbapplikation blir populär blir den ett lockande mål för hackare. Deras motiv är vanligtvis inte att sänka hela webbplatsen, tvärtom. Faktum är att **hackarna vill att du som serveradministratör ska vara helt omedveten om dem**. En duktig hackare väntar i flera månader, bevakar webbservern och får tag på det mest värdefulla - dina data.

För att skydda dina användare är det viktigt att kryptera all data och ha flera skyddslager. För lösenord använder du en av de långsamma funktionerna som [Bcrypt + salt](https://php.baraja.cz/hashovani), Argon2, PBKDF2 eller Scrypt. Michal Spacek har redan skrivit om [säkerhetsklassning] (https://pulse.michalspacek.cz/passwords/storages/rating#slow-hashes), och jag tackar honom för att han kompletterar artikeln. Som webbplatsägare måste du insistera på korrekt lösenordshashning när du diskuterar med en programmerare. Annars kommer du att gråta, precis som många människor före dig som också lurade sig själva att tro att det inte rörde dem.

Kan du se när du blir attackerad?
---------------------------------

Jag har lagt märke till att när en webbserver når en viss tröskel av trafik (i Tjeckien är det ungefär 50+ besökare per dag), börjar den få en rad attacker riktade mot sig från ingenstans. För att göra det inte så lätt har angriparen alltid en fördel, eftersom han kan välja vilken del av infrastrukturen han vill börja attackera och du - som ägare av webbservern - måste upptäcka det i tid, försvara dig och vara bättre förberedd på det nästa gång.

Hur många attacker har riktats mot dig till exempel under den senaste månaden? Vet du exakt vad? Hur många av dem har du försvarat? Har någon annan tillgång till din server?

Vad händer om någon attackerar dig just nu? Och nu... och nu också...?

Tyvärr finns det inget universellt sätt att känna igen en attack. Men det finns verktyg som kan ge dig en betydande fördel.

Det som har fungerat bäst för mig på sistone:

- **När en angripare inte vet var dina servrar befinner sig fysiskt** har han eller hon en mycket svårare position. Information om den fysiska arkitekturen kan döljas mycket bra med [Clouflare] (https://www.cloudflare.com/), som ger proxyservice (och krypterar all nätverkskommunikation i båda riktningarna). Den har också ett antal andra fördelar, t.ex. snabbare hämtning från utlandet, automatiskt skydd mot DDOS-attacker, komprimering av tillgångar och sist men inte minst automatisk blockering av oseriösa besökare. Jag använder Cloudflare för alla mina webbplatser (och nästan alla projekt använder olika tekniker).
- **Fel loggning**. Alltid och i allt. Det är troligt att din applikation innehåller många fel som programmeraren har begått. Det kan vara [uncaught exceptions] (https://php.baraja.cz/vyjimky), programfel, SQL-injektion och så vidare. Om du programmerar i PHP och inte förstår dig på loggning finns det tillräckligt många problem som [Tracy](https://tracy.nette.org/) (och eventuellt andra verktyg som [Sentry](https://sentry.io/)) och åtkomstloggar på själva servern kan upptäcka. Felloggning är inte ett trevligt alternativ, utan ett måste. Jag loggar buggar på alla webbplatser som jag aktivt underhåller, och jag får information om nya buggar skickad till min e-post så att jag får reda på dem omedelbart.
- **Säker och uppdaterad plattform**. Har du WordPress på din webbplats? Vet du hur man uppdaterar den på rätt sätt? Känner du till alla risker som du står inför? När något är gratis är det nästan alltid på bekostnad av något annat. Personligen använder jag [Nette framework] (https://nette.org/cs/) version 3 för programutveckling, som jag uppdaterar varje vecka och aktivt letar efter säkerhetsbrister att åtgärda. På samma sätt som du får din bil regelbundet servad, måste du få din webbplats servad regelbundet och minst en gång i månaden. Webbplatsunderhållet omfattar uppdatering av alla externa bibliotek och kontinuerlig övervakning av loggar. Om du använder en gammal version av ett bibliotek är det mycket troligt att en angripare känner till sårbarheten och försöker utnyttja den.
Frågan är när
Ett stort antal människor i mitt område underskattar säkerheten på ett otroligt sätt och lägger ut innehåll på Internet som är mycket lätt att bryta sönder och utnyttja.

Faktum är att även stora företag gör misstag, till och med när det gäller triviala saker som [XSS](https://www.zive.cz/clanky/vyvojar-objevil-zranitelnost-v-seznamu-dokazal-mezi-vysledky-propasovat-zakazany-kod/sc-3-a-200023/default.aspx) (cross-site scripting)-attacker.

Frågan är inte om någon någonsin kommer att bryta din webbplats. Frågan är när det kommer att hända och om du är tillräckligt förberedd för att hantera det. Kanske har en hackare haft tillgång till din server i flera månader utan att du vet om det (ännu).

Vad du ska göra när det uppstår problem
-------------------------------

När du får reda på hackarens arbete är det oftast för sent och hackaren har gjort allt han eller hon behövde göra. Han har med stor sannolikhet placerat mallware över hela servern och har åtminstone delvis kontroll över maskinen.

Detta är ett **kaotiskt problem och du måste agera snabbt**.

Eftersom detta är det sista steget i attacken rekommenderar jag personligen att du kopplar bort webbservern helt från internet och börjar gradvis ta reda på vad som hände. Dessutom vet vi aldrig exakt om servern döljer något annat. Angriparen har alltid ett stort försprång.

Om vi bestämmer oss för att lägga tillbaka den sista fungerande säkerhetskopian på servern kommer vi att hjälpa angriparen mycket. Han vet redan hur han ska bryta sig in på din server och det finns inget som hindrar honom från att göra det igen om några timmar. Dessutom innehåller säkerhetskopian förmodligen mallware eller någon annan typ av virus.

Även om detta är ett mycket impopulärt alternativ bland mina klienter rekommenderar jag personligen alltid att du först raderar WordPress-webbplatser** eller andra system som klienten inte uppdaterar och som har många säkerhetshot. Om du till exempel har 30 webbplatser på en server som är mer än två år gamla är det praktiskt taget säkert att minst en av dem innehåller någon form av sårbarhet.

Om du inte förstår frågan är det bäst att du tidigt kontaktar en lämplig specialist som har djup kunskap om din fråga och som förstår saker och ting i ett brett sammanhang.

**Etisk anmärkning:**

Om du förvaltar en webbplats för en kund som har haft en säkerhetsincident är det ditt ansvar att informera dem. Om uppgifter (t.ex. lösenord, men ofta mycket mer) läcker ut måste du också informera slutanvändarna om att deras lösenord är allmänt känt och att de bör byta det överallt. Om du använder en långsam hashfunktion (till exempel **Bcrypt + salt**) gör du det betydligt svårare för en angripare att knäcka lösenord (tyvärr kan [Bcrypt-lösenord knäckas] (https://arstechnica.com/information-technology/2015/08/cracking-all-hacked-ashley-madison-passwords-could-take-a-lifetime/)). Om du vill få regelbunden information om huruvida ditt konto har läckt ut rekommenderar jag att du registrerar dig på [Have i been pwned?] (https://haveibeenpwned.com/). Mer information om [säkerhetsöverträdelse] (https://m.uoou.cz/vismo/zobraz_dok.asp?id_ktg=5020&n=poruseni-zabezpeceni) finns på OOOO:s webbplats.

Atomära vanor
--------------

Under de senaste sex månaderna läste jag boken [Atomic Habits] (https://www.melvil.cz/kniha-atomove-navyky/), vilket var en ögonöppnare. Den beskriver en process där man varje dag förbättrar sig med 1 procent, vilket kommer att göra mycket i det långa loppet.

Eftersom jag kontaktas av företag och personer som befinner sig i olika stadier av teknikskulder vill jag sammanfatta de värsta sakerna:

- **FTP för serverkommunikation är inte meningsfullt**. Det är ett osäkert protokoll som styrs av lösenord. Om du vill ge någon annan tillgång måste de känna till lösenordet och kan göra samma sak som du. Om du vill ta bort hans åtkomst kan du inte göra det, det enda sättet är att ändra lösenordet. Det är bättre att helt byta till SSH och använda RSA-nycklar. Jag är ofta förvånad över att till och med företag löser detta problem i dag. Gör aldrig en tabell med alla lösenord. Och definitivt aldrig en gemensam för hela laget!
- **Källkoden finns i Git**, uppgifterna i databasen och det statiska innehållet (bilder, PDF-filer, ...) i filer på disken. Om du väljer fel datalagring försämrar du utformningen och säkerheten av programmet. URL:en till en PDF-fil (t.ex. en faktura) bör innehålla slumpmässigt genererat innehåll som endast mottagaren känner till. När det gäller extremt känsligt innehåll (t.ex. ett kontoutdrag) är det viktigt att kryptera innehållet på disken och endast tillhandahålla det efter inloggning.
- Icke-offentliga delar av webbplatsen måste innehålla META-huvudet `noindex` och `nofollow` för att inte indexeras av Google. Känsligt innehåll som dina konkurrenter inte får veta måste skyddas med lösenord.
- Inaktivera [directory content listing] (https://www.simplified.guide/apache/disable-directory-listing) på din server. Om den är felaktigt inställd kan hela webbplatsen genomsökas för att hitta känsliga filer.
- Projekt root! Det får aldrig finnas en direkt URL till konfigurationsfiler. Till exempel [Nette does it right] (https://doc.nette.org/cs/3.0/quickstart/getting-started#toc-obsah-web-projectu) redan i den första handledningen. Detsamma gäller [publicly available git repositories] (https://smitka.me/open-git/). Denna typ av sårbarhet är mycket lätt att underskatta och konsekvenserna är ofta tragiska.
- Felet kan också orsakas av ett oavsiktligt misstag vid utvecklingen. Det är mycket viktigt att göra en kodgranskning av en levande människa för varje ändring. Många fel upptäcks av [PHPStan] (https://github.com/phpstan/phpstan) eller liknande verktyg innan en människa ser koden.
- Skicka aldrig [lösenord i läsbar form via e-post] (https://www.lupa.cz/clanky/reset-a-poslani-hesla-v-citelne-podobe-e-mailem-nebezpecna-praktika/), det finns alltid ett annat sätt att hantera det.
- Säkerhetsproblem i gamla versioner av bibliotek och programvara. Robotar kryper runt på internet varje sekund och attackerar kända säkerhetshål (SQL-injektion, XSS, CSRF, ...). Många kända hål har äldre versioner av WordPress.

Det finns många fler sårbarheter och problemen varierar från projekt till projekt. Om du behöver göra en snabb serverrevision rekommenderar jag att du använder [Maldet](https://www.rfxn.com/projects/linux-malware-detect/) och sedan anlitar en lämplig specialist som hjälper dig att göra en [fullständig webbplatsrevision](https://baraja.cz/audit-webu), och inte bara av säkerhetsskäl.

Säkerhetsingenjörer är som plast i havet - bara för evigt. Vänj dig vid det.

Naturens princip om handling och reaktion
-------------------------------

Du måste också ha märkt att naturen alltid använder sig av reaktionsprincipen. Detta innebär att något händer i en viss miljö och att de omgivande organismerna reagerar på det på olika sätt. Detta tillvägagångssätt har många fördelar, men en stor nackdel - du kommer alltid att hamna på efterkälken.

Som tänkande person (designer, utvecklare, konsult) har du en stor fördel, nämligen att du kan vidta åtgärder - det vill säga att du i förväg känner till en viss del av de stora riskerna och aktivt kan förhindra att de inträffar från första början. Du kan kanske aldrig förhindra alla misstag, men du kan åtminstone mildra konsekvenserna eller bygga upp kontrollmekanismer som upptäcker problem tidigt så att du hinner reagera.

De flesta attacker sker under en lång tidsperiod, och om du loggar har du oftast tillräckligt med tid för att upptäcka problemet.
