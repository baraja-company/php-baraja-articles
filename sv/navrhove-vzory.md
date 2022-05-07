Designmönster i PHP
===================

> id: c22b3e54-bb66-4196-a998-f4b62b9308b2
> slug:
> 	cs: navrhove-vzory
> 	sv: designmoenster-i-php
> 
> publicationDate: '2020-02-13 10:32:29'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '9ee32f0a3ce55bad52725f45fcd1c041'

Designmönster är ett sätt att tänka på programmering.

De erbjuder en samling råd, färdiga metoder, bästa praxis och insikter om utveckling. För varje programmeringsparadigm och uppgiftstyp finns det vissa designmönster som är bäst lämpade.

Fördelar - varför använda designmönster?
---------------------------------------

Inom programmering är det repetitivt att lösa vissa typer av problem, så det är vettigt att välja en metod för att lösa dessa problem och upprepa den metoden.

Den största fördelen uppstår särskilt under utveckling i team, när alla vet hur programmet ska utvecklas (enligt vilket designmönster) och de bara tillämpar det. På så sätt slipper man onödiga tiotals timmar av felsökning av konstigt skriven kod och försök att förstå vilka principer författaren avsåg.

MVC - ett exempel på användning av ett designmönster
--------------------------------------

Mitt favoritdesignmönster är "MVC" (från "Model View Controller"), som innebär att programmet är uppdelat i tre oberoende lager som anropar varandra sekventiellt och skickar data till varandra.

När en sida visas kan det till exempel se ut som om den först bestämmer vilken typ av sida det är (till exempel en kategoridetalj) och därför anropar `CategoryController` med metoden `detail`.

Ett konkret exempel (jag förenklar mycket):

```php
class CategoryController
{
    public CategoryManager $categoryManager;

    public function actionDetail(string $id): void
    {
        $this->template->id = $id;
        $this->template->category = $this->categoryManager->getById($id);
    }
}
```

> **Note:**
>
> Det här är bara en exempelkod som förklarar principen för designmönstret `MVC`.
>
> I ett verkligt genomförande skulle vi behöva ta reda på hur vi till exempel kan hämta en instans av `CategoryManager` och hur vi kan skicka den till egenskapen. Vanligtvis används "injektion av beroenden" för denna typ av uppgift.

Innan sidan för kategoridetaljerna visas kallas först `CategoryController` för att ta emot den faktiska begäran (dvs. vi visar kategoridetaljerna med ett visst ID, som routern hämtar, t.ex. i URL:en), hämta data (genom att fråga efter motsvarande `Model`) och skicka de slutliga uppgifterna till mallen för visning.

Den stora fördelen med denna princip är att vi kan skriva många modeller (programlogik) som är oberoende av hur data presenteras (mallen), vilket ger återanvändbar kod. Om vi vill använda `CategoryManager` i ett annat projekt skickar vi helt enkelt data genom `Controller` på ett visst sätt, som kommer att återges enligt den mall som definieras av projektet självt, medan programlogiken förblir densamma och ingen bryr sig, eftersom programvarulagret har uppfyllt sitt överenskomna gränssnitt och ansvar.

> **Praktisk anmärkning:**
>
> Designmönstret `MVC` används av de flesta moderna ramverk som Nette, Symfony, Laravel och andra.
>
> Vi kan också stöta på `MVC` i utvecklingen av mobilappar och andra typer av programvara där vi behöver hämta data och återge den i en mall enligt sid- eller visningstyp.

Viktiga designmönster för webbutveckling
---------------------------------------

Inom programmering finns det i allmänhet många designmönster som inte lämpar sig för webbutveckling. Den här listan beskriver de viktigaste mönstren som jag själv använder och som du bör känna till.

En <a href="/category-design-patterns">komplett lista över alla designmönster</a>, exempel på hur de används och detaljerade förklaringar finns på en separat sida.

- **MVC** - Principen att dela upp ett program i modell (programlogik och data), vy (mall och datavy) och kontrollant (kopplar ihop modell och vy).
- **Dependency injection** - I stället för att skapa klassinstanser definierar vi så kallade tjänster som vi automatiskt injicerar. Koden medger alla beroenden, enskilda delar kan bytas ut och vi bygger programmet dynamiskt.
- **Singleton (Singleton)** - Varje klass har bara en instans (jag använder det personligen i kombination med `Dependency injection`, där varje tjänst har bara en instans, som skickas över hela programmet).
- **Lazy Initialization (Delayed Initialization)** - Vi skapar en instans av ett objekt när det behövs för första gången.
- **Adapter** - Om vi behöver tillhandahålla kommunikation mellan två inkompatibla klasser konverterar `Adapter` data från den ena typen till den andra (vanligtvis konverteras inhemska PHP-datatyper till databastyper och tillbaka igen).
- **Command** - Istället för att utföra en specifik uppgift direkt (t.ex. skicka ett e-postmeddelande) kommer vi bara ihåg att det var meningen att det skulle ske. En annan eller samma process tar sedan upp begäran och behandlar den. Den största fördelen är att vi kan bearbeta programmet på ett lättsamt sätt (och därför mycket snabbt), försöka igen misslyckade åtgärder och helt enkelt lagra parametrarna för anropet. Samtidigt kan vi ta emot förfrågningar från olika klienter, via API:er och så vidare. Avsända åtgärder kan också ställas i kö, prioriteras och eventuellt avbrytas innan de utvärderas.
- **Strategi** - Inkapslar någon form av algoritmer eller objekt som ska ändras så att de är utbytbara för klienten.

Det finns många fler designmönster, men de här är de viktigaste du bör känna till.

Anti-mönster
------------

Vissa tekniker för programutveckling anses vara "anti-mönster", vilket är raka motsatsen till ett designmönster. Det är vanligtvis en teknik som ger upphov till konstig kod som inte är lätt att felsöka eller underhålla och som beter sig "magiskt".

Ett typiskt exempel är användningen av <a href="/global-variable">globala variabler</a>.
