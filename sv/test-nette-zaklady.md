Test av grundläggande Nette-kunskaper
=====================================

> id: d137c177-503c-4e84-8709-0e65b0ce6060
> slug:
> 	cs: test-nette-zaklady
> 	sv: test-av-grundlaeggande-nette-kunskaper
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '28a2aef7-7490-43a9-add7-80d8a051f8a9'
> sourceContentHash: '66fb122b33aa8ce41158345bb01769ab'

Tröskel för framgång: 15 poäng

*Du får 1 poäng för varje korrekt besvarad fråga. För varje felaktigt besvarad fråga får du ingenting. Om svaret bara är partiellt (och det inte är möjligt att programmera saken utifrån det) räknas frågan som felaktig (det är inte möjligt att få en halv poäng). Om lösningen innehåller ett säkerhetsfel eller ett stavfel i koden eller ett stavfel i koden anses svaret vara felaktigt eftersom det inte skulle köras.*

-----------

1 Förklara skillnaden mellan slingorna `for`, `while` och `foreach`. Ge för var och en av dem ett konkret exempel på hur den används och som tydligt visar dess främsta fördel.


2. Vi har en variabel som vi nästan inte vet någonting om (vi vet bara dess namn). Hur kan vi se dess innehåll? Till exempel heter den `$data`.


3. Skriv följande kommandon nedan för att arbeta med Git-arkivet:
- Ladda ner de senaste ändringarna från servern
- tagga filen `Statistic.php`.
- märka alla filer i projektet
- märka alla filer i katalogen `cron`.
- Överföra ändringar med meddelandet "Min överföring".
- Skicka bekräftelsen till servern.


4. Låt oss ha en textsträng i variabeln. Ge ett exempel på en funktion för att beräkna kontrollsumman.


5. Skriv ett kodutdrag som skapar en åtgärd `delete` i `Presenter` som accepterar objekt-ID som ett heltal och tar bort en rad från tabellen `question` enligt det angivna ID:et. Efter en lyckad radering skrivs meddelandet "Question deleted" (fråga raderad) ut och omdirigerar till åtgärden `list`.

Under fråga för en extrapoäng: Om raderingen misslyckas av någon anledning, visas inte ett dödligt fel, utan användaren informeras om detta med ett meddelande (flashmeddelande).

6. När jag skapar ett Nette-formulär blir det en komponent. Vad är en Nette-komponent?

7. Jag behöver skapa ett enkelt Nette-formulär för att infoga en post i en tabell `question` som innehåller en lista med frågor. Tabellen är uppbyggd på följande sätt:

| Kolumn | Egenskaper |
|-----------|----------------------------------|
| id | int(8), unsigned, automatisk ökning |
| fråga | varchar(255) |
| is_active | tinyint(1), utan förtecken, standardvärde: 1 |

Skapa lämpliga formulärfält för att infoga en ny rad i tabellen. När posten har lagts in måste ett FlashMessage skickas ut som informerar om att posten har lagts in och leder vidare till redigering av posten (åtgärden `edit`).

- Validering av att formulärfältet har fyllts i
- Validering av att frågetexten passar in i varchar enligt tabellstrukturen.
- Bekräftelse på att en fråga med denna text inte längre finns
- Definiera en annan anpassad tabell `group` som innehåller information om grupperna. När du skapar en fråga kan du sedan avgöra vilken grupp frågan tillhör. Du måste skapa en session mellan borden (beskriv hur detta går till och hur det kommer att gå till).
- Vilket Latte-makro använder jag för att göra formuläret till mallen (standardrendering)?

8. Vi har ett redigeringsformulär i `Presenter` som skapas som en komponent. Vi vill skicka in standardvärden från vad som finns i databasen, dvs. vi behöver hämta data från tabellen på ett bekvämt sätt.
- Hur kommer ni att gå tillväga och vilka delar av koden behöver vi?
- Hur ska du överföra identifieraren för det redigerade objektet till formuläret?
- Hur ställer du in standardvärdet för formulärelementet?
- Hur kan vi kontrollera att en användare försöker redigera ett objekt som inte finns i databasen och hur kan vi informera användaren om detta på lämpligt sätt?

9 Betrakta följande data som hämtas från en databas (med hjälp av en vanlig Nette-databas):

```php
$questions = $this->db->questions()->fetchAll();
```

- Hur listar vi texten till alla frågor som en punktlista?
- Hur skickar vi data från tabellen till Latte-mallen?
- Vilka Latte-makros behöver vi för att lista artiklarna? Ge en specifik implementering för att lista kolumnerna `id` och `name` i formatet:

	*1024: Hur mår du?
	*1025: Vad åt du till lunch i dag?

10. Ange ett exempel på minst 3 olika formulärfält som skrivs in i formuläret:

```php
$form->add(tady bude příklad);
```

och förklara för var och en av dem vad den används till och vad den ger för resultat (datatyp + exempel).


11. Vi har ett Nette-formulär som skickats in.
- Hur får vi alla fält (namn och värden) skickade?
- Ge ett exempel på hur ett fält med namnet `question` kan skrivas ut.
- Ge en konkret implementering av kod som går igenom arrayen av värden och nycklar och returnerar en enda sträng som innehåller den totala listan av nycklar och värden, så att vi till exempel kan lagra strängen i en databas eller skicka den via e-post (lagring och sändning är inte föremål för uppgiften och kommer inte att utvärderas).


12. För varje villkor, avgör om resultatet är VARA eller FALSKT:
- `1 > 0`
- `1 == 1`
- `1 == "1"`
- `1 === "1"`
- `1 == sant`
- `1 === sant`
- `1 === falskt`
- `1 == "1" && 1=== sant`


13. Vilka datatyper känner vi till i PHP?
- Ge minst 5 exempel på datatypnamn.
- Ange för varje exempel minst tre möjliga värden som kan lagras i datatypen (om datatypen inte kan lagra så många möjliga värden, skriv det).
- Ge för varje datatyp ett typiskt exempel på hur den används (vad lagras i den i praktiken).
- Vad är skillnaden i tillstånd mellan `==` (två likställda) och `===` (tre likställda)?
- Förklara nackdelen med att använda `==` i villkor och hur just `==` löser detta problem (exempel där `==` kan misslyckas och `==` räddar situationen).


14. Låt oss ha en samordningstabell (coordinations table) som listar alla samordningar mellan 2 personer. En av dem organiserar samordningen och den andra är gäst. Skriv ett databasval som returnerar alla rader med koordineringar som involverar mig (är jag arrangör av samordningen eller är jag gäst i samordningen). Tabellen har kolumnerna `id`, `id_user_organizer` (id för organisatör), `id_user_quest` (id för gäst). Mitt ID lagras på vanligt sätt i `Presenter`.


15. Grupp med frågor om Latte:
- Vad är Latte?
- Vad är skillnaden mellan `variabel`, `makro`, `filter` och `n:attribut`? Vad används var?
- Hur skapar jag en `DashboardPresenter`-referens till en `default`-åtgärd?
- Hur genererar jag en länk till en specifik redigering (`QuestionPresenter`, `redigera`) av en fråga för att skicka ID:t för den listade frågan? Skriv en specifik Latte-kod.

Symboliskt skrivet (exempel i PHP, översätt till Latte):

```php
foreach ($questions as $question) {
   echo $question->id; // Fråge-ID
   echo $question->question; // frågetext
}
```

- Hur skriver man ett fast HTML-utrymme?


16. Vad används tjänster i Nette?
- Hur skapas den?
- Vad måste en tjänst göra för att användas?
- Hur laddar jag en tjänst i Presenter?
- Tjänsten `StatisticManager` har till exempel en offentlig metod `getStatistics()` som inte accepterar några parametrar. Hur laddar jag den här tjänsten i Presenter och anropar den offentliga metoden `getStatistics()` i standardåtgärden och skickar resultatet till mallen?
- Vad är skillnaden mellan `object`, `class` och `service`?
- Vad är "modell", "entitet" och "värdeobjekt"?
- Alla tjänster har en huvudansvarig som känner till dem och kan hämtas från den huvudansvarige. Vad heter den här chefen? Hur registrerar jag en ny tjänst i den?


17. Neon
- Vad är Neon-filer?
- Vilka typer finns det och vad är de sorterade efter?
- Vad innehåller Neon-filer? Vilka uppgifter lagras i dem?
- Ge ett exempel på hur man registrerar följande fält (receptet är på PHP, du måste översätta det) så att det kan användas som parameter:

```php
$imageGenerator = [
   "punkter" => [
      480: [910, 30, 1845, 1150],
      600: [875, 95, 1710, 910],
      768: [975, 130, 1743, 660]
   ]
];
```

- Ge ett exempel på registrering av en tjänst i Neon, där vi skickar parametern `imageGenerator` som vi registrerade i den föregående uppgiften, så att tjänsten tar emot den i konstruktören och kan använda den i tjänsten (i betydelsen konfiguration). För tjänsten ska du ge ett exempel på en implementering av konstruktören så att den första inmatningsparametern behandlas som datatyp för matrisen.


18. Objekt i allmänhet
- Vad är `method`, `properties` och `constants`? Vad är skillnaden mellan dem?
- Både metoder och egenskaper har tre grundläggande tillgänglighetstillstånd (`public`, `private`, `protected`), förklara skillnaden och ett specifikt exempel på användning och vem som kan se vad och när.
- Jag har en klass `course` där det finns en privat egenskap `currentCourse` där den aktuella kursen lagras. Hur gör man egenskapen skrivskyddad och kan inte skriva utifrån?


19. När jag skapar tabeller i en databas som är logiskt relaterade (t.ex. en tabell för en användare och sedan en tabell för hans artiklar) måste jag se till att uppgifterna kopplas samman på rätt sätt.
- Vad säkerställer att uppgifterna i tabellerna är kopplade på rätt sätt i databasen?
- Vad är referentiell integritet och vilken roll spelar den i databasen?
- Vilka typer av sessioner har vi? Vad är syftet med varje typ?
- Vilka parametrar ställer vi in för sessioner för att hantera radering eller ändring av data på olika sätt? Ge tre exempel och specifika användningsområden + en beskrivning av hur det fungerar.


20. Vad är syftet med fabriker (OOP-designmönster)?
- Ge ett exempel på en användning.
- Är Nette tjänster fabriker?
- Vad är syftet med beroendeinjektion?
- Vad är skillnaden mellan `DI` och `DIC`?
