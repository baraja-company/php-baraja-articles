Lære - Introduktion
===================

> id: fbd0461a-53fe-4713-8926-82e31bd1fb9b
> slug:
> 	cs: doctrine-uvod
> 	da: laere-introduktion
> 
> publicationDate: '2021-08-27 11:40:00'
> mainCategoryId: '818d311a-0f58-4df7-a9a4-da7d21489dd6'
> sourceContentHash: '2bc62308fcb66acda5a09ac95b5eb2c2'

Doctrine er et avanceret PHP-bibliotek til objektorienteret databasearbejde. Hovedformålet med Doctrine er at beskrive databaseskemaet ved hjælp af dataenheder og at manipulere dataene på en fuldt objektorienteret måde.

Dette paradigme kaldes ORM (Object-relational mapping), som er [design-pattern](/design-patterns) til konvertering (wrapping) af data, der er gemt i en relationel database, til et objekt, der kan bruges i et objektorienteret sprog. For at forstå og bruge Doctrine skal du derfor mindst kende de grundlæggende principper for [objektorienteret programmering](/oop).

Hvorfor lære doktrin?
------------------------

Der er mange grunde:

- Doctrine er den mest udbredte ORM-database, som bruges af de fleste avancerede PHP-folk.
- Det vil grundlæggende forenkle designet af din PHP-applikation
- Du giver en ensartet måde at designe, versionere, overføre og sikkerhedskopiere dit databaseskema på
- Du kan [få en masse databasetabeller ved at downloade pakken] (https://github.com/baraja-core/shop-product) uden at skulle finde ud af og konfigurere noget som helst
- Relationer mellem tabeller bliver til virkelige fysiske enheder
- Database output vil ikke være almindelige utypede arrays, men du vil få rigtige fysiske objekter
- Du får en nem måde at udføre mange operationer samtidigt i en enkelt transaktion
- Du kan nemt øge applikationssikkerheden og modstandsdygtigheden ved blot at vide, hvornår det sker, og at det sker sikkert
- Du får et kode- og databaselag, der nemt kan testes
- Du vil opdage et helt økosystem omkring Doctrine, som løser mange problemer på elegant vis. Du vil ofte finde enkle løsninger på komplekse problemer, som ellers er næsten umulige at løse på en nem måde
- Du vil lære en masse nye ting, udforske nye idéer og bruge databasen fuldt ud.
- Slip af med komplekse SQL-forespørgsler. Doctrine tilbyder en brugerdefineret grænseflade til at skrive forespørgsler (DQL), som er meget kraftfuld
- Ansøgningerne vil blive hurtigere. Du vil nemt opdage muligheder for optimering af applikationer, drage fordel af lazy loading og finde flaskehalse i applikationer

Forfatteren af denne artikel ([Jan Barasek](https://baraja.cz)) har længe været af den opfattelse, at Doctrine er den bedste måde at arbejde med en PHP-database på. Den har simpelthen ingen konkurrenter.

Hvordan kommer man i gang?
----------

Før du begynder at bruge Doctrine fuldt ud, skal du forberede et passende miljø. Hvis du lige er begyndt at arbejde med PHP eller ikke har nogen erfaring, er det bedste valg at installere Nette Framework med udvidelsespakken [Baraja Doctrine](https://github.com/baraja-core/doctrine), som automatisk integrerer fuld understøttelse. Hent først pakken via [Composer](/composer), opret derefter DI-udvidelsen, og Doctrine vil automatisk begynde at fungere.

For at Doctrine kan fungere korrekt, skal du forberede en tom database (Doctrine kan arbejde med et eksisterende projekt, men i de første trin er det uhensigtsmæssigt, da der er risiko for at overskrive eksisterende data) og konfigurere forbindelsen. Da Doctrine ikke blot er et databasebibliotek, men giver en avanceret database framework, skal du [løse anden konfiguration](/configure-connections-with-baraja-doctrine). De fleste af indstillingerne overskrives automatisk i denne pakke til Nette, men i minimumskonfigurationen skal din server dog understøtte udvidelserne `APCu Cache` eller `SQLite3`.

Hvis alt blev konfigureret korrekt, vil der blive oprettet en ny DI-tjeneste `Baraja\Doctrine\EntityManager` i Nette, som du kan [injicere](https://doc.nette.org/cs/3.1/di-usage) i Presenter:

```php
namespace App\FrontModule\Presenters;

use Baraja\Doctrine\EntityManager;

final class HomepagePresenter extends BasePresenter
{
	#[Inject]
	public EntityManager $entityManager;
}
```

Hvis det er lykkedes dig at injicere den grundlæggende EntityManager-tjeneste, kan du begynde at lære og arbejde med Doctrine.

Hvordan skal jeg fortsætte?
--------

De følgende kapitler er en kombination af en Doctrine-teknologireferenceguide, mange års erfaring, designmønstre og færdige løsninger. Sammen vil vi gennemgå alle de grundlæggende elementer i Doctrine, lige fra at definere din egen enhed til at generere et fysisk databaseskema, til at arbejde med et versioneringsværktøj og produktionsimplementering.

Jeg har brugt Doctrine i meget lang tid og har løst tusindvis af sager med det. Vi vil vise tips og tricks til, hvordan du bruger Doctrine til at optimere databasens hastighed, og hvordan du designer en database på en hensigtsmæssig måde. Du kan også bruge Doctrine til et eksisterende projekt (hvis du opfylder visse betingelser), og vi viser dig, hvordan du gør det.

Denne artikelserie blev skabt for at hjælpe mine trænings- og konsulentstuderende. Hvis du har brug for at drøfte eller forklare visse emner mere detaljeret, kan du sende mig en e-mail på jan@barasek.com. Da der er tale om en relativt krævende teknologi, vil alle spørgsmål blive behandlet som en betalt konsultation.
