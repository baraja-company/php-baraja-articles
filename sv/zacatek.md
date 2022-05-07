PHP online-kurs för nybörjare
=============================

> id: '10ecc1cc-8d49-4882-8ecf-114ad74902df'
> slug:
> 	cs: zacatek
> 	sv: php-online-kurs-foer-nyboerjare
> 
> perex:
> 	- 'Výuka PHP online, kurz pro začátečníky. Naučíte se programovat v PHP přímo od senior vývojáře.'
> 	- 'Online PHP-tutorial, kurs för nybörjare. Lär dig programmera i PHP direkt från en erfaren utvecklare.'
> 
> publicationDate: '2020-02-09 14:27:47'
> mainCategoryId: f4a34087-1b51-4761-8128-4459dfe83d8a
> sourceContentHash: f4f8a8d74e09eb298963f7ab8ff00959

PHP är ett skriptspråk på serversidan som är utformat för moderna webbapplikationer.

PHP-språket har en mycket snabb inlärningskurva, dvs. på mycket kort tid (på några veckor) kommer du att kunna förstå de flesta av språkets principer så att du kan skapa nästan alla enkla webbprogram med formulär, användarkonton, databaser och mycket mer.

En annan fördel med PHP är att det är mycket utbrett på nästan alla servrar (för hosting) och att det ständigt utvecklas, vilket gör att du kan vara säker på att din applikation/webb kommer att köras överallt.

Hur kommer man igång?!?
------------

Se till att du har följande saker på plats innan du börjar:

- Hjärnan handlar mycket om att tänka,
- En dator (eller server) där du kan köra dina skript,
- Kunskaper i matematik eller inom något tekniskt område är användbara,
- Lämpligt studiematerial (t.ex. den här webbplatsen och <a href="https://www.php.net">den officiella handboken</a>),
- Grundläggande kunskaper i HTML och CSS,
- Det är bra om du åtminstone har grundläggande kunskaper i engelska (de flesta material är endast på engelska, t.ex. den officiella handboken och webbforum),
- Kunskap om ett annat programmeringsspråk är en fördel (mycket likt C/C++, som PHP är baserat på),
- Jag rekommenderar starkt att du har grundläggande kunskaper i HTML och CSS, eftersom det är mycket svårt att förstå PHP utan dessa kunskaper.
- Grundläggande kunskaper om programvara (varierar mellan olika system och de bästa programmen **är inte gratis**).

Grundläggande programvara
-----------------

Windows-dator:`
- Alla moderna **webbläsare** som har felsökningsläge. Själv använder jag <a href="https://www.google.com/chrome">Google Chrome</a>.
- Till att börja med räcker det med en bättre **texteditor** med syntaxmarkering. Världens bästa är förmodligen <a href="https://www.sublimetext.com">Sublime Text</a> (som erbjuder avancerat arbete med vilken text som helst i många format, arbete med flera markörer, reguljära uttryck och är ett verktyg för flera ändamål, inte bara för programmering). Tidigare använde jag det tjeckiska redigeringsprogrammet <a href="https://www.pspad.com/cz/">PSpad</a> (som jag för närvarande anser vara mycket föråldrat och otillräckligt för moderna webbplatser), vissa använder också <a href="https://www.slunecnice.cz/sw/notepad/">Notepad++</a>.
- Om du menar allvar med utveckling skulle jag hellre använda den fullständiga utvecklingsmiljön. På jobbet använder jag <a href="https://www.jetbrains.com/phpstorm/">PhpStorm</a>, som jag ser som den bästa redigeraren för att skriva kod som någonsin har kodats.
- En **webbserver** som kan hantera PHP, MySql-databas och låter dig konfigurera dina inställningar. För närvarande anser jag att <a href="https://www.apachefriends.org/download.html">Xampp</a>, som är ett färdigpaketerat paket, är det bästa valet för Windows.

Linux (särskilt webbservern):`
- Vilken webbläsare som helst, till exempel <a href="https://www.google.com/chrome">Google Chrome</a> eller Firefox.
- I Ubuntu använder jag <a href="https://www.sublimetext.com">Sublime Text</a>, båda är tillräckliga för att komma igång.
- Att installera en **webbserver** är en större utmaning jämfört med Windows. I Ubuntu finns det till exempel ett <a href="https://wiki.ubuntu.cz/servery/apache_s_mysql_a_php">Tasksel</a> program för detta, som styrs via Terminal.
- Om du installerar en Linux-server är det också värt att överväga <a href="https://www.nginx.com/resources/wiki/">Ngnix</a>.

Mac:`
- Mac är utmärkt att programmera på, den är anpassad till användaren.
- För utveckling på en MacBook Pro använder jag <a href="https://www.jetbrains.com/phpstorm/">PhpStorm</a>, som jag tycker är den bästa utvecklingsmiljön, och för att redigera vanliga textfiler använder jag <a href="https://www.sublimetext.com">Sublime Text</a>, som hanterar stora filer mycket bra.
- Jag installerade servern själv via **Terminal**, vilket kan vara en utmaning för nybörjare, men det finns ett verktyg som heter **Mamp** där du kan klicka på alla saker med musen.

Seniorrekommendationer:`

Från och med 2020 börjar det bli uppenbart att alla problem med att köra PHP och hela applikationer enkelt kan lösas med Docker-containrar. Om du lär dig att arbeta med Docker kan du spara hundratals timmar i framtiden och enkelt integrera nykomlingar i ett befintligt projekt.

Delar av serien
------------

Jag har skrivit flera artiklar om hur du kan övervinna nybörjarbarriären och komma in i grunderna för PHP:

- <a href="/introduktion">Introduktion till att lära sig PHP</a>
- <a href="/first-script">Första skriptet</a>
- <a href="/principer för skrivning av variabler</a>
- <a href="/cyklar">Cyklar</a>
- <a href="/hur man hittar ut">Hur man hittar runt i koden</a>
- <a href="/methods-sending-data">Metoder för dataöverföring</a>
- <a href="/include-fil">Inkludera (sätter ihop sidor från delar)</a>
- <a href="/villkor">Villkor och förgrening</a>
- <a href="/safe-application">safe-app</a>

Senare är dock webbutveckling redan ganska komplicerat och man behöver verkligen mycket kunskap (eller åtminstone en misstanke om att en sådan finns). Eftersom konceptet för hela språket och webbutveckling är ganska komplext har jag förberett åtminstone <a href="/kunskap">en grundläggande kunskapsöversikt</a>, som jag successivt kompletterar och skriver artiklar om.

För att utveckla komplexa program rekommenderar jag att du börjar med <a href="/oop">objektorienterad programmering</a>.

Licens
-------

Jag tillhandahåller detta material gratis via webbplatsen `php.baraja.cz`, så det får inte användas i någon annan betald kurs. Texterna kan innehålla fel och felaktigheter. Detta är inte en officiell översättning av handboken.

Jag förbehåller mig alla rättigheter till texterna (egentligen) och därför är kopiering **förbjuden**. Du får använda webbadressen till denna webbplats (länkad här) och källkoden för exemplet utan ytterligare begränsningar.

Kontakta
-------

Jag pratar gärna med dig om webbutveckling och ger dig gärna allmänna råd, men **mer komplicerat arbete betraktas som ett betalt jobb**.

- E-post: jan@barasek.com
- Personlig <a href="https://www.facebook.com/janbarasek">Facebook</a>

<a href="https://baraja.cz/kontakt">Alla kontakter</a>
