Introduktion til PHP
====================

> id: '66b7fd22-6195-4999-ad8d-b799afdc1876'
> slug:
> 	cs: uvod
> 	da: introduktion-til-php
> 
> perex:
> 	- 'Úvodní tutoriál do jazyka PHP, možnosti jazyka, informace o tvorbě webu v PHP.'
> 	- 'Introduktion til PHP-sproget, sprogindstillinger, information om webudvikling i PHP.'
> 
> publicationDate: '2019-09-29 19:25:06'
> mainCategoryId: f4a34087-1b51-4761-8128-4459dfe83d8a
> sourceContentHash: db6ba8fe6d5ca28aab62d7302c1d7527

Dette er et onlinekursus til læring af PHP, der er beregnet til almen uddannelse. Den er gratis tilgængelig på dette websted. Teksterne må ikke anvendes i et betalt kursus uden skriftlig bekræftelse. Du kan bruge de eksempler, der anvendes på hele webstedet, i din applikation uden yderligere begrænsninger. [Vilkår og betingelser] (https://baraja.cz/vseobecne-obchodni-podminky).

Opgradering fra HTML
--------------

"Opgradering"? Det lyder mere som et mediespind, og det er det også.

Der er ingen opgradering. PHP beholder alle funktionaliteterne og funktionerne i et rent HTML-dokument, men tilføjer blot nye skrive- og implementeringsmuligheder. Fedt, ikke?

Du ved allerede fra udviklingen af statiske HTML-sider, at koden består af tags, der defineres som nøgleord omsluttet af spidse parenteser (f.eks. udtrykker `<b>Hello!</b>` den fede tekst **Hello!**).

PHP indsættes i HTML-siden som `<?php` og `?>`-tags, inden for hvilke anden programlogik er skrevet. Det er vigtigt, at PHP har sin egen syntaks (de regler, som koden skrives efter), og i modsætning til HTML er PHP ikke fejltolerant.

Der vil senere blive givet specifikke eksempler på, hvordan man gør dette, og hvad de enkelte mærker betyder. Til at begynde med er det vigtigt at forstå de generelle principper for, hvordan PHP fungerer på serveren, og hvordan koden behandles.

Kommunikationsstrømmen mellem brugeren og serveren
---------------------------------------

For en almindelig HTML-side fungerer det nogenlunde sådan her:

> Brugeren sender en forespørgsel (beder om en bestemt HTML-side), serveren kigger på disken og sender præcis det, der er gemt, tilbage. Der sker ikke noget særligt, og man skal ikke forvente mere. Siderne vil bare være statiske uden mulighed for serverinteraktion.

Hvis vi tilføjer PHP, vil der imidlertid ske underværker:

> Brugeren anmoder om siden igen. Serveren åbner filen på disken, men ser, at den ikke kun indeholder ren HTML, men også særlige tags, der indikerer et PHP-script. Så den evaluerer dem først og sender derefter det, som PHP har genereret.

Evaluering af PHP-kode sker som standard hver gang siden indlæses, men i fremtiden vil du lære at cache koden (gemme den kompileret for hurtigere behandling).

Forskellen mellem behandling af PHP-scripts og C/C++
------------------------------------------

Du har måske lært at bruge sproget `C` eller `C++` i skolen. PHP er direkte baseret på syntaksen i `C` sproget, og i kernen bruges `C` sproget, så det er godt at kende nogle fællestræk og omvendt forskelle.

Det grundlæggende princip for almindelige kompilerede programmer (som kører lokalt direkte på din computer eller smartphone) er at indlæse applikationslogikken i arbejdshukommelsen, som kommunikerer direkte med operativsystemet, der modtager input fra brugeren og derefter viser programmets output. Det vigtige er, at programmet kører isoleret hele vejen fra opstart til afslutning.

PHP starter med hver anmodning om at gengive en side, genindlæser al kode og data hver gang og afslutter derefter. PHP-scripts har derfor en bogstavelig talt yuppie-levetid og eksisterer typisk kun i nogle få millisekunder.

Fordelen ved denne fremgangsmåde er et højere niveau af isolation - hvis noget går galt, bliver alt genindlæst ved næste sideindlæsning. På den anden side stiller denne fremgangsmåde større krav til ydeevnen, fordi vi f.eks. gentagne gange skal oprette forbindelse til databasen, læse filer fra disken osv.

I fremtiden vil du lære, at du kan holde PHP-scripts indlæst i arbejdshukommelsen ved at bruge udvidelsen `OP Cache`, som de fleste nye servere (fra PHP 7.1 og fremefter) har indstillet i grundkonfigurationen.

Download af udenlandske PHP-scripts
--------------------------

Et forholdsvis almindeligt spørgsmål, som vi diskuterer med studerende, er, hvordan man downloader fremmede PHP-scripts fra serveren og ser på deres kildekode. Forud for dette spørgsmål står den betragtning, at HTML-koden for en side let kan vises i en webbrowser.

Svaret er, at **PHP-scripts ikke kan downloades**. Det skyldes, at PHP-koden først evalueres på webserveren, og derefter sendes den genererede HTML-kode (eller andet output) til brugerens browser. Det er derfor kun output fra PHP-scriptet, der kan downloades, ikke selve scriptet.

Hvordan fungerer genereringen til HTML?
---------------------------------

Det fungerer ikke bogstaveligt talt sådan, du surfer på HTML-sider. PHP-scriptet skal altid være i en PHP-fil.

Indtil nu har de fleste af jer været vant til at oprette store mapper fulde af filer med filtypenavnet **.html**. Nu vil det være langt færre filer. I det ekstreme tilfælde kan det være en enkelt fil.
