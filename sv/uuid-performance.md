UUID och prestanda för storskaliga tillämpningar
================================================

> id: '2f072ce8-13b1-41f6-b328-2bd3b416cdd2'
> slug:
> 	cs: uuid-performance
> 	sv: uuid-och-prestanda-foer-storskaliga-tillaempningar
> 
> publicationDate: '2019-11-08 10:09:54'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: '3b6d0e37684aedc0aedd92f2654e3626'

När databasens storlek överstiger miljontals rader är det lämpligt att börja skala programmet och dela upp databasen på flera fysiska servrar.

Det största problemet med att dela upp databasen i flera delar är den efterföljande synkroniseringen om användaren begär specifika uppgifter.

Varför använda UUID och vilka är fördelarna jämfört med autoinkrement?
--------------------------------------------------------

Anta att du har en tabell med `articles`, men eftersom du har en stor webbplats finns det tiotals miljoner artiklar högre upp och du måste fysiskt dela upp dem på flera maskiner.

Om vi skulle använda ett vanligt heltal som `id` (primärnyckel) med inställningen autoincrement, skulle vi mycket snabbt upptäcka att när vi skapar poster på olika maskiner på ett decentraliserat sätt och sedan synkroniserar dem, uppstår ID-kollisioner och vi måste numrera om posterna på ett komplicerat sätt. Om vi dessutom löser upp många sessioner till andra tabeller kan detta bli en mycket komplex overhead där det är lätt att göra misstag.

I stället för en numerisk identifierare kan vi därför generera en `UUID`, som är en textsträng som genereras av en komplex algoritm som garanterar att den är unik även om den genereras oberoende av varandra på flera maskiner.

Fördelar:

- Om du har flera oberoende databaser som du sedan synkroniserar innebär användningen av ett UUID att ett ID är unikt i alla databaser, inte bara i den databas där du befinner dig och där det genererades. När de slås samman till ett enda kluster uppstår inga konflikter.
- Du kan känna till din primära nyckel innan du lägger in posten i databasen. Detta minskar antalet SQL-förfrågningar, förenklar transaktionslogiken och du kan enkelt använda den som en främmande nyckel innan postsamlingen finns.
- UUID avslöjar inte information om antalet datum och sekvenser och är säkrare att använda i webbadresser. Om jag till exempel upptäcker att jag är användare `19010018` är det lätt att gissa att det också finns användare `19010017` och andra. Attacken kallas en vektorattack.

Skapa ett nytt UUID
----------------------

UUID kan erhållas antingen genom en enkel SQL-fråga `SELECT UUID();`, men detta ökar antalet frågor till databasen och vi förlorar möjligheten att förbereda data först i bulk i programlogiken och sedan skriva dem på en gång.

Därför gillar jag att använda paketet <a href="https://github.com/ramsey/uuid">ramsey/uuid</a> som Composer har fått fram som en bra lösning. Själva UUID har flera versioner, och paketet kan på ett lekfullt sätt generera alla sorters versioner efter behov.

Det gör den lätt att använda:

```php
require 'säljare/autoload.php';

use Ramsey\Uuid\Uuid;

// Genererar UUID-objekt i version 1 (tidsbaserat)
$uuid1 = Uuid::uuid1();
echo $uuid1->toString() . "\n"; // e4eaaaf2-d142-11e1-b3e4-080027620cdd

// Genererar UUID-objekt i version 3 (namnbaserat och hashed som MD5).
$uuid3 = Uuid::uuid3(Uuid::NAMESPACE_DNS, 'php.net');
echo $uuid3->toString() . "\n"; // 11a38b9a-b3da-360f-9353-a5a725514269

// Genererar UUID-objekt i version 4 (slumpmässigt)
$uuid4 = Uuid::uuid4();
echo $uuid4->toString() . "\n"; // 25769c6c-d34d-4bfe-ba98-e0ee856f3e7a

// Genererar UUID-objekt av version 5 (namnbaserat och hashed som SHA1).
$uuid5 = Uuid::uuid5(Uuid::NAMESPACE_DNS, 'php.net');
echo $uuid5->toString() . "\n"; // c4a760a8-dbcf-5254-a0d9-6a4474bd1b62
```

Om du använder Doctrine finns det ett tillägg <a href="https://github.com/ramsey/uuid-doctrine">ramsey/uuid-doctrine</a> som genererar ID direkt som en datatyp.

Fysisk lagring i databasen
---------------------------

I mina första försök använde jag `varchar(36)` som primärnyckel (ID), men <a href="https://www.facebook.com/groups/backendisti/permalink/2465260887049808/">det är inte alls en bra idé</a>.

> **förklaring av den interna logiken:**
>
> > MySql-databaser (och många andra) kan inte använda `varchar`, `char` eller andra datatyper som uttrycker en sträng som primärnyckel på ett effektivt sätt.
> I vissa databaser finns det en datatyp `GUID` som är utformad för att lagra UUID:er direkt. Om du inte kan använda den här typen finns det en lämplig ersättning i form av `binary(16)`.

När du undersöker databasen fysiskt representeras ID:t i HEX-format (eftersom binärt format inte kan visas), istället för det fina ID:t `726c67c4-e5eb-4a4c-8fcc-031da5d6f3c6`, kommer du bara att se `726C67C4E5EB4A4C8FCC031DA5D6F3C6`, vilket ser ut som `'?kYߟKg2c;'` i INSERT-frågan.

Konvertera originaldata från `varchar(36)` till `binary(16)`.
----------------------------------------------------

Jag antar att du representerar (eller planerar att representera) det nyligen fastställda ID:t i databasen som:

```sql
`id` binary(16) NOT NULL
```

Att bara ändra datatypen fungerar dock inte, så något som:

```sql
SET FOREIGN_KEY_CHECKS=0;

ALTER TABLE article CHANGE id id BINARY(16) NOT NULL

SET FOREIGN_KEY_CHECKS=1;
```

Det finns i princip två skäl:

- Primärnyckeln och sessionen till den måste ha samma datatyp. Därför måste du ändra både datatypen för artikel-ID och till exempel i den relationella tabell som matchar artiklar med författare.
- Det binära formatet innehåller något som skiljer sig något från den ursprungliga strängen. Du måste använda en konverteringsfunktion.

Därför är den enda korrekta lösningen att säkerhetskopiera data (men det bör du göra före varje migrering ändå), förbereda en tom databas med funktionella relationer och lägga in data där igen via migrering.

Om du har genererat UUID:er på ett konstigt sätt tidigare är det bättre att välja en sekventiell metod för att få fram UUID:n och numrera om alla poster. Anledningen är att sekventiell layout gör det möjligt att ordna värdena bättre och skapa ett `btree`, vilket gör att prestandan är nästan identisk med `bigint`.

**Om du känner till ett bättre sätt att konvertera en befintlig databas från UUID lagrad som varchar till binärt format utan att behöva göra komplexa migreringar och med bevarade utländska nycklar, skulle jag vara mycket tacksam för feedback**.
