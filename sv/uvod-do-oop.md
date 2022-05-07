Introduktion till objektorienterad programmering i PHP
======================================================

> id: '44a79461-dcfd-4dd0-b6fd-ba5db76db6de'
> slug:
> 	cs: uvod-do-oop
> 	sv: introduktion-till-objektorienterad-programmering-i-php
> 
> publicationDate: '2020-01-01 19:54:35'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '888db2cf35845892eebade9fdcc18871'

Välkommen till den första artikeln i online-kursen OOP i PHP. En fullständig förteckning över artiklarna finns på <a href="/oop">översiktssidan</a>.

> **Innehållsanteckningar:**
>
> Målet med den här serien är att **bästa förklara kärnan** av objektorienterad programmering så att du inte behöver spendera hundratals timmar på att experimentera med saker som inte är vettiga. Alla tekniker och handledningar kommer från praktiken och förklaras på samma sätt som jag själv skulle ha velat läsa dem när jag lärde mig programmera. Vissa **koncept är kraftigt förenklade** på bekostnad av 100 % saklig korrekthet, eftersom jag anser att det alltid är **bättre att förstå principerna** och åtminstone ha en viss uppfattning om problemen än att gå vilse i programmeringen och se allt som en enda stor röra.
>
> Som du snart kommer att se har OOP-programmering stora fördelar för din kod. Det ger din applikation en ny abstraktionsnivå och gör det möjligt för dig att lösa mycket komplexa problem som inte skulle vara möjliga på något annat sätt.

Vad är objekt?
------------------

I det grundläggande konceptet för programmering är objekt speciella datatyper som, förutom sitt eget värde, kan definiera detaljerade tillstånd, metoder, beteenden och göra det möjligt att utföra olika operationer, precis som i den verkliga världen.

Ett objekt inom programmering är något som liknar en verklig "sak" som vi kan namnge (t.ex. ett substantiv) och som har egenskaper som en verklig sak. Ett objekt kan till exempel vara en artikel som har vissa värden (titel, innehåll, författare, publiceringsdatum, ...) och metoder (infoga ny titel, läs innehåll, beräkna antal dagar sedan publicering, ...). I den här artikeln kommer vi främst att behandla hur man skapar och arbetar med objektet på en grundläggande nivå.

Objekt och klasser
---------------

Som vi redan har konstaterat är ett **objekt något som existerar**. Ordet "existerar" är mycket viktigt i detta skede, eftersom det, som vi snart kommer att se, fortfarande finns ett praktiskt genomförande att ta itu med i programmeringen.

Eftersom vi inom programmering skriver kod och ett objekt är något "levande" kommer vi från och med nu att skilja mellan två olika begrepp, för vilka det är viktigt att förstå innebörden i detalj och att skilja dem åt:

- Ett **objekt** är något "levande" som existerar just nu (har en instans) och som kan utföra något eller reagera på andra objekt.
- En **klass** är statiskt skriven kod som ännu inte har utvärderats och som förblir densamma hela tiden.

Eftersom vi alltid skriver kod som en statisk sträng (text) när vi programmerar, kommer vi att dela upp programmen i "klasser" (statisk definition av hela objektet) och "objekt" (en levande instans av klassen).

Exempel på en klassdefinition:

```php
class Article
{
    public string $title;

    public ?string $autor = null;
}
```

> **Praktiska anmärkningar:**
>
> I en riktig applikation skrivs varje klass vanligtvis till en separat PHP-fil som har samma namn som klassens namn.
>
> För klassen `Article` skapar vi till exempel `src/Article.php`. Denna konvention krävs inte i PHP, men den bidrar till att göra programmet mer lättläst så att du slipper ha en massa kod på samma ställe.
>
> Varje klass kan finnas högst en gång i hela programmet (har ett unikt namn), men det kan finnas (nästan) oändligt många instanser (beroende på RAM-kapaciteten). Dessutom kan en enskild klass inte delas upp i flera filer utan måste definieras på ett enda ställe på en gång. Om du behöver skapa flera klasser med samma namn använder du **namespaces**.
>
> Vi kommer också senare att visa att välstrukturerad kod gör det möjligt att återanvända den i många projekt.

Den här koden definierar en klass `Article` med två egenskaper (`title` och `author`). Kommentarer ignoreras av PHP och används endast för statisk typkontroll (vanligtvis läses de av PhpStorm).

Kod:

```php
public string $title;
```

är definitionen av `properties`, som är en egenskap för klassen `Article`. Vi kan tänka oss att `Article` är ett slags fält och `title` är dess index, som vi kan skriva in ett värde i. Varje egenskap måste ha en definierad tillgänglighet (`public`, `protected` eller `private`), vi kommer att förklara senare vad varje inställning betyder. Om du inte vet vilken tillgänglighet du ska välja i ett visst fall, skriv `public`.

Instantiera en klass och skapa ett objekt
----------------------------------

Begreppet "instans" är ett av de svåraste begreppen att förstå, så det är viktigt att du läser följande rader mycket noggrant och jag rekommenderar att du ger alla exempel ett ordentligt försök.

Om vår applikation redan har en klass definierad i källkoden används den egentligen ingenstans och PHP känner inte till den. Detta beror på att OOP introducerar "datakapslingsprincipen", vilket innebär att en klass har en intern (lokal) kontext som endast gäller inom klassen. Detta beror på att vi i objektlös utveckling var vana vid att alltid utvärdera koden i sin helhet. Försök också att tänka på en klass som någon form av funktion eller ett externt kallat program.

Nu kan vi skapa vår första instans med hjälp av den nya operatorn `new`.

```php
$myArticle = new Article;
$myArticle->title = 'Min första artikel'; // skriver värdet

echo $myArticle->title; // listar "Min första artikel"
```

Observera att vi placerar hela `Article`-objektet som skapats av `new`-operatorn i variabeln `$myArticle`. Det betyder att objektet faktiskt är en datatyp, så vi kan flytta det mellan variabler och fortsätta att arbeta med det.

Piloperatorn `->` används för att manipulera objektet. Om vi har en instans av ett visst objekt (vi har en variabel med dess innehåll) kan vi enkelt skriva värden till de interna egenskaperna, läsa dem eller ändra dem på olika sätt med hjälp av metoder (vi kommer att se senare).

**Instansen av ett objekt är skapandet av en lokal "levande" kopia av klassen i minnet (en variabel).**

Skapa flera instanser av samma klass samtidigt
---------------------------------------------

Som vi redan har diskuterat har varje objekt en lokal kontext som gäller för dess interna delar. Tack vare denna princip kan vi skapa många oberoende instanser av samma klass och manipulera dem fritt. Vi kan till och med skapa ett dynamiskt antal instanser och till exempel lagra dem som arrayelement. Möjligheterna är oändliga.

> **Varning:**
>
> När du skapar ett extremt antal instanser (hundratals eller fler) bör du tänka på minnesavtrycket, eftersom PHP måste spara information om alla instanser i RAM. I praktiken uppstår dock inte problemet med överfyllt minne på grund av många instanser.

Exempel:

```php
$firstArticle = new Article;
$firstArticle->title = 'Min första artikel';

$secondArticle = new Article;
$secondArticle->title = 'Om en hund och en katt';

echo $firstArticle->title;  // listar "Min första artikel"
echo $secondArticle->title; // skriver ut "Om en hund och en katt".
```

Observera att listningen av egenskapen `title` (`->title`) beror på vilken instans vi läser från.

Sammanfattning
-------

Vi har visat hur vi definierar vår första klass och skapar en instans av den (ett objekt) i vilken vi har skrivit in flera värden.

I nästa avsnitt förklarar vi begreppet <a href="/methods-and-passing-input">Konstruktör, metoder och ingångspassager</a>.
