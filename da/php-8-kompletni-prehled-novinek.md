PHP 8 er ude - komplet oversigt
===============================

> id: '8b6ce751-195f-41d2-82c6-1af4be3e86b5'
> slug:
> 	cs: php-8-kompletni-prehled-novinek
> 	da: php-8-er-ude-komplet-oversigt
> 
> publicationDate: '2020-11-26 11:53:54'
> mainCategoryId: '17545205-215b-4962-b910-0d67ad1e933a'
> sourceContentHash: f657145ac1d2a1109fcc2a1ff3b6e6cf

I dag, den 26. november 2020, blev den nye store version af PHP 8 udgivet efter flere år, og den indeholder en række nye funktioner. Dette er en af de største opdateringer i lang tid og fortjener en særlig artikel.

I denne artikel opsummerer vi alle de vigtigste nye funktioner og forskellene i syntaks og muligheder i forhold til den ældre version. De fleste af de nye funktioner er bagudkompatible og medfører adfærdsforbedringer, som du vil nyde godt af.

> **Vigtig information:** PHP 8 er nu i en `feature freeze`-fase, hvilket betyder at der ikke længere kan tilføjes ny adfærd, og at kun fejl rettes. Så du kan regne med kompatibilitet og fejlsøge dine applikationer fuldt ud.

Unionens typer
----------

PHP generelt har i de seneste år ændret sig fra et rent dynamisk sprog, hvor enhver variabel kunne indeholde hvad som helst, til en streng form, hvor vi på forhånd ved, hvilken datatype der vil være i hvilken variabel, parameter, argument eller egenskab. Brugen af [data-types](/datove-types) er stadig valgfri, men jeg anbefaler brugen af stærk typning og bruger det selv i alle projekter.


Union-typer udtrykker en samling af flere typer, som accepterer ethvert argument eller enhver egenskab i dem.

For eksempel:

```php
function validatePsc(string|int $psc): bool
{
	// implementering
}
```

Funktionen `validatePsc()` i variablen `$psc` accepterer datatyperne `string` (streng) og `int` (heltal).

I den tidligere version af PHP 7.4 var denne notation ikke mulig og blev typisk omgået ved hjælp af [comment](/document-comment):

```php
/**
 * @param string|int $psc
 */
function validatePsc($psc): bool
{
	// implementering
}
```

Denne annotationskommentar ignoreres imidlertid af PHP (det er trods alt en kommentar), og vi var nødt til at foretage en yderligere kontrol med et eksternt værktøj som PhpStan, hvilket mange udviklere ignorerede. Nu foretages kontrollen direkte ved kørselstid (når programmet kører) og kan ikke omgås.

PHP har dog kendt en bestemt type union-type siden version 7, hvor det var muligt at sige, at hovedtypen også kunne være `nullable`, dvs. at den accepterer hoveddatatypen plus værdien `null`.

Dette blev skrevet på to måder, hver med en anden betydning:

```php
function setPhone(?string $phone): void
{
	// implementering
}

// eller

function setPhone(string $phone = null): void
{
	// implementering
}

// eller kombination

function setPhone(?string $phone = null): void
{
	// implementering
}
```

Alle poster siger, at telefonen `int` (integer) enten er en `string` eller `null`.

- Den første notation kræver altid, at værdien
- Den anden notation kræver ikke, at der overføres nogen værdi; hvis der ikke overføres noget, er standardværdien `null` (dette er et valgfrit argument)
- Den tredje post er en kombination af mulighederne og opfører sig som den anden post

Når vi bruger unionstyper, kan vi ikke længere bruge en notation med et spørgsmålstegn, og vi skal f.eks. definere datatypen `null` strengt:

```php
function setPhone(string|int|null $phone = null): void
{
	// implementering
}
```

Telefonnummeret skal nu være `string`, `int` eller `null`.

Union-typer har stadig en række anvendelsesmuligheder, som avancerede udviklere kan læse om i dokumentationen eller implementeringen af specifikke biblioteker.


JIT - hurtigere scriptbehandling
----------------------------------

JIT-kompileren (just in time) giver en betydelig forbedring af scriptkomplikationens (parsing og forståelse) ydeevne. Denne adfærd kan dog variere i forbindelse med webanmodninger.

Du kan nu se, om du har JIT aktiveret i Tracy-linjen i Nette-rammen, og se [separat artikel](https://stitcher.io/blog/php-jit) for flere detaljer.

Hvad man kan sige om kompilering generelt er, at PHP forsøger at behandle koden på forhånd, så den ikke behøver at gennemgå den fysiske scriptfil, analysere den og fortolke den, når den behandler en bestemt forespørgsel. Tidligere blev dette håndteret via OPCache-udvidelsen (som servere og værter har som standard), og det forbedrede behandlingshastigheden med ca. halvdelen.

Som en generel tommelfingerregel gælder det, at hvis du har et langsomt program, er det altid bedre at vælge en passende algoritme til at håndtere en bestemt opgave end at foretage mikrooptimeringer i koden. De store forsinkelser skyldes normalt ventetid på databasen og dens langsomme forespørgsler, lagring af sessioner, ventetid på at der bliver plads på harddisken og andre hardwareoperationer.

Nullsafe-operator (valgfri sammenkædning)
-------------------------------------

I en rigtig applikation er det ofte nødvendigt at kontrollere, at der findes en returværdi (at den ikke er `null`) fra en metode og derefter betinget kalde en anden metode. De [ternære operatorer](/ternary-operator) er gode til dette, men de fungerer kun med én betingelse og kan ikke være indlejret i hinanden. Nullsafe-operatoren giver mulighed for indlejring af nesting.

> **TIP:** Stort set den samme opførsel understøttes allerede af Latte-templating-systemet, men det tilsidesætter denne type syntaks i den oprindelige PHP-kode, så du kan bruge nullsafe-operatoren i ældre versioner af PHP (fra og med PHP 7). Helt klart en stor ros til David for denne ændring!

Det gør det nemt at bruge:

```php
$orderId = $order?->getId();
```

Variablen `$orderId` indeholder enten den værdi, der returneres af metoden `getId()`, eller `null`, hvis variablen `$order` indeholder værdien `null`, og metoden `getId()` ikke kunne kaldes.

Denne type problem blev omgået i PHP 7 ved hjælp af følgende syntaks via ternær operatoren:

```php
$orderId = isset($order) ? $order->getId() : null;
```

Muligvis en betingelse:

```php
if (isset($order)) {
	$orderId = $order->getId();
} else {
	$orderId = null;
}
```

Indtastningen kan skrives længere inde i indkaldelsen. Jeg har taget et eksempel fra [Latte documentation] (https://latte.nette.org/cs/syntax#toc-volitelne-retezeni-s-nullsafe-operatorem), som beskriver det perfekt:

```php
$orderName = $order->item?->name;

// det samme som:

$orderName = isset($order->item) ? $order->item->name : null;
```

Typisk bruges den til at angive mere komplekse strukturer i en skabelon, f.eks. i Latte ser det således ud (eksempel fra dokumentationen):

```php
{$user?->address?->street}
// betyder ca ($user !== null) && ($user->address !== null) ? $user->address->street : null

{$items[2]?->count}
// erstatte ca ($items[2] !== null) ? $items[2]->count : null

{$user->getIdentity()?->name}
// replace ca $user->getIdentity() !== null ? $user->getIdentity()->name : null
```

I rigtig kode kan det f.eks. se sådan ud, at hvis vi ønsker at finde ud af kundens land ved at læse hans profil (og du har dataene i databasen gemt pænt via sessioner, som det er meningen), så så så det sådan ud i den gamle PHP:

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

Nu kan den forkortes til en enkelt linje:

```php
$country = $session?->user?->getAddress()?->country;
```

Brugen af nullsafe-operatoren forhindrer også forskellige fejl, som en uerfaren udvikler i PHP 7 ikke let kunne opdage.

For eksempel vil denne post generere en fatal fejl:

```php
var_dump($invoice->getDate()->format('Y-m-d') ?? null);

// return: fatal error: uncaught Fejl: kald til en medlemsfunktion format() på null
```

Den korrekte syntaks er som følger:

```php
var_dump($invoice->getDate()?->format('Y-m-d'));

// returnerer: null
```

Navngivne argumenter
---------------------

I god gammel PHP skulle funktionsopkald med argumenter skrives ved at overdrage argumenterne i den nøjagtige rækkefølge, der er defineret af målfunktionen. Det er der ikke noget galt med, men når der anvendes en række parametre med lignende værdier, kan det dog forringe læsbarheden. Eller hvis vi ønskede at videregive op til den niende parameter i rækkefølgen, skulle alle valgfrie parametre videregives før, hvilket kunne have en negativ indvirkning på læsbarheden og fremadrettet kompatibilitet.

Forestil dig f.eks. funktionen `setCookie()` i Nette, som har mange argumenter:

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

De tre første argumenter (`$name`, `$value` og `$time`) er obligatoriske, men hvis vi ønskede at videregive `$httpOnly`-argumentet, skulle vi videregive alle de foregående og beregne rækkefølgen korrekt:

```php
$http->setCookie(
	'myCookie',
	'David kan lide heste',
	'nu',
	null, // sti
	null, // domæne
	null, // sikker
	true
);
```

Hvilket du simpelthen ikke ønsker at gøre, hvis du ikke er nødt til det.

Elegant skrift ser så ud som:

```php
$http->setCookie(
    name: 'myCookie',
    value: 'David kan lide heste',
    time: 'nu',
    httpOnly: true
);
```

Denne type syntaks kræver, at navnene på argumenterne i målfunktionen aldrig ændres, fordi de stadig vil være typetypede, når de kaldes. I det mindste vil udviklerne være i stand til at give dem bedre navne.

Hvis vi kun ønsker at bruge et af argumenterne, kan syntaksen kombineres og komprimeres til kun én linje:

```php
$http->setCookie('myCookie', 'David kan lide heste', 'nu', httpOnly: true);
```

De første tre argumenter overføres på den oprindelige måde, derefter overføres det valgfrie argument `httpOnly` (fordi det er navngivet).

Attributter
---------

De fleste større sprog som Java og C# indeholder allerede indbygget såkaldte annotationer, som er en syntaks i det oprindelige sprog, der gør det muligt at tilføje metainformationer til andre sprogkonstruktioner.

I PHP har denne type syntaks længe manglet og er blevet omgået ved at bruge DOC-kommentarer, som er en klassisk kommentar over en metode, bortset fra at den har to `/**`-stjerner.

Disse kommentarer ignoreres under scriptbehandlingen, og der skal tilføjes en særlig brugerlogik for at analysere og fortolke dem ved kørselstid via refleksion. Du kan sikkert forstå, hvilken indvirkning dette kan have på ydelsen, og syntaksen i kommentarerne kan ikke kræves og er meget svær at kontrollere på compiletime (når scriptet behandles, før det køres), og igen skal du bruge yderligere værktøjer uden for den normale PHP-værktøjskasse for at gøre dette.

For at bevare bagudkompatibilitet tilbyder PHP attributter med syntaks, der ligner den alternative kommentarnotation, hvilket ikke ødelægger scriptet i ældre PHP.

Den oprindelige notation (bruges f.eks. til Inject-afhængigheder i Nette Presenter):

```php
final class HomepagePresenter extends BasePresenter
{
	/** @inject */
	public EntityManager $entityManager;
}
```

Nu kan du fjerne kommentaren og bruge den oprindelige attribut:

```php
use App\Attributes\Inject;

final class HomepagePresenter extends BasePresenter
{
	#[Inject]
	public EntityManager $entityManager;
}
```

Det er meget vigtigt, at attributten ikke længere blot er et stykke streng i en kommentar, men en fysisk klasse, som er gyldig PHP-kode.

Det er godt, fordi du nu kan validere input til en attribut på en sikker måde, og brugen af en attribut bliver faktisk et kald til dens konstruktør, hvor der kan bruges anden logik. Jeg ser frem til at se dette understøttet af Doctrine, som bruger annotationer til alting.

Implementeringen af selve attributten kan så se nogenlunde sådan ud:

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

Strenge logik kan anvendes inden for attributter igen, f.eks. ved at kontrollere argumentdatatyper, unionstyper og andre sprogfunktioner.

Match-udtryk
----------------

Den nye sprogkonstruktion `match()` er en moderniseret forbedring af den gode gamle `switch()` (som jeg forsøger at undgå at bruge) og har en række smarte funktioner (som vil få mig til at begynde at bruge den igen).

Vi ønsker f.eks. at ændre værdien af en variabel baseret på input:

```php
$pozdrav = match(bool $formal) {
    true => 'Hej',
    false => 'Hej',
};
```

En vigtig ny funktion i syntaksen er, at vi ikke behøver at bruge `break` (som den gamle `switch`), og syntaksen er generelt meget mere økonomisk.

Samtidig kan vi validere flere input på én gang inden for en betingelse (adskilt af et komma) og eventuelt returnere en standardværdi (hvis ingen af dem er opfyldt).

Det er praktisk, når du f.eks. skal omskrive HTTP-tilstandskoden til en fejlmeddelelse (du vil helt sikkert sætte pris på det, når du håndterer undtagelseskoder):

```php
$message = match ($statusCode) {
    200, 300 => null,
    400 => 'ikke fundet',
    500 => 'serverfejl',
    default => 'ukendt statuskode',
};
```

Sammenligningen af værdier sker udelukkende ved hjælp af operatoren `===` (switch bruger kun `==`), hvilket igen viser, at PHP følger den strenge designvej. Derfor vil input `'200'` (en streng, der indeholder et tal) ikke blive accepteret i det foregående tilfælde.

Hvis du ikke angiver en værdi for `default`, og der ikke er noget match, opstår der en `UnhandledMatchError`.

Den nye syntaks gør det også muligt at bruge et udtryk eller et funktionskald til at matche (det opfører sig som en betingelse). I tilfælde af en fejl kan vi så kaste en undtagelse (da `throw`-tokenet nu er et udtryk og kan bruges på denne måde):

```php
$message = match ($statusCode) {
    200 => null,
    $this->checkServerError($statusCode) => throw new ServerError(),
    default => 'ukendt statuskode',
};
```

Overførsel af egenskaber til konstruktøren
-------------------------------------------------

Dette er blot et syntaktisk sukkerstof, som vil være nyttigt til hurtig og nem definition af en enhed og dens egenskaber direkte i konstruktøren.


For eksempel den oprindelige enhed:

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

Det kan kun forkortes til:

```php
final class User
{
    public function __construct(
        public string $name
    ) {}
}
```

Egenskaben `$name` valideres i forhold til datatypen `string`, og dens værdi kan læses direkte fra instansen, fordi det er en offentlig egenskab. Hvis du bruger SmartObject i Nette (hvilket jeg ikke anbefaler til PHP 8), kan du desuden få adgang til private egenskaber ved at kalde deres getter-metode først, og denne syntaks forenkler tingene igen.

Returtype statisk
--------

Tidligere kunne vi bruge datatypen `self` som returværdi for en metode, men den returnerer en instans af den klasse, hvor den er defineret. Datatypen `static` kan generelt gøre dette selv i tilfælde af arv og returnerer datatypen for den klasse, hvorfra instansen blev udført, og ikke dens forfader.

For eksempel:

```php
class BaseEndpoint
{
    public function getInstance(): static
    {
        return new static();
    }
}
```

Datatype blandet
------------------

Typen `mixed` kan nu bruges som et funktions- eller metodeargument. Det betyder, at metoden altid skal acceptere et input (og derfor er et obligatorisk argument).

Hvis du i det mindste kan det en lille smule, skal du altid bruge en direkte datatype eller i det mindste union. Mixed er kun nyttig, hvis funktionen kan acceptere hvad som helst. I praksis er denne anvendelse f.eks. nyttig for forskellige dump-funktioner, der accepterer vilkårlige input og skal kunne vise dem.

Typen `mixed` accepterer følgende typer: `string`, `int`, `float`, `null`, `bool`, `array`, `callable`, `object`, `resource`.

David vil derefter bruge den blandede type til sin funktion:

```php
function bdump(mixed $var): mixed
{
	Tracy\Debugger::barDump($var);
	return $var;
}
```

Token throw som et udtryk
------------------------

Tokenet `throw` er nu blevet et udtryk, hvilket i praksis betyder, at en undtagelse kan udløses, når lambdafunktionen `fn()` bliver afkortet, eller når en ternær operatør kontrolleres:

```php
$error = fn () => throw new \InvalidArgumentException('Dette giver altid en fejl.');

$userName = $user['navn'] ?? throw new \LogicException('Brugeren skal have et navn.');
```

Funktionen str_contains()
-----------------------

PHP indeholder endelig en indbygget funktion til at kontrollere, om standardstrengen indeholder en delstreng.

For eksempel:

```php
if (str_contains('Honzik kan lide katte.', 'katte')) {
	echo 'Funktionen håndterer katte.';
}
```

Tidligere blev forekomsten af en delstreng kontrolleret ved hjælp af funktionen [strpos](/strpos-function):

```php
if (strpos('Honzik kan lide katte.', 'katte') !== false) {
	echo 'Funktionen håndterer katte.';
}
```

Funktioner str_starts_with() og str_ends_with()
----------------------------------------------

Et par nye funktioner til at kontrollere, om en streng starter eller slutter med en delstreng:

```php
str_starts_with('Honzik kan lide katte.', 'Honzik'); // sandt

str_ends_with('Honzik kan lide katte.', 'katte.'); // sandt
```

Funktion get_debug_type()
-------------------------

Forbedrer output af den eksisterende funktion [gettype](/function-gettype), som kun returnerede den generiske type af den overgivne variabel. Funktionen bruges f.eks. når vi kaster en undtagelse, når vi får ikke-valide input og ønsker at fortælle brugeren, hvad de faktisk har afgivet.

Når vi kalder funktionen `gettype()` med en variabel, der indeholder en instans af klassen `\App\User`, returnerer funktionen `object`, så vi ved ikke, hvilken klasse det er. Den nye funktion `get_debug_type()` returnerer navnet på klassen.

Funktionen get_resource_id()
--------------------------

Denne funktion returnerer identifikatoren for en ekstern ressource fra en variabel.

For eksempel håndterer PHP forbindelsen til en MySql-database ved hjælp af en særlig datatype `resource`, og nu er det muligt at finde ud af, hvilket ID der er blevet tildelt den.

> **Historisk note:**
>
> Typen `resource` i PHP blev oprettet på et tidspunkt, hvor PHP endnu ikke vidste, hvordan man brugte objekter, og hvor man på en eller anden måde måtte finde ud af, hvordan man kunne videregive referencer til noget som en `datatype`. I fremtiden kan du forvente, at `resource` vil blive fjernet fra sproget helt og holdent, så det er bedst ikke at bruge denne funktion.

Udvidelsen ext-json er altid tilgængelig
-------------------------------------

Tidligere kunne PHP kompileres uden understøttelse af json. Nu vil json altid være tilgængelig, så du kan fjerne afhængigheden `ext-json` fra dine `composer.json`-filer og altid vide, at json kan bruges.

Forrang for sammenkædning
-------------------------------------------------------

Forestil dig noget i stil med:

```php
echo 'Den samlede sum:' . $a + $b;
```

Er det nummeradditionen, der foretages først, eller tilføjes variablen `$a` først til strengen, og derefter tilføjes hele den nye streng til `$b`?

Man ville forvente, at tilføjelse skulle ske først, men det er en fin antagelse. PHP gør faktisk noget i den retning:

```php
echo ('Den samlede sum:' . $a) + $b;
```

PHP 8 vil nu opføre sig forudsigeligt:

```php
echo 'Den samlede sum:' . ($a + $b);
```

Generelt er det dog altid bedre at bruge parenteser til at afgrænse et udtryk.

Stabil bestilling
---------------

Før PHP 8 blev stringsortering udført med en såkaldt ustabil algoritme, hvilket betyder, at PHP ikke garanterede, at elementer med samme (eller på anden måde tilsvarende) værdi ikke ville blive byttet. Den nye version ændrer opførslen af alle sorteringsfunktioner til stabil, så sorteringen altid udføres deterministisk, og du får altid det samme output.

Dette løser f.eks. tilfælde, hvor vi rangordnede brugerbedømmelser efter relevans, men hvor nogle bedømmelser havde samme score. Nu vises de i samme rækkefølge hver gang du sorterer, og de vil ikke springe over hele tiden.

Andre nye funktioner
---------------

PHP har mange andre mindre nye funktioner og forbedringer. F.eks. vil fejl blive kastet anderledes (men det generer ikke os, der skriver fejlfri kode, vel?).

Du kan altid se den fulde liste over ændringer i den officielle dokumentation og i RFC-posten.

Hvad jeg savner i den nye PHP
-----------------------

Jeg ville ønske, at PHP endelig understøttede sammensatte array-typer, for eksempel når en metode returnerer et array af identifikatorer, skal vi stadig kun angive `getIds(): array`, og noget som `getIds(): int[]` ville være meget bedre. Måske får vi det snart at se, og så vil den stærke typekontrol være komplet.

Flere ressourcer
------------

David Grudl holdt et fint foredrag om de nye ting hos Posobot. Jeg anbefaler, at du ser optagelsen:

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/ZYkwmcCUTCg" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Jeg vil gerne takke David for hans foredrag, da jeg har hentet nogle oplysninger fra det til denne artikel. Især ting om Nettes overgang til PHP 8 og andre tips om PHP bag kulisserne.
