Kalkylator i PHP: Behandling av ett matematiskt uttryck som en sträng
=====================================================================

> id: e6798758-031d-4e7c-b24e-1f77cf61558d
> slug:
> 	cs: pokrocila-kalkulacka
> 	sv: kalkylator-i-php-behandling-av-ett-matematiskt-uttryck-som-en-straeng
> 
> publicationDate: '2020-02-16 17:07:38'
> mainCategoryId: c2134b23-9b10-46b3-aa54-e3996707255e
> sourceContentHash: '63f5336bf2bbabe5312121b57e1ce34e'

Föreställ dig att du står inför uppgiften att bearbeta ett enkelt matematiskt exempel som en användare skriver in som en textsträng, t.ex. i en sökruta. Vanligtvis vill användaren utföra en enkel numerisk operation med siffror. I den här artikeln beskrivs tankeprocessen och specifika instruktioner för hur man gör det.

Naivt genomförande
-------------------

Jag har länge funderat på om ett enkelt matematiskt uttryck skulle kunna bearbetas med något trick för att göra koden så kort som möjligt... och efter många år har jag faktiskt hittat lösningen.

Se lösningen som ges **ndast som ett exempel**, eftersom den är **extremt farlig** och en oärlig användare kan lätt understryka strängen, vilket till exempel kan radera hela programmet eller stjäla databasen!

```php
// Användarförfrågan
$query = '5 + 3 * 2';

// Bearbetar uttrycket som vanlig PHP-kod
eval('$result = @(' . $query . ');');

// Lista variabler med uttryckslösning
echo $result; // skriver ut 11
```

Tricket är att <a href="/function-eval">eval()</a>-funktionen utför strängen som om den vore i en PHP-kod. Det är galet, men det fungerar. I omslaget visas inga felmeddelanden.

Hantering av mer komplexa indata
--------------------------

Förutom det faktum att **behandlingen av uttryck via eval() är extremt farlig**, erbjuder den inte heller en tillräckligt vältalig syntax för att passa alla. Om användaren gör ett enda syntaxbrott kan hela uttrycket inte behandlas.

Lösningen är därför att först **förstå** och korrigera användarens fråga enligt den formella aspekten (det kallas normalisering till den kanoniska formen) och sedan skicka och bearbeta den vidare.

Jag har tidigare programmerat [QueryNormalizer] (https://github.com/mathematicator-core/engine/blob/master/src/QueryNormalizer.php) för just denna uppgift.

Själva bearbetningen är en mycket utmanande uppgift, eftersom du måste förstå de olika kontexterna på rätt sätt. Till exempel att parenteser betecknar inbäddade block och måste utvärderas rekursivt. Till exempel kan uttrycket `5+2^(1+3/2)` inte lösas rakt av eftersom bråket först måste lösas, adderas till talet inom parentes, sedan lösas för heltalspotensen och slutligen adderas på basnivå.

För att ens kunna uppfylla detta krävande krav kan uttrycket inte längre behandlas som en vanlig sträng utan vi måste **tappa abstraktionsnivån**. I huvudsak är matematik ett slags språk som beskriver förhållandet mellan operationer och tal, eftersom vi måste hantera operatörsprioriteringar, olika betydelser, sammanhang, rekursion och till och med datatyper. Det är här som **frågetokeniseringsprocessen** kommer in i bilden.

> Jag har arbetat med problemet med matematisk tokenisering sedan 2015 och har skrivit flera olika parsers sedan dess.

Den bästa av dessa, som för närvarande driver den nya Mathematicator, är [tillgänglig som öppen källkod på GitHub] (https://github.com/mathematicator-core/tokenizer).

Poängen med tokenisering är att **parera en sträng**, dela upp den i grupper av mindre strängar av kända typer och sedan omvandla dessa till objekt (datatyper). Den konverterade matrisen av objekt konverteras sedan med **snill logik till ett binärt träd** som kan beskriva beroenden och rekursion. Detta är en mycket krävande process eftersom det finns hundratals möjliga scenarier och användarna kan vara mycket kreativa i sina sökningar.

Den största fördelen med token arrayen är att den mycket enkelt kan skickas vidare till nästa lager, som till exempel [utför själva beräkningen] (https://github.com/mathematicator-core/calculator) eller [ritar om trädet till LaTeX] (https://github.com/mathematicator-core/tokenizer/blob/master/src/TokensToLatex.php).

Användningen kan se elegant ut så här:

```php
$tokenizer = new Tokenizer(/* vissa beroenden */);

// Konvertera matematiska formler till en array av tecken:
$tokens = $tokenizer->tokenize('(5+3)*(2/(7+3))');

// Nu kan du konvertera tokens till ett mer användbart format:
$objectTokens = $tokenizer->tokensToObject($tokens);

dump($objectTokens); // Returnerar typade tokens med metadata

// Rendering till LaTeX
echo $tokenizer->tokensToLatex($objectTokens);

// Renderar till felsökningsträdet (extremt snabbt):
echo $tokenizer->renderTokensTree($objectTokens);
```

Visa förfaranden
-----------------

Ett stort antal användare kommer att uppskatta om **programmet visar proceduren** när det räknar för att visa hur det har gjort det. Detta är faktiskt användbart även för programmeraren, eftersom han åtminstone lätt kan upptäcka fel i beräkningen och korrigera algoritmen i enlighet med detta. När du kombinerar allt detta med maskininlärning baserad på automatiserade tester får du något fantastiskt.

Titta på hur `QueryNormalizer` kunde förstå din fråga, skicka data till tokenizer, den renderade frågan till LaTeX i enlighet med den och skickade sedan objektträdet till kalkylatorn som returnerade det totala resultatet.

Příklad: [5+2^(1+3/2)](https://mathematicator.com/search/5%2B2%5E%281%2B3/2%29).

Förfarandet representeras genom att kalkylatorn går igenom inmatningsträdet och utvärderar en regel i taget i enlighet med de tecken och regler som det innehåller. När en regel utvärderas placeras steginformationen i en array. Ibland kan ett steg visa sig vara felaktigt och vi måste gå tillbaka och ta en annan väg i beräkningen, men det finns en hel del magi bakom detta, som kommer att förbli dolt för tillfället och som du kan studera i genomförandet.

Slutsats
-----

Ovanstående procedur beskriver hur man på ett elegant sätt hanterar matematiska uttryck där vi har siffror, operationer och relationer med dem. Det här tillvägagångssättet kan till exempel inte ändra uttryck eller lösa ekvationer, men det tar vi upp nästa gång.

*Om du har andra idéer om hur man kan bearbeta matematik på ett effektivt sätt, hör jag gärna av dig.*
