Introduktion till PHP
=====================

> id: '66b7fd22-6195-4999-ad8d-b799afdc1876'
> slug:
> 	cs: uvod
> 	sv: introduktion-till-php
> 
> perex:
> 	- 'Úvodní tutoriál do jazyka PHP, možnosti jazyka, informace o tvorbě webu v PHP.'
> 	- 'Introduktion till språket PHP, språkalternativ, information om webbutveckling i PHP.'
> 
> publicationDate: '2019-09-29 19:25:06'
> mainCategoryId: f4a34087-1b51-4761-8128-4459dfe83d8a
> sourceContentHash: db6ba8fe6d5ca28aab62d7302c1d7527

Detta är en onlinekurs för att lära sig PHP, avsedd för allmän utbildning. Den finns tillgänglig gratis på denna webbplats. Texterna får inte användas i någon betald kurs utan skriftlig bekräftelse. Du får använda de exempel som används på webbplatsen i din ansökan utan ytterligare begränsningar. [Villkor] (https://baraja.cz/vseobecne-obchodni-podminky).

Uppgradera från HTML
--------------

"Uppgradering"? Det låter mer som en mediespindel, och det är det också.

Det finns ingen uppgradering. PHP behåller alla funktioner och egenskaper hos ett rent HTML-dokument, men lägger bara till nya skriv- och spridningsalternativ. Bra, eller hur?

Du vet redan från utvecklingen av statiska HTML-sidor att koden består av taggar, som definieras som nyckelord inom spetsiga parenteser (till exempel, `<b>Hello!</b>` uttrycker den fetstilade texten **Hello!**).

PHP infogas i HTML-sidan i form av taggarna `<?php` och `?>`, inom vilka annan programlogik skrivs. Det är viktigt att PHP har sin egen syntax (reglerna för hur koden skrivs) och till skillnad från HTML är den inte feltolerant.

Specifika exempel på hur man gör detta och vad varje tagg betyder kommer att ges senare. Till att börja med är det viktigt att förstå de allmänna principerna för hur PHP fungerar på servern och hur koden bearbetas.

Kommunikationsflödet mellan användaren och servern.
---------------------------------------

För en vanlig HTML-sida fungerar det ungefär så här:

> Användaren skickar en begäran (ber om en viss HTML-sida), servern tittar på disken och skickar tillbaka exakt det som finns lagrat. Inget speciellt händer, och förvänta dig inget mer. Sidorna kommer bara att vara statiska, utan möjlighet till interaktion med servern.

Men om vi lägger till PHP kommer det att hända underverk:

> Användaren begär sidan igen. Servern öppnar filen på disken, men ser att den inte bara innehåller ren HTML utan även speciella taggar som visar att det är ett PHP-skript. Den utvärderar dem först och skickar sedan det som PHP har genererat.

Utvärdering av PHP-kod görs som standard varje gång sidan laddas, men i framtiden kommer du att lära dig hur du kan cachelagra koden (lagra den kompilerad för snabbare behandling).

Skillnaden mellan PHP-skriptbehandling och C/C++
------------------------------------------

Du kanske har lärt dig att använda språket `C` eller `C++` i skolan. PHP är direkt baserat på syntaxen i C-språket, och i kärnan används C-språket, så det är bra att känna till vissa gemensamma drag och omvänt skillnader.

Grundprincipen för vanliga kompilerade program (som körs lokalt direkt på din dator eller smartphone) är att ladda in programlogik i driftsminnet, som kommunicerar direkt med operativsystemet, som tar emot inmatning från användaren och sedan visar programmets utdata. Det viktiga är att programmet körs isolerat hela vägen från start till avslut.

PHP börjar med varje begäran om att göra en sida, laddar om all kod och data varje gång och avslutar sedan. PHP-skript har därför en bokstavligen yuppie-livstid och existerar vanligtvis bara i tiotals millisekunder.

Fördelen med detta tillvägagångssätt är en högre grad av isolering - om något går fel laddas allting om vid nästa sidladdning. Å andra sidan ställer detta tillvägagångssätt högre krav på prestanda eftersom vi måste ansluta till databasen upprepade gånger, läsa filer från disken och så vidare, till exempel.

I framtiden kommer du att lära dig att du kan hålla PHP-skript inlästa i operativsystemet genom att använda tillägget `OP Cache`, som de flesta nya servrar (från och med PHP 7.1) har inställt i grundkonfigurationen.

Hämta utländska PHP-skript
--------------------------

En relativt vanlig fråga som vi diskuterar med studenterna är hur man laddar ner främmande PHP-skript från servern och tittar på deras källkod. Frågan föregås av att HTML-koden för en sida lätt kan visas i en webbläsare.

Svaret är att **PHP-skript inte kan laddas ner**. Detta beror på att PHP-koden först utvärderas på webbservern och sedan skickas den genererade HTML-koden (eller annan utdata) till användarens webbläsare. Därför kan endast utdata från PHP-skriptet laddas ner, inte själva skriptet.

Hur fungerar genereringen till HTML?
---------------------------------

Det fungerar inte bokstavligen så, du surfar på HTML-sidor. PHP-skriptet måste alltid finnas i en PHP-fil.

Hittills har de flesta av er varit vana vid att skapa enorma mappar fulla av filer med ändelsen **.html**. Nu kommer det att vara mycket färre filer. I extrema fall kan det vara en enda fil.
