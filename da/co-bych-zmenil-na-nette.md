Hvad jeg ville ændre ved Nette, hvis jeg var David Grudl
========================================================

> id: bad8fe4f-92a4-4396-8698-7ec42bb2aef8
> slug:
> 	- null
> 	da: hvad-jeg-ville-aendre-ved-nette-hvis-jeg-var-david-grudl
> 
> cs: co-bych-zmenil-na-nette
> publicationDate: '2022-11-18 17:45:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '953e6c78b9dff185bbf8e3e9582d24c3'

Jeg har aktivt brugt Nette Framework i næsten 6 år til kommerciel softwareudvikling. I de første år var jeg meget begejstret, og rammen opfyldte alle de behov, vi havde som team, og der var stort set ingen grund til at lede efter et andet værktøj.

Siden 2019 er jeg begyndt at se en vis afsmitning af den oprindelige entusiasme i Nette, hvor jeg begynder at savne nogle af de avancerede funktioner. Ofte er det ting, der ligger uden for selve rammen, så jeg forventer ikke engang, at de nogensinde vil blive implementeret, men på den anden side skal en udvikler træffe en beslutning om, hvordan han eller hun vil fortsætte udviklingen fremadrettet og måske vælge en anden teknologi.

Database lag
-----------------

Jeg mener desværre, at Nette Database er en af de største fejltagelser i rammen.

Hver uge har vi samtaler i virksomheden, hvor unge, der lige er kommet ud af skolen, eller endda folk på mellemniveau, der er på vej fra en anden ramme, ansøger og opdager Nette Database som en del af deres udforskning af Nette. Som databaselag er det ikke så dårligt, men problemet er så den praktiske anvendelse i rigtige projekter.

Det skyldes, at Nette Database ikke gør nogen anstrengelser for at presse udviklerne til at bruge strenge datatyper fra starten, til at navngive kolonner og relationer, til at adskille forespørgselsmetoder (repository) fra modeller og andre små nuancer, der gør en masse samlet set.

I mange år brugte jeg Dibi-biblioteket, som spillede en stor rolle i sin tid. Det gav dig mulighed for at søge i databasen på en ganske fin måde og forenede grænsefladen. På den anden side er tiderne gået fremad, og når databaser returnerer objekter uden typeangivelse, vil PHPStan af princip ikke være tilfreds.

Personligt ville jeg enten inkorporere native understøttelse af enheder og repositories i Nette Database, eller give officiel understøttelse af Doctrine i Nette. Hvis du programmerer applikationer på niveau med en stor virksomhed, skal du være seriøs med udvikling, og Doctrine giver især god mening. Samtidig ønsker du at uddanne nye medarbejdere i bedre teknologi med det samme, og du ønsker ikke at lære dem gamle vaner.

Tracy og fejllogning
---------------------

Jeg mener stadig, at Tracy er den bedste runtime debugger til PHP.

Da jeg kom til et unavngivet digitalt bureau i 2017 og så den tekniske tilstand af deres Nette-projekter, blev jeg forfærdet. Hvor mange gange har de ikke haft websteder skrevet i ren PHP uden forsøg på at logge fejl. En ret nem løsning var at installere Tracy på projektet, kalde `Debugger::enable()` og ikke gøre mere. Fejlene blev automatisk logget i en mappe, som blot skulle hentes manuelt med få dages mellemrum.

Hvad angår Tracy, forekommer det mig, at det er et færdigt produkt, som David ikke har nogen grund til at forbedre. På den anden side ser jeg stadig plads til forbedringer i en ny retning.

Hvorfor indeholder Tracy f.eks. ikke betalte funktioner til virksomheder eller enkeltpersoner, der tager sikkerheden alvorligt?

Tracy kunne implementere en brugerdefineret webgrænseflade af Sentry-typen, hvor du opretter en konto, og produktionsfejl sendes automatisk via et API, hvor du kan spore dem på tværs af projekter. Appen kunne også anmode om individuelle websteder og automatisk registrere, at de ikke er tilgængelige, og underrette ejeren på andre måder (da Tracy ikke logisk kan registrere et servernedbrud). Jeg støder også nogle gange på det problem, at Tracy ved et uheld er aktiveret på et produktionssted, og du kan finde ud af via DIC, hvordan du får adgang til databasen eller andetsteds. Tracy bør også gøre dig opmærksom på dette.

Tracy kunne også implementere andre små ting, som jeg kender fra Node.js. For eksempel kunne det for visning af kildekode være muligt at vælge det tegninterval, hvor linjen markeres på en særlig måde, hvorfor det er muligt at fremhæve et bestemt token i en linje. Samtidig ville det være rart at tilføje understøttelse af en anden (måske blå) markeringslinje for at vise konteksten i den næste del af programmet.

Når jeg viser et uddrag af kildekoden, ville jeg ikke være bange for at tilføje en knap, der kan gengive resten af koden i ajax, så jeg kan udforske den direkte i browseren fra callstack. Det er det, som Symfony gør, og det er en fantastisk funktion. Hvis dette blev suppleret med en grundlæggende statisk analyse med mulighed for at gennemsøge metoder, klasser og arv, ville det spare hele teamet for en masse fejlfinding.

Hvis callstack gennemløber en pakke, ville det være rart at vise den version af pakken, som jeg arbejder med for en bestemt fil. Ofte får jeg en fejl i en pakke, jeg er ved at udvikle, og jeg kan lige så godt vide visuelt, at det er en ældre version.

Statisk routing
----------------

I Nette Router vil jeg tillade at definere en ny type kaldet statisk router. Den ville være en efterkommer af den grundlæggende router, men den ville kun kunne acceptere string-literal, som ikke ville være et regulært udtryk, men en lige vej. Mange sider fungerer statisk (kontakter, forskellige artikler, overraskende ofte endda forsiden), og routeren skal derfor evaluere regex-regler for match og URL-sammensætning hver gang.

Hvis der f.eks. blev brugt en statisk rute i en Latte-skabelon, kunne den sikkert oversættes til en statisk `{$basePath}/path`-streng på kompileringstidspunktet for skabelonen, så der ikke ville være behov for at kompilere de 100 URL'er, der f.eks. er i layoutet, ved hver anmodning.

Jeg forstår, at jeg kan opnå den samme funktionalitet ved at caching på skabelonsiden, men jeg ville sætte meget mere pris på en systemløsning.

I Next.js framework faldt jeg fuldstændig for konceptet statisk routing. Der findes ikke noget som `n:href`, kort sagt skal du kende URL'en på forhånd, hvilket tvinger dig til ikke at lave så mange omdirigeringer, tænke på URL-formater på forhånd og holde dem vedholdende.

PSR-grænseflade
------------

Jeg synes, det er en skam, at Nette ikke implementerer PSR-interfacet nativt. Som pakkeudvikler er du på afveje, hvis du overhovedet ønsker at definere en Composer-afhængighed af Nette. På den ene side løser Nette en masse problemer for dig, på den anden side ved du, at du simpelthen ikke vil erstatte den bagefter. Mere end én gang har jeg fundet mig selv i at foretrække at bruge Symfony, Laravel eller en helt anden generisk grænseflade på grund af manglen på en generisk grænseflade.

Den manglende understøttelse af generiske standarder med henblik på fremtidig portabilitet er hovedårsagen til, at jeg forlod Nette helt i 2022, og at vi nu udvikler nye applikationer i teams udelukkende i generiske standarder.

API-lag
----------

Hvad hvis du ønsker at bruge Nette som backend til at håndtere REST API-forespørgsler? Opbygger du en præsentation og sender svaret som `$this->sendJson()`? Vil du installere et bibliotek fra en tredjepart? Eller er du David Grudl, som løser behovet for et manglende bibliotek ved at skrive et nyt med det samme?

Hvorfor kan Nette ikke implementere noget så almindeligt som REST API lige fra starten? I dag har næsten alle applikationer brug for at kommunikere via et API - f.eks. for at levere data til frontend'en.

For nybegyndere fører dette igen til, at de bruger de indbyggede Latte og snippets i stedet for at finde ud af, at der er en bedre mulighed, nemlig at bygge hele frontend'en separat ved siden af og blot levere data til den.

Tillid til, hvad der vil ske om 10 år
-------------------------

Det sidste punkt er meget pragmatisk og stammer fra en debat, som vi fra tid til anden hører fra forretningsafdelinger i teamet.

Hvad ville der ske, hvis David Grudl helt stoppede med at udvikle Nette? Hvad hvis nogen stoppede med at udvikle det? Hvad skulle vi skifte til? Og hvor udfordrende ville det være?

Mange udviklere (som ofte ikke har nogen erfaring fra virksomheder) kan lide at spotte med denne bekymring og sige, at det er i orden. Ja, men det er det ikke.

Når du står over for spørgsmålet om, hvorvidt du skal investere tid i Nette eller React og Node.js for at uddanne juniorudviklere i din virksomhed, er det desværre sidstnævnte løsning, der vinder.

Nette har ikke misset toget endnu, men i de snesevis af interviews, jeg har haft i løbet af de sidste seks måneder, har jeg følt det ret kraftigt.
