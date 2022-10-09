En dag vil hackere angribe dit websted
======================================

> id: a11ca0a6-77e1-4ba4-8eca-83449c9cbf48
> slug:
> 	cs: jednou-vam-hackeri-napadnou-web
> 	da: en-dag-vil-hackere-angribe-dit-websted
> 
> publicationDate: '2020-08-04 12:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: a74c964823fb0c4231b711d113055a24

Det vil ikke ske for dig? :DDDDDDDDD

Mens jeg administrerer mere end 300 websteder, får jeg fra tid til anden forskellige nødsituationer. Nogle gange er de ret ophedede, men ofte er det en fuldstændig banalitet. Hvis du ligesom jeg tidligere har været fristet af programmering, og hvis du også ved, at [programmering er irriterende] (http://borisovo.cz/programming-sucks-cz.html), vil du helt sikkert være enig med mig.

I grebet af onde hackere
----------------------

Når en webapplikation bliver populær, bliver den et fristende mål for hackere. Deres motivation er normalt ikke at nedlægge hele webstedet, tværtimod. Faktisk ønsker **hackerne, at du som serveradministrator skal være fuldstændig uvidende om dem**. En god hacker venter i månedsvis, holder øje med webserveren og får fat i det mest værdifulde - dine data.

For at beskytte dine brugere er det vigtigt at kryptere alle data og have flere lag af beskyttelse. Til adgangskoder skal du bruge en af de langsomme funktioner som f.eks. [Bcrypt + salt](https://php.baraja.cz/hashovani), Argon2, PBKDF2 eller Scrypt. Michal Špaček har allerede skrevet om [sikkerhedsvurdering] (https://pulse.michalspacek.cz/passwords/storages/rating#slow-hashes), og jeg takker ham for at supplere artiklen. Som ejer af et websted skal du insistere på korrekt hashing af adgangskoder, når du diskuterer med en programmør. Ellers kommer du til at græde, ligesom mange andre før dig, der også har bildt sig selv ind, at det ikke vedrører dem.

Kan du se, hvornår du er under angreb?
---------------------------------

Jeg har bemærket, at når en webserver når en vis tærskel for trafik (i Tjekkiet er det omkring 50+ besøgende om dagen), begynder den at få en række angreb rettet mod den ud af det blå. For at gøre det ikke let har angriberen altid en fordel, fordi han kan vælge, hvilken del af infrastrukturen han vil angribe, og du - som ejer af webserveren - skal opdage det i tide, forsvare dig og være bedre forberedt næste gang.

Hvor mange angreb er der f.eks. blevet rettet mod dig i den seneste måned? Ved du det præcist? Hvor mange af dem har du forsvaret? Er der andre, der har adgang til din server?

Hvad nu, hvis nogen angriber dig lige nu? Og nu... og nu også...?

Desværre er der ikke nogen standardiseret måde at genkende et angreb på. Men der findes værktøjer, som kan give dig en betydelig fordel.

Det, der har virket bedst for mig på det seneste:

- **Når en angriber ikke ved, hvor dine servere befinder sig fysisk**, har han en meget vanskeligere position. Fysiske oplysninger om arkitekturen kan sløres meget godt med [Clouflare] (https://www.cloudflare.com/), som giver proxyservice (og krypterer al netværkskommunikation i begge retninger). Den har også en række andre fordele, såsom hurtigere hentning fra udlandet, automatisk beskyttelse mod DDOS-angreb, komprimering af aktiver og sidst men ikke mindst automatisk blokering af uautoriserede besøgende. Jeg bruger Cloudflare til alle mine websteder (og næsten alle projekter bruger forskellige teknologier).
- **Logføring af fejl**. Altid og i alt. Der er stor sandsynlighed for, at din applikation indeholder en masse fejl, der er begået af din programmør. Det kan være [uncaught exceptions] (https://php.baraja.cz/vyjimky), programfejl, SQL-injektion osv. Hvis du programmerer i PHP og ikke forstår logning, er der nok problemer, som [Tracy](https://tracy.nette.org/) (og muligvis andre værktøjer som [Sentry](https://sentry.io/))) og adgangslogfiler på selve serveren vil opdage. Fejllogning er ikke en rar ting at have, det er et must. Jeg logger fejl på alle de websteder, jeg aktivt vedligeholder, og får oplysninger om nye fejl sendt til min e-mail, så jeg får besked om dem med det samme.
- **Sikker og opdateret platform**. Har du WordPress på dit websted? Ved du, hvordan man opdaterer den korrekt? Kender du alle de risici, du står over for? Når noget er gratis, er det næsten altid på bekostning af noget andet. Personligt bruger jeg [Nette framework](https://nette.org/cs/) version 3 til udvikling af applikationer, som jeg opdaterer ugentligt og aktivt leder efter sikkerhedshuller, der skal lappes. Ligesom du får din bil regelmæssigt serviceret, skal du også få dit websted serviceret regelmæssigt og mindst en gang om måneden. Vedligeholdelse af webstedet omfatter opdatering af alle eksterne biblioteker og konstant overvågning af logfiler. Hvis du bruger en gammel version af et bibliotek, er det meget sandsynligt, at en angriber kender til sårbarheden og vil forsøge at udnytte den.
Spørgsmålet er, hvornår
En lang række mennesker i mit område undervurderer sikkerheden på en utrolig måde og udsætter indhold på internettet, som er meget let at bryde og udnytte.

Faktisk begår selv store virksomheder fejl, selv i trivielle ting som [XSS](https://www.zive.cz/clanky/vyvojar-objevil-zranitelnost-v-seznamu-dokazal-mezi-vysledky-propasovat-zakazany-kod/sc-3-a-200023/default.aspx) (cross-site scripting)-angreb.

Spørgsmålet er ikke, om nogen nogensinde vil ødelægge dit websted. Spørgsmålet er, hvornår det vil ske, og om du er forberedt nok til at håndtere det. Måske har en hacker haft adgang til din server i månedsvis, og du ved det ikke (endnu).

Hvad skal du gøre, når der opstår problemer
-------------------------------

Når du finder ud af, hvad hackeren har gjort, er det som regel for sent, og hackeren har gjort alt, hvad han skulle gøre. Han har højst sandsynligt placeret mallware over hele serveren og har i det mindste delvis kontrol over maskinen.

Dette er et **kaotisk problem, og du skal handle hurtigt**.

Da dette er den sidste fase af angrebet, anbefaler jeg personligt at afbryde webserveren helt fra internettet og begynde at finde ud af, hvad der er sket. Desuden vil vi aldrig vide præcis, om serveren skjuler noget andet. Angriberen har altid et betydeligt forspring.

Hvis vi beslutter os for at lægge den sidste fungerende sikkerhedskopi tilbage på serveren, vil vi hjælpe angriberen meget. Han ved allerede, hvordan han kan bryde ind på din server, og der er intet, der forhindrer ham i at gøre det igen om et par timer. Desuden indeholder sikkerhedskopien sandsynligvis mallware eller en anden form for virus.

Selv om dette er en meget upopulær løsning blandt mine kunder, anbefaler jeg personligt altid **at du først sletter WordPress-websteder** fuldstændigt, eller andre systemer, som kunden ikke opdaterer, og som har mange sikkerhedstrusler. Hvis du f.eks. hoster 30 websteder på en server, der er mere end 2 år gamle, er du næsten sikker på, at mindst ét af dem indeholder en eller anden form for sårbarhed.

Hvis du ikke forstår problemet, er det bedre at kontakte en egnet specialist i god tid, som har et indgående kendskab til dit problem og forstår tingene i en bred sammenhæng.

**Ethisk note:**

Hvis du administrerer et websted for en kunde, der har haft en sikkerhedshændelse, er det dit ansvar at informere dem. Hvis der lækkes data (f.eks. adgangskoder, men ofte meget mere), skal du også informere slutbrugerne om, at deres adgangskode er offentligt kendt, og at de bør ændre den overalt. Hvis du bruger en langsom hashing-funktion (f.eks. **Bcrypt + salt**), gør du det betydeligt sværere for en angriber at knække adgangskoder (desværre kan [Bcrypt passwords can be cracked](https://arstechnica.com/information-technology/2015/08/cracking-all-hacked-ashley-madison-passwords-could-take-a-lifetime/)). Hvis du ønsker at modtage regelmæssig information om, hvorvidt din konto er blevet lækket, anbefaler jeg, at du registrerer dig hos [Have i been pwned?](https://haveibeenpwned.com/). Du kan få flere oplysninger om [sikkerhedsbrud] (https://m.uoou.cz/vismo/zobraz_dok.asp?id_ktg=5020&n=poruseni-zabezpeceni) på OOOO's websted.

Atomiske vaner
--------------

I de sidste seks måneder afsluttede jeg læsningen af bogen [Atomic Habits] (https://www.melvil.cz/kniha-atomove-navyky/), som var en øjenåbner. Den beskriver en proces med en gradvis forbedring på 1 % hver dag, som vil gøre meget i det lange løb.

Da jeg bliver kontaktet af virksomheder og enkeltpersoner på forskellige stadier af teknologigæld, vil jeg gerne opsummere de værste ting:

- **FTP til serverkommunikation giver ikke mening**. Det er en usikker protokol, der er adgangskodekontrolleret. Hvis du vil give andre adgang, skal de kende adgangskoden og kan gøre det samme som dig. Hvis du ønsker at fjerne hans adgang, kan du ikke gøre det, men du kan kun ændre adgangskoden. Det er bedre at skifte helt til SSH og bruge RSA-nøgler. Jeg er ofte overrasket over, at selv virksomheder løser dette problem i dag. Lav aldrig én tabel med alle adgangskoder. Og bestemt aldrig en fælles for hele holdet!
- **Kildekoden hører hjemme i Git**, dataene i databasen og det statiske indhold (billeder, PDF-filer, ...) i filer på disken. Valg af forkert datalagring forringer applikationsdesignet og sikkerheden. URL-adressen til en PDF-fil (f.eks. med en faktura) skal indeholde tilfældigt genereret indhold, som kun modtageren kender. For ekstremt følsomt indhold (f.eks. et kontoudtog) er det vigtigt at kryptere indholdet på disken og kun give adgang til det efter login.
- Ikke-offentlige dele af webstedet skal indeholde META-headeren `noindex` og `nofollow` for at undgå at blive indekseret af Google. Følsomt indhold, som dine konkurrenter ikke må få kendskab til, skal være beskyttet med adgangskode.
- Deaktiver [directory content listing] (https://www.simplified.guide/apache/disable-directory-listing) på din server. Hvis den er indstillet forkert, kan hele webstedet blive gennemsøgt af en maskine for at finde følsomme filer.
- Projekt rod! Der må aldrig være en direkte URL til konfigurationsfiler. For eksempel [Nette does it right](https://doc.nette.org/cs/3.0/quickstart/getting-started#toc-obsah-web-projectu) lige fra den første vejledning. Det samme gælder for [publicly available git repositories] (https://smitka.me/open-git/). Denne type sårbarhed er meget let at undervurdere, og konsekvenserne er ofte tragiske.
- Fejlen kan også opstå som følge af en utilsigtet udviklingsfejl. Det er meget vigtigt at foretage en kodegennemgang af et levende menneske for hver ændring. Mange fejl bliver opdaget af [PHPStan](https://github.com/phpstan/phpstan) eller lignende værktøjer, før et menneske ser koden.
- Send aldrig [adgangskoder i læsbar form pr. e-mail] (https://www.lupa.cz/clanky/reset-a-poslani-hesla-v-citelne-podobe-e-mailem-nebezpecna-praktika/), der er altid en anden måde at løse problemet på.
- Sikkerhedsproblemer i gamle versioner af biblioteker og software. Robotter gennemsøger internettet hvert sekund og angriber kendte sikkerhedshuller (SQL-injektion, XSS, CSRF, ...). Mange af de kendte huller har ældre versioner af WordPress.

Der er mange flere sårbarheder, og problemerne varierer fra projekt til projekt. Hvis du har brug for at lave en hurtig serverrevision, anbefaler jeg at bruge [Maldet](https://www.rfxn.com/projects/linux-malware-detect/) og derefter hyre en egnet specialist til at hjælpe dig med at lave en [full site audit](https://baraja.cz/audit-webu), og ikke kun af sikkerhedshensyn.

Sikkerhedsingeniører er som plastik i havet - bare for evigt. Du skal vænne dig til det.

Naturens aktions- og reaktionsprincip
-------------------------------

Du har sikkert også bemærket, at naturen altid anvender reaktionsprincippet. Det betyder, at der sker noget i et bestemt miljø, og at de omgivende organismer reagerer forskelligt på det. Denne fremgangsmåde har mange fordele, men en stor ulempe - du vil altid blive efterladt.

Som tænkende person (designer, udvikler, konsulent) har du en stor fordel, nemlig at du kan handle - dvs. at du på forhånd kender en vis del af de store risici og aktivt kan forhindre dem i at opstå. Du kan måske aldrig forhindre alle fejltagelser, men du kan i det mindste afbøde konsekvenserne eller opbygge kontrolmekanismer, der opdager problemer tidligt, så du har tid til at reagere.

De fleste angreb finder sted over en lang periode, og hvis du logger, har du normalt tid nok til at opdage problemet.
