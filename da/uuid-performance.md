UUID og ydeevne for applikationer i stor skala
==============================================

> id: '2f072ce8-13b1-41f6-b328-2bd3b416cdd2'
> slug:
> 	cs: uuid-performance
> 	da: uuid-og-ydeevne-for-applikationer-i-stor-skala
> 
> publicationDate: '2019-11-08 10:09:54'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: '3b6d0e37684aedc0aedd92f2654e3626'

Når databasens størrelse vokser til over millioner af rækker, er det tilrådeligt at begynde at skalere programmet og opdele databasen på flere fysiske servere.

Det største problem ved at opdele en database i flere dele er den efterfølgende synkronisering, når en bruger anmoder om specifikke data.

Hvorfor bruge UUID, og hvad er fordelene ved det i forhold til autoincrement?
--------------------------------------------------------

Lad os antage, at du har en tabel med `articles`, men fordi du har et stort websted, er der flere millioner artikler, og du er nødt til fysisk at dele dem op på flere maskiner.

Hvis vi skulle bruge et almindeligt heltal som `id` (primærnøgle) med indstillingen autoincrement, ville vi meget hurtigt opdage, at når vi opretter poster på forskellige maskiner på en decentraliseret måde og derefter synkroniserer dem, opstår der ID-kollisioner, og vi er nødt til at omnummerere posterne på en kompliceret måde. Hvis vi desuden løser mange sessioner til andre tabeller, kan dette være et meget komplekst overhead, hvor det er let at begå fejl.

Derfor kan vi i stedet for en numerisk identifikator generere et `UUID`, som er en tekststreng, der genereres af en kompleks algoritme, der garanterer, at den vil være unik, selv om den genereres uafhængigt af hinanden på flere maskiner.

Fordele:

- Hvis du har flere uafhængige databaser, som du derefter synkroniserer, betyder brugen af et UUID, at et ID er unikt på tværs af alle databaser, ikke kun den database, hvor du befinder dig, og hvor det blev genereret. Når de slås sammen til en enkelt klynge, opstår der ingen konflikter.
- Du kan kende din `primary key` før du faktisk indsætter posten i databasen. Dette reducerer antallet af SQL-forespørgsler, forenkler transaktionslogikken, og du kan nemt bruge den som en fremmednøgle, før record collection eksisterer.
- UUID'et afslører ikke oplysninger om antallet af datoer og sekvenser og er mere sikkert til brug i URL'er. Hvis jeg f.eks. finder ud af, at jeg er brugeren `19010018`, er det let at gætte, at der også findes brugeren `19010017` og andre. Dette angreb kaldes et vektorangreb.

Generering af et nyt UUID
----------------------

UUID kan fås enten ved en simpel SQL-forespørgsel `SELECT UUID();`, men dette øger antallet af forespørgsler til databasen, og vi mister muligheden for at forberede dataene først i bulk i applikationslogikken og derefter skrive dem på én gang.

Derfor kan jeg godt lide at bruge <a href="https://github.com/ramsey/uuid">ramsey/uuid</a>-pakken, som fås af Composer, som en god løsning. Selve UUID'et har flere versioner, og pakken kan legevis generere alle slags versioner efter behov.

Det gør det nemt at bruge:

```php
require 'vendor/autoload.php';

use Ramsey\Uuid\Uuid;

// Genererer UUID-objekt i version 1 (tidsbaseret)
$uuid1 = Uuid::uuid1();
echo $uuid1->toString() . "\n"; // e4eaaaf2-d142-11e1-b3e4-080027620cdd

// Genererer UUID-objekt i version 3 (navnebaseret og hashed som MD5)
$uuid3 = Uuid::uuid3(Uuid::NAMESPACE_DNS, 'php.net');
echo $uuid3->toString() . "\n"; // 11a38b9a-b3da-360f-9353-a5a725514269

// Genererer UUID-objekt i version 4 (tilfældigt)
$uuid4 = Uuid::uuid4();
echo $uuid4->toString() . "\n"; // 25769c6c-d34d-4bfe-ba98-e0ee856f3e7a

// Genererer UUID-objekt i version 5 (navnebaseret og hashed som SHA1)
$uuid5 = Uuid::uuid5(Uuid::NAMESPACE_DNS, 'php.net');
echo $uuid5->toString() . "\n"; // c4a760a8-dbcf-5254-a0d9-6a4474bd1b62
```

Hvis du bruger Doctrine, er der en udvidelse <a href="https://github.com/ramsey/uuid-doctrine">ramsey/uuid-doctrine</a>, der genererer ID'et direkte som en datatype.

Fysisk lagring i databasen
---------------------------

I mine første forsøg brugte jeg `varchar(36)` som primærnøgle (ID), men <a href="https://www.facebook.com/groups/backendisti/permalink/2465260887049808/">det er slet ikke en god idé</a>.

> **Oplysning af den interne logik:**
>
> > MySql-databaser (og mange andre) kan ikke bruge `varchar`, `char` eller andre datatyper, der udtrykker en streng som primærnøgle, effektivt.
> I nogle databaser findes der en datatype `GUID`, som er designet til at gemme UUID'er direkte. Hvis du ikke kan bruge denne type, findes der en passende erstatning i form af `binary(16)`.

Når databasen undersøges fysisk, repræsenteres ID'et i HEX-format (da binært format ikke kan vises), og i stedet for det flotte ID `726c67c4-e5eb-4a4c-8fcc-031da5d6f3c6`, vil du blot se `726C67C67C4E5EB4A4C8FCC031DA5D6F3C6`, som ligner `'?kYߟKg2c;'` i INSERT-forespørgslen.

Konvertering af de oprindelige data fra `varchar(36)` til `binary(16)`
----------------------------------------------------

Jeg går ud fra, at du repræsenterer (eller planlægger at repræsentere) det nyligt indstillede ID i databasen som:

```sql
`id` binary(16) NOT NULL
```

Det virker dog ikke at ændre datatypen, så noget som:

```sql
SET FOREIGN_KEY_CHECKS=0;

ALTER TABLE article CHANGE id id BINARY(16) NOT NULL

SET FOREIGN_KEY_CHECKS=1;
```

Der er grundlæggende to grunde:

- Den primære nøgle og sessionen til den skal have samme datatype. Derfor skal du ændre både datatypen for artikel-id'et og f.eks. i den relationelle tabel, der matcher artikler til forfattere.
- Det binære format indeholder noget lidt anderledes end den oprindelige streng. Du skal bruge en konverteringsfunktion.

Derfor er den eneste rigtige løsning at tage backup af dataene (men det bør du alligevel gøre før hver migrering), forberede en tom database med funktionelle relationer og lægge dataene der igen via migrering.

Hvis du tidligere har genereret UUID'er på en mærkelig måde, er det bedre at vælge en sekventiel metode til at få UUID'et og omnummerere alle poster. Årsagen er, at sekventielt layout giver bedre mulighed for at ordne værdierne og skabe et `btree`, hvilket gør ydelsen næsten identisk med `bigint`.

**Hvis du kender en bedre måde at konvertere en eksisterende database fra UUID gemt som varchar til binært format uden at skulle udarbejde komplekse migreringer og med bevarelse af fremmednøgler, vil jeg være meget taknemmelig for feedback**.
