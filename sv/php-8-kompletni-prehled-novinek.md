PHP 8 är ute - fullständig översikt
===================================

> id: '8b6ce751-195f-41d2-82c6-1af4be3e86b5'
> slug:
> 	cs: php-8-kompletni-prehled-novinek
> 	sv: php-8-aer-ute---fullstaendig-oeversikt
> 
> publicationDate: '2020-11-26 11:53:54'
> mainCategoryId: '17545205-215b-4962-b910-0d67ad1e933a'
> sourceContentHash: f657145ac1d2a1109fcc2a1ff3b6e6cf

Idag, den 26 november 2020, släpptes den nya stora versionen av PHP 8 efter flera år, och den innehåller en stor mängd nya funktioner. Detta är en av de största uppdateringarna på länge och förtjänar en särskild artikel.

I den här artikeln sammanfattar vi alla de viktigaste nya funktionerna och skillnaderna i syntax och alternativ jämfört med den äldre versionen. De flesta av de nya funktionerna är bakåtkompatibla och medför beteendeförbättringar som du kommer att uppskatta.

> ** Viktig information:** PHP 8 befinner sig nu i en fas av `feature freeze`, vilket innebär att nya beteenden inte längre kan läggas till och att endast buggar rättas. Så du kan räkna med kompatibilitet och felsöka dina program fullt ut.

Unionsformer
----------

PHP i allmänhet har under de senaste åren gått från ett rent dynamiskt språk där alla variabler kan innehålla vad som helst till en strikt form där vi i förväg vet vilken datatyp som kommer att finnas i vilken variabel, parameter, argument eller egenskap. Användningen av [data-types](/datove-types) är fortfarande frivillig, men jag rekommenderar användning av stark typning och använder den själv i alla projekt.


Unionstyper uttrycker en samling av flera typer som accepterar alla argument eller egenskaper i dem.

Till exempel:

```php
function validatePsc(string|int $psc): bool
{
	// genomförande
}
```

Funktionen `validatePsc()` i variabeln `$psc` accepterar datatyperna `string` (sträng) och `int` (heltal).

I den tidigare versionen av PHP 7.4 var denna notation inte möjlig och förbigicks vanligtvis med [comment](/document-comment):

```php
/**
 * @param string|int $psc
 */
function validatePsc($psc): bool
{
	// genomförande
}
```

Denna kommentar ignoreras dock av PHP (det är trots allt en kommentar) och vi var tvungna att göra en ytterligare kontroll med ett externt verktyg som PhpStan, vilket många utvecklare ignorerade. Nu görs kontrollen direkt vid körning (när programmet körs) och kan inte kringgås.

PHP har dock känt till en viss typ av unionstyp sedan version 7, då det var möjligt att säga att huvudtypen också kunde vara `nullable`, dvs. den accepterar huvuddatatypen plus värdet `null`.

Detta skrevs på två olika sätt, med olika innebörd:

```php
function setPhone(?string $phone): void
{
	// genomförande
}

// eller

function setPhone(string $phone = null): void
{
	// genomförande
}

// eller en kombination av dessa.

function setPhone(?string $phone = null): void
{
	// genomförande
}
```

Alla poster säger att telefonen `int` (heltal) antingen är en `string` eller `null`.

- Den första notationen kräver alltid att värdet överförs
- Den andra notationen kräver inte att något värde överförs; om inget överförs är standardvärdet `null` (detta är ett valfritt argument).
- Den tredje posten är en kombination av alternativen och beter sig som den andra posten.

När vi använder unionstyper kan vi inte längre använda en notation med ett frågetecken utan måste strikt definiera datatypen `null`, till exempel:

```php
function setPhone(string|int|null $phone = null): void
{
	// genomförande
}
```

Telefonnumret måste nu vara `string`, `int` eller `null`.

Union-typer har fortfarande ett antal användningsområden som avancerade utvecklare kan läsa om i dokumentationen eller i implementeringen av specifika bibliotek.


JIT - snabbare bearbetning av skript
----------------------------------

JIT-kompilatorn (just in time) ger en betydande förbättring av prestandan för skriptkomplikationer (tolkning och förståelse). Detta beteende kan dock variera i samband med webbbegäranden.

Du kan nu se om du har JIT aktiverat i Tracy-baren i Nette-ramverket, och se [separat artikel] (https://stitcher.io/blog/php-jit) för mer information.

Vad man kan säga om kompilering i allmänhet är att PHP försöker behandla koden i förväg så att den inte behöver gå igenom en fysisk skriptfil, analysera den och tolka den när den behandlar en viss begäran. Tidigare hanterades detta via OPCache-tillägget (som servrar och värdar har som standard) och det förbättrade bearbetningshastigheten med ungefär hälften.

En allmän tumregel är att om du har ett långsamt program är det alltid bättre att välja en lämplig algoritm för att hantera en viss uppgift än att göra mikrooptimeringar i koden. Vanligtvis orsakas de stora förseningarna av väntan på databasen och dess långsamma förfrågningar, lagring av sessioner, väntan på att hårddiskutrymme ska bli ledigt och andra maskinvaruoperationer.

Nollsäker operatör (valfri kedjebildning)
-------------------------------------

I verkliga tillämpningar är det ofta nödvändigt att kontrollera att det finns ett returvärde (att det inte är noll) från en metod och sedan villkorligt anropa en annan. [ternära operatorer](/ternary-operator) är bra för detta, men de fungerar bara med ett villkor och kan inte vara inbäddade. Operatören nullsafe tillåter inbyggd häckning.

> **TIP:** Nästan samma beteende stöds redan av Latte-templating-systemet, men det åsidosätter denna typ av syntax i PHP-kod, så att du kan använda nullsafe-operatorn i äldre versioner av PHP (från och med PHP 7). En stor eloge till David för denna ändring!

Det gör den lätt att använda:

```php
$orderId = $order?->getId();
```

Variabeln `$orderId` innehåller antingen det värde som returneras av metoden `getId()` eller `null` om variabeln `$order` innehåller värdet `null` och metoden `getId()` inte kunde anropas.

Den här typen av problem kringgicks i PHP 7 med följande syntax via den ternära operatorn:

```php
$orderId = isset($order) ? $order->getId() : null;
```

Eventuellt är det ett villkor:

```php
if (isset($order)) {
	$orderId = $order->getId();
} else {
	$orderId = null;
}
```

Införandet kan skrivas längre in i samtalet. Jag tog provet från [Latte documentation] (https://latte.nette.org/cs/syntax#toc-volitelne-retezeni-s-nullsafe-operatorem), som beskriver det perfekt:

```php
$orderName = $order->item?->name;

// samma som:

$orderName = isset($order->item) ? $order->item->name : null;
```

Typiskt användningsområde är när man listar mer komplexa strukturer i en mall, till exempel i Latte ser det ut så här (exempel från dokumentationen):

```php
{$user?->address?->street}
// betyder ca ($user !== null) && ($user->address !== null) ? $user->address->street : null

{$items[2]?->count}
// ersätta ca ($items[2] !== null) ? $items[2]->count : null

{$user->getIdentity()?->name}
// ersätta ca $user->getIdentity() !== null ? $user->getIdentity()->name : null
```

I riktig kod kan det se ut så här, till exempel om vi vill ta reda på kundens land genom att läsa hans profil (och du har data i databasen som lagras snyggt via sessioner, vilket är meningen), så såg det ut så här i den gamla PHP:

```php
$country =  null;
if ($session !== null) {
    $user = $session->user;
    if ($user !== null) {
        $address = $user->getAddress();
        if ($address !== null) {
            $country = $address->country;
        }
    }
}
```

Nu kan den förkortas till en enda rad:

```php
$country = $session?->user?->getAddress()?->country;
```

Användningen av nullsafe-operatorn förhindrar också olika buggar som en oerfaren utvecklare i PHP 7 inte lätt kan upptäcka.

Till exempel kommer den här posten att generera ett dödligt fel:

```php
var_dump($invoice->getDate()->format('Y-m-d') ?? null);

// return: fatal error: uncaught Error: call to a member function format() on null (fel: anrop till en medlemsfunktion format() på null)
```

Den korrekta syntaxen är följande:

```php
var_dump($invoice->getDate()?->format('Y-m-d'));

// återger: null
```

Namngivna argument
---------------------

I den gamla goda gamla PHP-versionen var man tvungen att skriva funktionsanrop med argument genom att skicka argumenten i exakt den ordning som målfunktionen definierar. Det är inget fel med detta, men om du använder ett antal parametrar med liknande värden kan det försämra läsbarheten. Eller om vi vill skicka upp till den n:e parametern i ordningen måste alla valfria parametrar skickas före, vilket kan ha en negativ effekt på läsbarheten och kompatibiliteten i framtiden.

Tänk dig till exempel funktionen `setCookie()` i Nette, som har många argument:

```php
public function setCookie(
	string $name,
	string $value,
	$time,
	string $path = null,
	string $domain = null,
	bool $secure = null,
	bool $httpOnly = null,
	string $sameSite = null
)
```

De tre första argumenten (`$name`, `$value` och `$time`) är obligatoriska, men om vi vill skicka argumentet `$httpOnly` måste vi skicka alla de tidigare argumenten och beräkna ordningen korrekt:

```php
$http->setCookie(
	'myCookie',
	'David gillar hästar',
	'nu',
	null, // sökväg
	null, // domän
	null, // säker
	true
);
```

Vilket du helt enkelt inte vill göra om du inte måste.

Elegant skrivning ser då ut som:

```php
$http->setCookie(
    name: 'myCookie',
    value: 'David gillar hästar',
    time: 'nu',
    httpOnly: true
);
```

Denna typ av syntax kräver att namnen på argumenten i målfunktionen aldrig ändras, eftersom de fortfarande kommer att skrivas när de anropas. Utvecklarna kommer åtminstone att kunna namnge dem bättre.

Om vi bara vill använda ett av argumenten kan syntaxen kombineras och komprimeras till en enda rad:

```php
$http->setCookie('myCookie', 'David gillar hästar', 'nu', httpOnly: true);
```

De tre första argumenten skickas på samma sätt som i original, sedan skickas det valfria argumentet `httpOnly` (eftersom det är namngivet).

Egenskaper
---------

De flesta stora språk som Java och C# innehåller redan från början så kallade annotationer, vilket är en syntax som gör det möjligt att lägga till metainformation till andra språkkonstruktioner.

I PHP har denna typ av syntax länge saknats och har kringgåtts genom att använda DOC-kommentarer, vilket är en klassisk kommentar över en metod, förutom att den har två `/**` asterisker.

Dessa kommentarer ignoreras under skriptbehandlingen och särskild användarlogik måste läggas till för att tolka dem vid körning via reflektion. Du kan säkert förstå vilken inverkan detta kan ha på prestandan, plus att syntaxen i kommentarerna inte kan krävas och är mycket svår att kontrollera vid kompilering (när skriptet bearbetas innan det körs), och återigen måste du använda ytterligare verktyg utanför den normala PHP-verktygslådan för att göra detta.

För att bevara bakåtkompatibilitet tillhandahåller PHP attribut med syntax som liknar den alternativa kommentarsnotationen, vilket inte bryter mot att köra skriptet i äldre PHP.

Den ursprungliga notationen (används t.ex. för Inject-beroenden i Nette Presenter):

```php
final class HomepagePresenter extends BasePresenter
{
	/** @injekt */
	public EntityManager $entityManager;
}
```

Nu kan du ta bort kommentaren och använda det ursprungliga attributet:

```php
use App\Attributes\Inject;

final class HomepagePresenter extends BasePresenter
{
	#[Inject]
	public EntityManager $entityManager;
}
```

Det är mycket viktigt att attributet inte längre bara är en sträng i en kommentar, utan en fysisk klass som är giltig PHP-kod.

Det här är bra, eftersom du nu kan validera inmatningarna till ett attribut på ett säkert sätt, och användningen av ett attribut blir faktiskt ett anrop till dess konstruktör där annan logik kan användas. Jag ser fram emot att se detta som ett naturligt stöd för Doctrine, som använder anteckningar för allting.

Implementeringen av själva attributet kan då se ut ungefär så här:

```php
#[Attribute]
class Inject
{
    public string $value;

    public function __construct(string $value)
    {
        $this->value = $value;
    }
}
```

Strikt logik kan användas inom attribut igen, t.ex. genom att kontrollera argumentdatatyper, unionstyper och andra språkfunktioner.

Matchningsuttryck
----------------

Den nya språkkonstruktionen `match()` är en moderniserad förbättring av den gamla goda `switch()` (som jag försöker att inte använda), och har ett antal coola funktioner (som kommer att få mig att börja använda den igen).

Vi vill till exempel ändra värdet på en variabel baserat på inmatningen:

```php
$pozdrav = match(bool $formal) {
    true => 'Hej',
    false => 'Hej',
};
```

En viktig nyhet i syntaxen är att vi inte behöver använda `break` (som den gamla `switch`) och syntaxen är generellt sett mycket mer ekonomisk.

Samtidigt kan vi validera flera inmatningar samtidigt inom ett villkor (separerade med ett kommatecken) och eventuellt returnera ett standardvärde (om inget av dem uppfyller villkoren).

Detta är till exempel praktiskt när du skriver om HTTP-statuskoden till ett felmeddelande (du kommer definitivt att uppskatta det när du hanterar undantagskoder):

```php
$message = match ($statusCode) {
    200, 300 => null,
    400 => 'inte funnen',
    500 => 'serverfel',
    default => 'okänd statuskod',
};
```

Jämförelsen av värden görs strikt med hjälp av operatören `===` (switch använder endast `==`), vilket återigen visar att PHP följer den strikta designvägen. Därför kommer inmatningen `'200'` (en sträng som innehåller ett nummer) inte att accepteras i det föregående fallet.

Om du inte anger något värde för `default` och det inte finns någon matchning, uppstår ett `UnhandledMatchError`.

Den nya syntaxen gör det också möjligt att använda ett uttryck eller ett funktionsanrop för matchning (det fungerar som ett villkor). Om ett fel inträffar kan vi kasta ett undantag (eftersom `throw`-tecknet nu är ett uttryck och kan användas på detta sätt):

```php
$message = match ($statusCode) {
    200 => null,
    $this->checkServerError($statusCode) => throw new ServerError(),
    default => 'okänd statuskod',
};
```

Spridning av egenskaper till konstruktören
-------------------------------------------------

Detta är bara en syntaktisk förbättring som kommer att vara användbar för att snabbt och enkelt definiera enheter och deras egenskaper direkt i konstruktören.


Till exempel den ursprungliga enheten:

```php
final class User
{
    public string $name;

    public function __construct(
        string $name,
    ) {
        $this->name = $name;
    }
}
```

Det kan bara förkortas till:

```php
final class User
{
    public function __construct(
        public string $name
    ) {}
}
```

Egenskapen `$name` valideras mot datatypen `string` och dess värde kan läsas direkt från instansen eftersom det är en offentlig egenskap. Om du använder ett extra SmartObject i Nette (vilket jag inte rekommenderar för PHP 8) kan du också få tillgång till privata egenskaper genom att först anropa deras getter-metod, och den här syntaxen förenklar saker och ting igen.

Returtyp statisk
--------

Tidigare kunde vi använda datatypen `self` som returvärde för en metod, men den returnerar en instans av den klass där den är definierad. Datatypen `static` kan generellt sett göra detta även vid arv, och returnerar datatypen för den klass från vilken instansen exekverades, inte dess föregångare.

Till exempel:

```php
class BaseEndpoint
{
    public function getInstance(): static
    {
        return new static();
    }
}
```

Datatyp blandad
------------------

Typen `mixed` kan nu användas som ett funktions- eller metodargument. Detta innebär att metoden alltid måste ta emot någon input (och är därför ett obligatoriskt argument).

Om du kan åtminstone lite grann, använd alltid en direkt datatyp, eller åtminstone union. Mixed är endast användbart om funktionen accepterar vad som helst. I praktiken är användningen användbar, till exempel för olika dumpfunktioner som tar emot godtycklig inmatning och måste kunna visa den.

Typen `mixed` accepterar följande typer: `string`, `int`, `float`, `null`, `bool`, `array`, `callable`, `object`, `resource`.

David kommer sedan att använda den blandade typen för sin funktion:

```php
function bdump(mixed $var): mixed
{
	Tracy\Debugger::barDump($var);
	return $var;
}
```

Token throw som ett uttryck
------------------------

Token `throw` har nu blivit ett uttryck, vilket i praktiken innebär att ett undantag kan utlösas när lambdafunktionen `fn()` trunkeras eller när en ternär operator kontrolleras:

```php
$error = fn () => throw new \InvalidArgumentException('Detta ger alltid upphov till ett fel.');

$userName = $user['namn'] ?? throw new \LogicException('Användaren måste ha ett namn.');
```

Funktionen str_contains()
-----------------------

PHP har äntligen en inhemsk funktion för att kontrollera att standardsträngen innehåller en delsträng.

Till exempel:

```php
if (str_contains('Honzik gillar katter.', 'katter')) {
	echo 'Funktionen hanterar katter.';
}
```

Tidigare kontrollerades förekomsten av en delsträng av funktionen [strpos](/strpos-function):

```php
if (strpos('Honzik gillar katter.', 'katter') !== false) {
	echo 'Funktionen hanterar katter.';
}
```

Funktionerna str_starts_with() och str_ends_with()
----------------------------------------------

Ett par nya funktioner för att kontrollera om en sträng börjar eller slutar med en delsträng:

```php
str_starts_with('Honzik gillar katter.', 'Honzik'); // sant

str_ends_with('Honzik gillar katter.', 'katter.'); // sant
```

Funktion get_debug_type()
-------------------------

Förbättrar resultatet av den befintliga funktionen [gettype](/function-gettype), som endast returnerade den generiska typen för den överlämnade variabeln. Funktionen används t.ex. för att skapa ett undantag, när vi får en icke giltig inmatning och vill tala om för användaren vad han eller hon faktiskt har skickat in.

När vi anropar funktionen `gettype()` med en variabel som innehåller en instans av klassen `\App\User` returnerar funktionen `object`, så vi vet inte vilken klass det är. Den nya funktionen `get_debug_type()` returnerar namnet på klassen.

Funktionen get_resource_id()
--------------------------

Den här funktionen returnerar identifieraren för en extern resurs från en variabel.

Till exempel hanterar PHP anslutningen till en MySql-databas genom att använda en speciell datatyp `resource`, och nu är det möjligt att ta reda på vilket ID som har tilldelats den.

> **Historisk anmärkning:**
>
> Typen `resource` i PHP skapades vid en tidpunkt då PHP ännu inte visste hur man använde objekt och var tvungen att på något sätt komma på hur man skulle skicka referenser till något som liknade en `datatyp`. I framtiden kan du förvänta dig att `resource` kommer att tas bort från språket helt och hållet, så det är bäst att inte använda den här funktionen.

Tillägget ext-json är alltid tillgängligt
-------------------------------------

Tidigare kunde PHP kompileras utan stöd för json. Nu kommer json alltid att vara tillgängligt, så du kan ta bort beroendet `ext-json` från dina `composer.json`-filer och alltid veta att json kan användas.

Företräde för sammankoppling
-------------------------------------------------------

Tänk dig något i stil med:

```php
echo 'Den totala summan:' . $a + $b;
```

Görs siffertillägget först, eller läggs variabeln `$a` till strängen först och sedan läggs hela den nya strängen till `$b`?

Man skulle kunna förvänta sig att tillägget görs först, men det är ett trevligt antagande. PHP gör faktiskt något liknande:

```php
echo ('Den totala summan:' . $a) + $b;
```

PHP 8 kommer nu att bete sig förutsägbart:

```php
echo 'Den totala summan:' . ($a + $b);
```

I allmänhet är det dock alltid bättre att använda parenteser för att avgränsa ett uttryck.

Stabil ordning
---------------

Före PHP 8 gjordes strängsortering med hjälp av den så kallade instabila algoritmen, vilket innebär att PHP inte garanterade att element med samma (eller på annat sätt likvärdiga) värde inte skulle bytas ut. Den nya versionen ändrar beteendet för alla sorteringsfunktioner till stabilt, så att sorteringen alltid sker deterministiskt och du alltid får samma resultat.

Detta löser till exempel fall där vi rangordnade användarnas betyg efter relevans, men där vissa betyg hade samma poäng. Nu visas de i samma ordning varje gång du sorterar och hoppar inte över hela tiden.

Andra nya funktioner
---------------

PHP har många andra mindre nya funktioner och förbättringar. Till exempel kommer fel att kastas annorlunda (men det stör inte oss som skriver felfri kod, eller hur?).

Du kan alltid se hela listan över ändringar i den officiella dokumentationen och RFC-posten.

Vad jag saknar i den nya PHP
-----------------------

Jag skulle vilja att PHP äntligen stödde sammansatta array-typer, till exempel när en metod returnerar en array av identifierare måste vi fortfarande ange bara `getIds(): array` och något som `getIds(): int[]` skulle vara mycket bättre. Kanske kommer vi att få se detta snart och stark typkontroll kommer att vara fullständig.

Fler resurser
------------

David Grudl höll ett trevligt föredrag om de nya sakerna på Posobot. Jag rekommenderar att du tittar på inspelningen:

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/ZYkwmcCUTCg" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Jag vill tacka David för hans föreläsning, eftersom jag har hämtat en del information från den i den här artikeln. Framför allt saker om Nettes övergång till PHP 8 och andra tips om PHP bakom kulisserna.
