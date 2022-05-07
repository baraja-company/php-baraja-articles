Översikt över kunskap om webbutveckling
=======================================

> id: e5e9613c-66fe-4d5e-a686-a182aab8061a
> slug:
> 	cs: znalosti
> 	sv: oeversikt-oever-kunskap-om-webbutveckling
> 
> publicationDate: '2019-09-11 10:07:07'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: b7284b1fc11b40ddb7628995070198a1

Att veta hur man skapar en webbplats och sedan tar hand om den är inte bara en fråga om att göra den. Det finns många hinder på vägen och det är bra att åtminstone ha en grundläggande uppfattning om varje sak. När jag började visste jag inte riktigt vad jag skulle lära mig. Den här sidan fungerar som en vägvisare genom de ämnen som jag var tvungen att studera successivt för att kunna förstå webbutveckling och hantera de vanligaste situationerna.

Serveradministration
--------------

> En webbserver är en dator som kör webben. När en användare tittar på en sida skickar webbservern sidan till användaren.
>
> För närvarande (2021) är det inte längre meningsfullt att skaffa gratis webbhotell om man menar allvar med webben. En server kan hyras **från 50 kronor i månaden**.

- Serverinstallation (skiljer sig åt mellan Windows och Linux)
- Serverkonfiguration
	- Apache-inställningar
	- PHP-inställningar
	- <a href="/info">hämtning av den aktuella PHP-konfigurationen</a> (funktion `phpinfo()`)
	- Ngnix routing (förbättrar serverns prestanda)

Internet och webbläsare
--------------------------------

- Webbläsare
- Principen om begäran och svar
	- URL-adress
	- Ajax och Ajaj
	- Generering av HTML-kod (templating-system)

Strängar
-----------------

- Läsa, skriva och sammanlänka strängar, särskilt grundläggande strängarbete.
- Bearbetning av strängar
	- Bläddra tecken för tecken
	- <a href="/if">jämföra strängar</a>
	- Stringlikhet (funktionerna `levenshtein()`, `similar_text()` och `soundex()`)
	- <a href="/explode">Explode</a>, delning med separator
	- **Regulära uttryck** delar upp strängar enligt en universellt konfigurerbar mask.
	- **Tokenizer** delar upp strängar i små delar (tokens).
- Serialisering av data till en sträng
	- <a href="/json">Json</a>, ett javascriptobjekt som lagras i en sträng
