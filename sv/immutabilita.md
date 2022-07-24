Objektets oföränderlighet - ett viktigt designkoncept
=====================================================

> id: '057467db-4e3b-4e18-9ea5-dfb25feb3800'
> slug:
> 	cs: immutabilita
> 	sv: objektets-ofoeraenderlighet-ett-viktigt-designkoncept
> 
> publicationDate: '2022-07-24 15:00:00'
> mainCategoryId: ae4c1c70-11b3-433e-b1d0-e590155bb8b9
> sourceContentHash: '88c26f35883c860426b4708f0be8761f'

Oföränderlighet är ett av de viktigaste designkoncepten för att bygga stabila program. Den grundläggande principen är att när ett tillstånd väl är skrivet kan det bara läsas senare utan möjlighet att ändra det. Om vi vill ändra tillståndet måste vi skapa en ny instans och ersätta hela objektet med ett annat.

Datatyper kan därför grovt delas in i två stora kategorier:

- **Mutable** (föränderligt tillstånd inom en enda instans)
- **Immutable** (oföränderligt internt tillstånd)

Mutabla objekt kan ändras internt. Det vill säga, de tillhandahåller operationer som, när de anropas i olika kombinationer, ger olika resultat. Immutabilitet försöker förhindra detta beteende.

Definition
--------

> En klass är **immutabel** just om instansdata inte kan ändras på något sätt efter att instansen skapats.

All data fastställs alltså i konstruktören. Alla skalära datatyper är också automatiskt oföränderliga.

En stor fördel
--------------

Att utforma program med oföränderliga tillstånd ger en grundläggande fördel när det gäller säkerheten vid utförandet av operationer. Om vi vet att data inte kan ändras (muteras) senare när de väl har skrivits kan vi till exempel felsöka på ett mycket tillförlitligt sätt eller dela upp programmet i underfunktioner utan att riskera att glömma något av de mellanliggande tillstånden.

Idén om oföränderlighet motsätter sig i allmänhet principen om att lagra tillstånd i egenskaper i objekt/entiteter och beskriver snarare ett funktionellt tillvägagångssätt där data bara "flyter" genom programmet, som till exempel javascript gör.

Ur ett prestandaperspektiv kan vi automatiskt säga om oföränderliga objekt att de kan cachelagras i all oändlighet, eftersom de aldrig kommer att bli inaktuella.

Ett verkligt exempel från PHP
--------------------

Den absolut vanligaste användningen av oföränderliga objekt i PHP är objektet `DateTimeImmutable`, som när det väl har skapats endast kan anropas av formateringsmetoder. Om vi ändrar de interna inställningarna returnerar metoden en ny instans. Den här funktionen är viktig när du använder en ORM som använder det så kallade identitetsmönstret - den gör att vi till exempel kan garantera att när vi läser skapandedatumet för en order kommer det att vara detsamma överallt i programmet och den referentiella integriteten kommer inte att skadas.

Ett konkret exempel på ett föränderligt objekt:

```php
$date = new DateTime('2021-05-14');
$tomorrow = $date->modify('+1 dag');
echo $date->format('Y-m-d'); // 2021-05-15
echo $tomorrow->format('Y-m-d'); // 2021-05-15
```

Samma datum skrevs ut eftersom metoden `modify()` endast ändrade det interna tillståndet för objektet `DateTime` och returnerade samma instans. Det fanns alltså ingen så kallad **mutation av det interna tillståndet**, vilket är ett grundläggande beteende i objektorienterad programmering. Genom att uppdatera variabeln ändrades även den ursprungliga variabeln.

Och nu ett exempel på ett oföränderligt objekt:

```php
$date = new DateTimeImmutable('2021-05-14');
$tomorrow = $date->modify('+1 dag');
echo $date->format('Y-m-d'); // 2021-05-14
echo $tomorrow->format('Y-m-d'); // 2021-05-15
```

Objektet `DateTimeImmutable` är oföränderligt, vilket innebär att dess interna tillstånd aldrig ändras. En ny modifierad instans (som också är oföränderlig) lagras i variabeln efter att metoden modify()` har anropats. Om vi inte lagrar det nya värdet i variabeln kan det inte användas senare.

Det ursprungliga värdet berörs aldrig och förvaras säkert.

När ska en klass vara oföränderlig?
---------------------------

Om du inte har en mycket god anledning att göra den föränderlig, skriv alltid en klass eller funktion som oföränderlig. Detta kommer att förenkla din konstruktion i framtiden.

Mutabla klasser bör ändras så lite som möjligt. Jag rekommenderar alltid att man dokumenterar beteendet för oföränderlighet.

Den kanske enda nackdelen med oföränderlighet är att en ny instans måste skapas vid varje attributändring, vilket har en liten inverkan på prestandan. Om ditt program (som de flesta program) tenderar att visa data och ändra dem mer sällan, är denna nackdel ganska obetydlig med dagens datorers prestanda.

Vilka typer av data bör vara oföränderliga?
------------------------------------

- Alla identifierare och unika koder
- De flesta databassessioner för ManyToOne och OneToOne
- Datum, tider, kalendervärden
- Cyklade genom matriser där vi vill göra samma operation på varje element.
- Intervall, par, tripplar, ...
- Geometriska figurer, punkter, linjer, GPS-koordinater, fysiska adresser, ...
- Loggar och historiska uppgifter
- Information om bearbetade beställningar och de flesta finansiella uppgifter
- Metadata om den relaterade enheten

Vad som inte bör vara oföränderligt:

- Stora objekt med många egenskaper
- De flesta tabeller från en databas, t.ex. Doctrine-enheter.
- Successivt konstruerade föremål från mindre delar
