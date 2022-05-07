Grundläggande filosofi för objektorienterad programmering
=========================================================

> id: c38214d9-8c92-4484-9c31-239d716f2545
> slug:
> 	cs: filosofie-oop
> 	sv: grundlaeggande-filosofi-foer-objektorienterad-programmering
> 
> publicationDate: '2020-01-02 22:21:56'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: a3841046b40a039413bacd045f275c68

Objektorienterad programmering är ett paradigm, en syn på hur man programmerar. Du kommer snart att se att OOP innebär en ganska grundläggande förenkling av alla vanliga problem och svårigheter som löses om och om igen i riktig programmering.

Den grundläggande idén
-----------------

Den grundläggande idén med objektorienterad programmering är att dela upp ett stort program (en komplex uppgift) i många små delar som vi kan lösa på ett elegant och oberoende sätt.

Om vi till exempel programmerar ett bokningssystem för flygbiljetter är det ett mycket komplext projekt som löser tusentals fall. Om vi kan dela upp all komplex logik i många lager och delar kan vi lätt förstå hela det komplexa systemet och programmera de enskilda deluppgifterna självständigt.

Praktiska fördelar med OOP och indelning av kod i objekt
------------------------------------------------

Förutom den akademiska aspekten finns det många praktiska skäl att använda OOP:

- Programmet kommer att skapa en <a href="/kapsling">kapsling</a>, vilket innebär att data alltid behandlas i ett lokalt sammanhang, vilket är få, så att vi inte behöver tänka på hela programmet på en gång.
- Vi kan dela upp ett program i hundratusentals små delar som kan utvecklas av olika personer parallellt och bara ordna ett gemensamt gränssnitt.
- Vi kan titta på programmet från olika skikt (abstrakt på hela moduler eller lokalt på specifika funktioner som löser en specifik algoritm).
- Vid felsökning av programmet (debugging) kan vi stegvis ändra programmet, utelämna eller ersätta vissa delar.
- Vi kan bättre tillämpa **designmönster** och på så sätt förebygga fel och komplexitet i koden innan de uppstår.

Personligen kan jag inte föreställa mig att team med fler än en person programmerar annorlunda.

> **Note:**
>
> Användning av objekt innebär att datorn får lite mer arbete med minne och processor, vilket minskar programmets prestanda något. I en verklig miljö spelar detta dock ingen roll, eftersom omprogrammering av programmet till objekt innebär en viss prestandaförlust (vanligen några procentenheter), men det sparar programmerare tid (vanligen tiotals till hundratals procent). Mänsklig tid är alltid mycket dyrare (och mycket begränsad) än datortid.
>
> > Glöm inte heller att OOP innebär en stor förenkling av hela programmet och gör det möjligt att slutföra stora program på rimlig tid. Ett stort antal komplexa tillämpningar skulle vara nästan omöjliga att programmera utan objekt.

Förhållandet till den verkliga världen
-------------------------

Det grundläggande målet med OOP i programvarudesign är att så långt som möjligt simulera egenskaper, beteenden och principer i den verkliga världen. Objekt i OOP representerar verkliga enheter. Detta sätt att tänka gör det möjligt för oss att bygga enorma komplexa system som kan förstås väl, lösa verkliga problem internt på samma sätt som de skulle kunna lösas utan en dator, och principerna kan förklaras för riktiga människor.

Om vi till exempel implementerar ett program för innehållshantering är det vettigt att lägga ut all intern logik i många verkliga enheter (artikel, författare, kategori) och att bygga upp sessioner inte enligt artificiellt genererade register-ID:n (som man vanligtvis gör i databaser) utan enligt verkliga relationer.

Exempel på ett konkret genomförande:

```php
class Article
{
    private Author $author;

    /** @var Category[] */
    private array $categories;

    private string $title;
}

class Author
{
    private string $name;
}

class Category
{
    private string $name;
}
```

Som vi kan se innehåller klassen `Article` inte bara tekniska parametrar, t.ex. ID för posten med författaren, utan den är också en riktig bindning till enheten Author, som har sina egna egenskaper.

För en förklaring av det specifika genomförandet och syntaxen, se <a href="/uvod-do-oop">introduktionshandledningen</a>.
