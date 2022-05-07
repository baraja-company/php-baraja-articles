Flerårig blankett
=================

> id: '2abacddd-8a6b-4d25-a387-603fc7abc333'
> slug:
> 	cs: vicekrokovy-formular
> 	sv: flerarig-blankett
> 
> publicationDate: '2019-09-16 09:30:19'
> mainCategoryId: a23332c0-a233-4093-abd7-85b1b00a383b
> sourceContentHash: '69cb85c856478d588b96e9db9e695d97'

Ibland måste vi dela upp formuläret i flera delar (sidor), behandla dem separat och sedan sammanställa dem till ett resultat.

I den här artikeln beskrivs metoder och designmönster för att göra detta.

> **Note:**
>
> Frågan om att dela upp ett formulär i flera steg är mycket komplex, särskilt om du vill göra det bra. Jag har stött på många metoder under min livstid, som jag kommer att diskutera här. Vissa tillvägagångssätt ser tilltalande ut, men är naiva och fungerar bara i vissa fall. För varje tillvägagångssätt beskriver jag när det är meningsfullt och vilka tillvägagångssätt som inte är meningsfulla.

Utformning av en teoretisk lösning
-------------------------

Vanligtvis är målet att hämta grunddata från det första formuläret på den första sidan, validera dem, spara dem "någonstans" och visa nästa sida.

När användaren kommer till den sista sidan måste hela formuläret skickas in och inmatningarna behandlas.

I varje steg är det viktigt att alltid noggrant validera alla uppgifter och låta användaren hoppa tillbaka genom sidorna när han eller hon vill så att han eller hon kan korrigera uppgifterna när han eller hon stöter på ett fel. Om formuläret dessutom ska återges på villkor baserat på de uppgifter som redan erhållits är detta en mycket krävande process.

Genomförande av själva blanketterna
--------------------------------

Vi kan antingen själva implementera de enskilda formulären i ren HTML och sedan hantera behandlingen i PHP eller använda färdiga lösningar som <a href="https://doc.nette.org/cs/3.0/forms">Nette forms</a>.

> **Exempel från livet:**
>
> Mycket ofta skickar nybörjare programmerare e-post till mig och ställer till synes enkla frågor som det finns en färdig lösning på. Till exempel specifikt om formulärbehandling i PHP.
>
> Jag rekommenderar alltid att man hoppar över manuell bearbetning helt och hållet och använder en färdig lösning i stället. I verkligheten är det mycket komplicerat att korrekt genomföra till exempel validering av den inmatade e-postadressen och att två lösenord i två fält matchar varandra, samtidigt som vi vill omdirigera användaren tillbaka till det förfyllda formuläret enligt hans uppgifter och med felmeddelanden i händelse av ett fel.
>
> Eftersom människor **inte vet att de inte vet att de inte vet att de inte vet** och därför, istället för att investera en timmes tid i att lära sig en färdig lösning för 99,99 % av problemen, föredrar de att välja sin egen lösning, som de spenderar dussintals timmar på att felsöka, och det finns fortfarande fall där formulär inte fungerar, ger fel, har säkerhetsbrister och inte skyddar de uppgifter som matas in.

Målet med det här steget är att skapa flera sidor på olika URL:er som kommer att innehålla tomma formulär.

Jag rekommenderar att du implementerar varje formulär oberoende av de andra (atomärt) och hanterar statusöverföringen i ett annat applikationslager. Anledningen är att varje formulär i praktiken kommer att hantera datavalidering på olika sätt, skriva sina utdata på olika sätt, hantera fel på olika sätt, och vi kommer förmodligen att vilja utöka eller ändra det med tiden, så vi behöver inte känna till sammanhanget för hela processen och ändra dussintals webbplatser för att göra det.

Statsöverföring
---------------

När vi behandlar det första formuläret vill vi först validera de mottagna uppgifterna och om de är korrekta, skicka vidare användaren till det andra steget. Det är en bra idé att hantera omdirigeringen som en HTTP-omdirigering, eftersom det lätt kan hända att uppgifterna inte är giltiga, och i så fall vill vi skicka tillbaka användaren till det första formuläret och inte till nästa steg.

Vi kan i princip lagra tillstånd på fyra olika sätt:

**Inte rekommenderat:**

- Detta har nackdelen att användaren kan ändra de uppgifter som redan skickats i URL:en en gång och på så sätt förfalska inmatningen. Alternativt kan vi avslöja känslig information som lösenord i URL:en.
- **Lägg kontinuerligt till <a href="/sessions">sessions</a>**, dvs. lägg in nyvunna data stegvis i fältet efter nyckel. Detta har den nackdelen att om programmet gör ett fel, sitter användaren fast i sessionerna och kan inte lösa felet på något sätt (förutom att radera kakorna, vilket är extremt svårt för de flesta), och med ett ofullständigt formulär finns det en risk att uppgifterna förblir förinställda och kan ses av någon annan. Det är dock mycket värre om sessionen har en mycket kort giltighetstid (till exempel 5 minuter) och användaren förlorar uppgifterna från det första steget när han eller hon fyller i det sista steget... detta kan bli ganska irriterande.

**Rekommenderat:**

- **Depositering till databasen och överlämnande av identifieraren**. Första gången formuläret skickas in lagrar vi alla insamlade data i en databastabell och genererar en slumpmässig identifierare (t.ex. en 10 tecken lång sträng) som skickas mellan sidorna som en parameter. Fördelen med detta är att när vi behandlar ett formulär kan vi skriva in de nyligen hämtade och validerade uppgifterna direkt i tabellen, och om det skulle uppstå ett fel har vi fysiska säkerhetskopior av de analyserade formulären och kan agera på dem. När en beställning är ofullständig kan vi till exempel skicka ett e-postmeddelande till användaren om att han eller hon inte har slutfört beställningen, vilket ökar chansen för en försäljning.
- **Save to currently logged in account** fungerar exakt på samma sätt som vidarebefordran via ID, förutom att vi i stället för att använda en slumpmässig identifierare (token) använder en session till ID för den inloggade användaren (om det finns en sådan). Fördelen är att vi kan visa de förfyllda uppgifterna för användaren godtyckligt i framtiden.

Slutsats
-----

Ingen av dessa lösningar är perfekt eller den enda rätta. Själv kombinerar jag flera metoder när jag arbetar med lösningar i flera steg. Vanligtvis löser jag till exempel en kundvagn som en databastabell, till vilken jag tilldelar de uppgifter jag redan har samlat in och binder den antingen till en användare (om han är inloggad) eller till en session (om han inte är inloggad och vi inte känner varandra ännu).
