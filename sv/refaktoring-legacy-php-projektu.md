Refaktorisering av ett äldre PHP-projekt - hur du tar igen teknikskulden
========================================================================

> id: '8f37305a-e9dd-4b1e-9f53-04f0fca648f7'
> slug:
> 	cs: refaktoring-legacy-php-projektu
> 	sv: refaktorisering-av-ett-aeldre-php-projekt---hur-du-tar-igen-teknikskulden
> 
> publicationDate: '2021-05-11 20:45:00'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: b13673b23a86bc82a88636e4a9464b4b

När jag rådgör med kunniga och erfarna projektägare kommer jag ofta in på frågan om ett digitalt projekts långsiktiga hållbarhet. Många stora projekt som går längre än tre år av utveckling börjar bli internt föråldrade och inte längre hållbara - tänk dig ett team av utvecklare med olika nivåer av kunskap, erfarenhet och framför allt flit.

För att hålla ditt digitala projekt tekniskt sett på toppnivå behöver du:

- **kompetenta utvecklare och en projektledare** som har en bra kommunikation och där alla vet vad de gör och hur det påverkar de andra,
- **Funktionella verktyg och arbetsflöden** som hela teamet känner till och anpassar sitt arbete till andra i enlighet med detta,
- **Automatiserade uppgifter** som ger en daglig rutin som är så lätt att glömma att den är oerhört viktig.

Om du utvecklar ett stort projekt har du det helt enkelt inte lätt. Det är viktigt att göra saker rätt, men det är ännu viktigare att göra rätt saker.

Vad är refaktorisering och varför behöver du det?
---------------------------------------

Begreppet refaktorisering omfattar en uppsättning aktiviteter som ändrar utseendet och den interna logiken i ett programs kod utan att påverka den omgivande miljön. Du kan tänka på refaktoriseringsprocessen som en julstädning - allt förblir som vanligt, men det är mycket mer organiserat och onödiga saker kastas bort. När det gäller refaktorisering är det också viktigt att komma ihåg att det inte är en engångshändelse, utan en långsiktig process som du måste upprepa i regelbundna cykler. Om du inte gör det här kan du råka ut för alla möjliga konstiga misstag, ekonomiska förluster, problem med att komma in i företaget och gråtande missnöje.

Om du kan hålla projektet stabilt och uppdaterat får du stora fördelar:

- Projektet kommer att bli **säkrare**. Det släpps regelbundet patchar för buggar och säkerhetssårbarheter i de bibliotek du använder. Du måste ta itu med säkerheten, det är en av de grundläggande livsfunktionerna i ett sunt projekt.
- Koden och den interna arkitekturen blir enklare och mer genomtänkt. Du kommer att ha bättre möjligheter att hitta fel, och många fel kan till och med förebyggas.
- Med förenklad kod kan du också ta in juniora utvecklare för att <a href="https://www.youtube.com/watch?v=1wZtdIb-Ppk">måttligt minska</a> din totala budget.
- Du kanske upptäcker att du gör vissa saker för komplicerade och överkomplicerade - projektet kommer att förenklas överlag.

För att hålla sig på rätt spår i ett stort digitalt projekt måste du springa. Men du vill växa.

Hur man omarbetar på ett säkert sätt
-------------------------

Varje omarbetning är en stor satsning som kanske inte lönar sig.

Jag ser alltid till att ha en stabil miljö långt innan jag börjar omarbeta.

För stabiliteten behöver du framför allt **funktionell felloggning**. För enkla projekt räcker det med <a href="https://tracy.nette.org">Tracy</a>, som loggar fel till en HTML-fil. För mer avancerade projekt kan du använda verktyg som <a href="https://sentry.io/welcome/">Sentry</a> eller <a href="https://rollbar.com">Rollbar</a>. Personligen använder jag Tracy i alla projekt, andra verktyg beror på typen av projekt.

Ungefär en månad före refaktoriseringen börjar jag spåra fel och skapar ett kalkylblad med kända fel som jag kan fokusera på senare. Det är alltid viktigt att veta vilka fel som infördes genom refaktorisering och vilka fel som redan fanns i projektet tidigare. På så sätt kan man bättre försvara arbetets resultat inför kunden.

När jag tack vare loggen vet att jag arbetar i en relativt stabil miljö kan vi börja med det hårda arbetet. Andra rubriker innehåller beskrivningar av metoderna beroende på hur stor risk de innebär.

Fastställande av kodningsstil och kodningsstandard
-----------------------------------

De flesta projekt har inte en konsekvent kodformateringsstil. Detta är ett stort misstag. Ett välskrivet projekt ser ut som en persons projekt, där allt är skrivet på samma sätt.

Grundläggande formateringsfel korrigeras väl av verktyget <a href="https://doc.nette.org/en/3.1/code-checker">Nette Code Checker</a> som jag regelbundet kör över hela projektet.

Kodningsstil däremot omfattar hur koden är formaterad och hur enskilda språkuttryck är indragna. Standarden <a href="https://www.php-fig.org/psr/psr-12/">PSR-12</a> används ofta i PHP-världen, och många andra standarder bygger på den. Jag rekommenderar att du använder.

I vissa projekt kombineras <a href="/spaces-and-tabs">tabs och mellanslag</a> på ett olämpligt sätt. För att effektivt förhindra att du bryter mot vitrymdskonventionen (och andra fel) skapar du en fil `.editorconfig` i projektroten som talar om för din editor hur nyskriven kod ska formateras.

Själv använder jag den här konfigurationen:

```txt
root = true

[*]
charset = utf-8
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true

[*.{php,phpt}]
indent_style = tab
indent_size = 4
```

Rätta också till alla <a href="/apostrophes-and-carriage-notes">bilagebeteckningar till apostrofer</a>. Det kan göras automatiskt.

Du kan också kontrollera formateringen av din kod automatiskt, jag har förberett en <a href="https://github.com/baraja-core/sandbox/blob/0db1f41068b6cedf79500c6a8b0ac26eae94a8eb/.github/workflows/coding-style.yml">fullständigt fungerande demo på GitHub</a> för detta. Om du också behöver göra en automatisk korrigering av koden kan du göra det med samma verktyg. PhpStorm är också mycket bra på att automatiskt rätta kod direkt, även i stora mängder för hela projektet.

Hur man fixar kodningsstilen för stora projekt
----------------------------------------------

För stora projekt rekommenderar jag att du gör automatisk kodformatering av en modul i taget, eftersom detta kan skapa ett stort antal konflikter i Git. Därför är den bästa strategin att rätta så många rader som möjligt med en enda stor commit, lägga den commit direkt till master och sedan distribuera ändringen till de andra grenarna.

Om konflikterna är för stora är det vettigt att formatera koden på master först, och sedan på varje stor gren där det finns många ändringar. Väldigt många ändrade linjer upphäver varandra (gör en snabbspolning), så att du löser väldigt få konflikter, eller ibland inga konflikter.

Om du inte gör någonting kommer koden fortfarande att vara dålig. Iterativa omskrivningar under många år fungerar definitivt inte, eftersom varje sammanslagning innebär att man måste rätta dussintals rader där ingen förändring egentligen har skett, och det komplicerar i onödan både kodgranskning och potentiella ändringar.

PhpStan - statisk kodanalys och korrigering av typfel
------------------------------------------------------

Innan du påbörjar en storskalig logikrättning är det mycket viktigt att se över och rätta till **grundläggande funktioner i projektets livscykel**. Det handlar bland annat om sådana uppenbara saker som man förväntar sig av ett projekt, men som ibland ändå inte uppfylls.

Till exempel:

- Alla klasser, metoder och funktioner måste finnas
- Klassarv får inte brytas
- Klasserna måste implementera det gränssnitt eller angränsande gränssnitt som används. Samtidigt får du inte ärva `final`-klasser.
- Du får inte anropa osäkra funktioner som `eval()`, `shell_exec()`, `var_dump()` och så vidare. Och om du ringer dem ändå måste det uttryckligen anges i kommentaren.
- Du måste alltid fånga upp undantag och inte låta hela programmet krascha.

Lösningen på problemet är att installera <a href="https://phpstan.org">PhpStan</a> i projektet och åtgärda det **minst till nivå 1**. Ja, det är svårt, och ja, det är mycket arbete. Men om du inte gör det blir varje refaktorisering rysk roulette, och utvecklaren hoppas bara att skadan blir så liten som möjligt.

PhpStans grundnivå kräver inte att datatyper och andra formella saker finns överallt, vilket vi kommer att ta upp i framtiden. Å andra sidan är det inte säkert att en funktion eller metod returnerar en viss datatyp men hävdar något annat i typhint.

Rector - säker iterativ reparation
-----------------------------------

En välkänd programmeringsdikt säger att innan vi lägger till ett nytt fel i systemet (t.ex. genom att kasta ett undantag) bör vi först lägga till felet som en varning, och först därefter göra det till ett allvarligt fel om det inte visar sig på lång tid.

Det är samma sak med datatyper. När jag refaktoriserar okänd kod låter jag automatiska verktyg som <a href="https://getrector.org">Rector</a> lägga till kommentarer till koden först, vilket inte bryter sönder något, men hjälper till att klargöra vad som kommer att vara var senare. Dessa kommentarer lyssnas sedan av PhpStan, som kan användas för att säkert kontrollera att vi inte har brutit något och att det är en säker ändring.

Jag lägger i allmänhet till kommentarer till egenskaper och argument i en enda överföring, väntar sedan i kanske en månad och när allt fungerar på lång sikt tar jag till omskrivning till fasta datatyper på ställen där det inte har varit något problem på lång sikt.

Se artikeln <a href="https://tomasvotruba.com/blog/2019/07/29/how-we-completed-thousands-of-missing-var-annotations-in-a-day/">Hur vi tog bort tusentals saknade @var-annotationer på en dag</a>.

Mer om <a href="https://github.com/rectorphp/rector">Rector finns på GitHub</a>.

Uppdatering av beroenden
----------------------

Innan du börjar uppdatera beroenden av paket eller till och med PHP-versioner är det viktigt att undersöka och lära dig hur man använder <a href="/github-actions-best-ci-for-2021">automatiserade tester</a>.

Om testerna fungerar kan vi gå till verktyget <a href="/composer">Composer</a> för att utföra uppdateringen. Om du kan, ta hjälp av en <a href="https://dependabot.com">Dependabot</a>-robot som uppdaterar ditt projekts `composer.json` automatiskt och kan kontrollera kompatibilitetsändringar.

Om du har en stor mängd ändringar på gång, gör dem alltid långsamt och öka dem version för version. Uppdatera aldrig många paket samtidigt. Efter varje uppdatering ska du skanna hela projektet med PhpStan och åtgärda fel. Det är en lång process som tar flera timmar, men det är mycket som står på spel.

Vart ska vi ta vägen härnäst?
-------

Nästa steg är individuellt beroende på projektets typ och status. I allmänhet underhålls väldesignad kod som skrivits av erfarna utvecklare i storleksordning bättre än kod som köpts billigt av en junior som arbetat på en mediebyrå som har webbutveckling mer som en bisyssla, så att säga.

Vi håller tummarna! Det kommer att vara svårt, men du kommer att klara det.
