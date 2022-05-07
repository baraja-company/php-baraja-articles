Lärosatserien - Introduktion
============================

> id: fbd0461a-53fe-4713-8926-82e31bd1fb9b
> slug:
> 	cs: doctrine-uvod
> 	sv: laerosatserien---introduktion
> 
> publicationDate: '2021-08-27 11:40:00'
> mainCategoryId: '818d311a-0f58-4df7-a9a4-da7d21489dd6'
> sourceContentHash: '2bc62308fcb66acda5a09ac95b5eb2c2'

Doctrine är ett avancerat PHP-bibliotek för objektorienterat databasarbete. Det huvudsakliga syftet och målet med Doctrine är att beskriva databasschemat med hjälp av dataenheter och manipulera data på ett helt objektorienterat sätt.

Detta paradigm kallas ORM (Object-relational mapping), som är [design-pattern](/design-patterns) för att omvandla data som lagras i en relationsdatabas till ett objekt som kan användas i ett objektorienterat språk. För att förstå och använda Doctrine måste du alltså kunna åtminstone grunderna i [objektorienterad programmering](/oop).

Varför lära sig doktrin?
------------------------

Det finns många orsaker till detta:

- Doctrine är den mest använda ORM-databasen och används av de flesta avancerade PHP-användare.
- Det kommer att dramatiskt förenkla utformningen av din PHP-applikation.
- Du tillhandahåller ett konsekvent sätt att utforma, versionera, överföra och säkerhetskopiera ditt databasschema.
- Du kan [få många databastabeller genom att hämta paketet] (https://github.com/baraja-core/shop-product) utan att behöva räkna ut och konfigurera något.
- Relationer mellan tabeller blir verkliga fysiska enheter
- Databasutgångarna kommer inte att vara vanliga otyperade matriser, utan du kommer att få riktiga fysiska objekt.
- Du får ett enkelt sätt att utföra många operationer samtidigt i en enda transaktion.
- Du kan enkelt öka applikationssäkerheten och motståndskraften genom att helt enkelt veta när något händer och att det sker på ett säkert sätt.
- Du får ett kod- och databaslager som är lätt att testa.
- Du kommer att upptäcka ett helt ekosystem runt Doctrine som löser många problem på ett elegant sätt. Du hittar ofta enkla lösningar på komplexa problem som annars är nästan omöjliga att lösa på ett enkelt sätt.
- Du kommer att lära dig massor av nya saker, utforska nya idéer och använda databasen fullt ut.
- Bli av med komplexa SQL-förfrågningar. Doctrine tillhandahåller ett gränssnitt för att skriva egna frågor (DQL) som är mycket kraftfullt.
- Ansökningarna kommer att bli snabbare. Du kommer lätt att upptäcka utrymme för optimering av applikationer, dra nytta av lazy loading och hitta flaskhalsar i applikationer.

Författaren till den här artikeln ([Jan Barasek](https://baraja.cz)) har länge ansett att Doctrine är det bästa sättet att arbeta med en PHP-databas. Den har helt enkelt ingen konkurrens.

Hur kommer man igång?
----------

Innan du börjar använda Doctrine fullt ut måste du förbereda en lämplig miljö. Om du precis har börjat med PHP eller inte har några större kunskaper är det bästa valet att installera Nette Framework med tilläggspaketet [Baraja Doctrine] (https://github.com/baraja-core/doctrine), som automatiskt integrerar fullt stöd. Hämta först paketet via [Composer](/composer), konfigurera sedan DI Extension och Doctrine börjar fungera automatiskt.

För att Doctrine ska fungera korrekt måste du förbereda en tom databas (Doctrine kan arbeta med ett befintligt projekt, men för de första stegen är detta olämpligt eftersom det finns risk för att befintliga data skrivs över) och konfigurera anslutningen. Eftersom Doctrine inte bara är ett databasbibliotek utan tillhandahåller ett avancerat databasramverk måste du [lösa andra konfigurationer](/configure-connections-with-baraja-doctrine). De flesta inställningar skrivs automatiskt över i paketet för Nette, men i den minsta konfigurationen måste din server ha stöd för tilläggen `APCu Cache` eller `SQLite3`.

Om allt har konfigurerats korrekt kommer en ny DI-tjänst `Baraja\Doctrine\EntityManager` att skapas i Nette, som du kan [injicera] (https://doc.nette.org/cs/3.1/di-usage) i Presenter:

```php
namespace App\FrontModule\Presenters;

use Baraja\Doctrine\EntityManager;

final class HomepagePresenter extends BasePresenter
{
	#[Inject]
	public EntityManager $entityManager;
}
```

Om du lyckas injicera den grundläggande EntityManager-tjänsten kan du börja lära dig och arbeta med Doctrine.

Hur ska man gå vidare?
--------

Följande kapitel är en kombination av en referensguide för Doctrine-teknik, många års erfarenhet, designmönster och färdiga lösningar. Tillsammans kommer vi att gå igenom alla grundläggande delar av Doctrine, från att definiera din egen enhet till att generera ett fysiskt databasschema, till att arbeta med ett verktyg för versionshantering och produktionsdistribution.

Jag har använt Doctrine under mycket lång tid och har löst tusentals fall med den. Vi kommer att visa tips och tricks om hur man använder Doctrine för att optimera databasens hastighet och hur man utformar en databas på lämpligt sätt. Du kan också använda Doctrine för ett befintligt projekt (om du uppfyller vissa villkor) och vi visar hur du gör.

Den här artikelserien skapades för att hjälpa mina utbildnings- och konsultstudenter. Om du vill diskutera eller förklara vissa frågor mer ingående kan du skicka ett e-postmeddelande till mig på jan@barasek.com. Eftersom detta är en relativt krävande teknik kommer alla frågor att behandlas som en betald konsultation.
