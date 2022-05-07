PHP 8 je vonku - kompletný prehľad
==================================

> id: '8b6ce751-195f-41d2-82c6-1af4be3e86b5'
> slug:
> 	cs: php-8-kompletni-prehled-novinek
> 	sk: php-8-je-vonku---kompletny-prehlad
> 
> publicationDate: '2020-11-26 11:53:54'
> mainCategoryId: '17545205-215b-4962-b910-0d67ad1e933a'
> sourceContentHash: f657145ac1d2a1109fcc2a1ff3b6e6cf

Dnes, 26. novembra 2020, bola po niekoľkých rokoch vydaná nová hlavná verzia PHP 8, ktorá obsahuje odvážny súbor nových funkcií. Ide o jednu z najväčších aktualizácií za dlhú dobu a zaslúži si osobitný článok.

V tomto článku zhrnieme všetky hlavné nové funkcie a rozdiely v syntaxi a možnostiach v porovnaní so staršou verziou. Väčšina nových funkcií je spätne kompatibilná a prináša vylepšenia správania, ktoré oceníte.

> **Dôležité informácie:** PHP 8 je teraz vo fáze `zmrazenia funkcií`, čo znamená, že už nie je možné pridávať nové správanie a opravujú sa len chyby. Môžete sa tak spoľahnúť na kompatibilitu a plne odladiť svoje aplikácie.

Typy Únie
----------

Jazyk PHP vo všeobecnosti v posledných rokoch prechádza od čisto dynamického jazyka, v ktorom môže akákoľvek premenná obsahovať čokoľvek, k striktnej forme, v ktorej vopred vieme, aký dátový typ bude v ktorej premennej, parametri, argumente alebo vlastnosti. Používanie [data-types](/datove-types) je stále voliteľné, ale odporúčam používať silné písmo a sám ho používam vo všetkých projektoch.


Typy únie vyjadrujú kolekciu viacerých typov, pričom akceptujú akýkoľvek argument alebo vlastnosť.

Napríklad:

```php
function validatePsc(string|int $psc): bool
{
	// implementácia
}
```

Funkcia `validatePsc()` v premennej `$psc` akceptuje dátové typy `string` (reťazec) a `int` (celé číslo).

V predchádzajúcej verzii PHP 7.4 tento zápis nebol možný a zvyčajne sa obchádzal pomocou [comment](/document-comment):

```php
/**
 * @param string|int $psc
 */
function validatePsc($psc): bool
{
	// implementácia
}
```

PHP však tento komentár ignoruje (je to predsa len komentár) a my sme museli vykonať dodatočnú kontrolu pomocou externého nástroja, napríklad PhpStan, čo mnohí vývojári ignorovali. Kontrola sa teraz vykonáva priamo za behu (keď je aplikácia spustená) a nie je možné ju obísť.

PHP však pozná určitý typ typu union už od verzie 7, kedy bolo možné povedať, že hlavný typ môže byť aj `nullable`, t. j. akceptuje hlavný dátový typ plus hodnotu `null`.

Toto bolo napísané dvoma spôsobmi, každý s iným významom:

```php
function setPhone(?string $phone): void
{
	// implementácia
}

// alebo

function setPhone(string $phone = null): void
{
	// implementácia
}

// alebo kombinácia

function setPhone(?string $phone = null): void
{
	// implementácia
}
```

Všetky položky hovoria, že telefón `int` (celé číslo) je buď `string` alebo `null`.

- Prvý zápis vždy vyžaduje odovzdanie hodnoty
- Druhý zápis nevyžaduje odovzdanie žiadnej hodnoty; ak nie je odovzdaná žiadna hodnota, predvolená hodnota je `null` (ide o nepovinný argument)
- Tretia položka je kombináciou možností a správa sa ako druhá položka

Pri používaní typov union už nebudeme môcť používať zápis s otáznikom a musíme striktne definovať napríklad dátový typ `null`:

```php
function setPhone(string|int|null $phone = null): void
{
	// implementácia
}
```

Telefónne číslo musí byť teraz `string`, `int` alebo `null`.

Typy Union majú stále množstvo využití, o ktorých sa pokročilí vývojári dočítajú v dokumentácii alebo v implementácii konkrétnych knižníc.


JIT - rýchlejšie spracovanie skriptov
----------------------------------

Kompilátor JIT (just in time) prináša výrazné zlepšenie výkonu komplikovania skriptov (analyzovanie a porozumenie). Toto správanie sa však môže líšiť v kontexte webových požiadaviek.

Na paneli Tracy v rámci Nette teraz môžete zistiť, či máte zapnutý JIT, a ďalšie podrobnosti nájdete v [samostatnom článku](https://stitcher.io/blog/php-jit).

Všeobecne sa o kompilácii dá povedať, že PHP sa snaží spracovať kód vopred, aby pri spracovaní konkrétnej požiadavky nemusel prechádzať fyzický súbor so skriptom, analyzovať ho a interpretovať. V minulosti sa to riešilo prostredníctvom rozšírenia OPCache (ktoré majú servery a hostitelia k dispozícii v predvolenom nastavení) a rýchlosť spracovania sa tým zvýšila približne o polovicu.

Všeobecne platí, že ak máte pomalú aplikáciu, je vždy lepšie vybrať vhodný algoritmus na zvládnutie konkrétnej úlohy, ako vykonávať mikrooptimalizácie v kóde. Veľké oneskorenia sú zvyčajne spôsobené čakaním na databázu a jej pomalé dopyty, ukladaním relácií, čakaním na uvoľnenie miesta na pevnom disku a inými hardvérovými operáciami.

Operátor Nullsafe (voliteľné reťazenie)
-------------------------------------

V reálnych aplikáciách je veľmi často potrebné overiť existenciu návratovej hodnoty (či nie je `null`) z jednej metódy a potom podmienečne zavolať inú. Na to sa výborne hodia [ternárne operátory](/ternárny operátor), ktoré však pracujú len s jednou podmienkou a nedajú sa vkladať. Operátor nullsafe umožňuje vnorenie natívne.

> **TIP:** Prakticky rovnaké správanie už podporuje šablónovací systém Latte, ktorý však tento typ syntaxe v natívnom kóde PHP potláča, takže operátor nullsafe môžete používať aj v starších verziách PHP (od PHP 7). Pochvala Davidovi za túto úpravu!

Vďaka tomu sa ľahko používa:

```php
$orderId = $order?->getId();
```

Premenná `$orderId` obsahuje buď hodnotu vrátenú metódou `getId()`, alebo `null`, ak premenná `$order` obsahuje hodnotu `null` a metódu `getId()` nebolo možné zavolať.

Tento typ problému bol v PHP 7 obídený nasledujúcou syntaxou pomocou ternárneho operátora:

```php
$orderId = isset($order) ? $order->getId() : null;
```

Prípadne podmienka:

```php
if (isset($order)) {
	$orderId = $order->getId();
} else {
	$orderId = null;
}
```

Záznam je možné zapísať ďalej do výzvy. Vzorku som prevzal z [Latte documentation](https://latte.nette.org/cs/syntax#toc-volitelne-retezeni-s-nullsafe-operatorem), ktorá ju dokonale opisuje:

```php
$orderName = $order->item?->name;

// rovnako ako:

$orderName = isset($order->item) ? $order->item->name : null;
```

Typické použitie je pri výpise zložitejších štruktúr v šablóne, napríklad v Latte to vyzerá takto (ukážka prevzatá z dokumentácie):

```php
{$user?->address?->street}
// znamená približne ($user !== null) && ($user->adresa !== null) ? $user->address->street : null

{$items[2]?->count}
// replace approx ($items[2] !== null) ? $items[2]->count : null

{$user->getIdentity()?->name}
// replace approx $user->getIdentity() !== null ? $user->getIdentity()->name : null
```

V reálnom kóde to môže vyzerať napríklad takto, že chceme zistiť krajinu zákazníka čítaním jeho profilu (a údaje máte v databáze uložené pekne cez relácie, ako by sa to malo robiť), potom v starom PHP to vyzeralo takto:

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

Teraz sa dá skrátiť na jeden riadok:

```php
$country = $session?->user?->getAddress()?->country;
```

Použitie operátora nullsafe tiež zabraňuje rôznym chybám, ktoré by neskúsený vývojár v PHP 7 nemohol ľahko odhaliť.

Napríklad táto položka vygeneruje fatálnu chybu:

```php
var_dump($invoice->getDate()->format('Y-m-d') ?? null);

// return: fatal error: uncaught Chyba: volanie členskej funkcie format() na null
```

Správna syntax je nasledovná:

```php
var_dump($invoice->getDate()?->format('Y-m-d'));

// vráti: null
```

Pomenované argumenty
---------------------

V starom dobrom PHP sa volania funkcií s argumentmi museli písať tak, že sa argumenty odovzdávali v presnom poradí definovanom cieľovou funkciou. Na tom nie je nič zlé, avšak pri použití viacerých parametrov s podobnými hodnotami by to mohlo spôsobiť horšiu čitateľnosť. Alebo ak by sme chceli odovzdať až n-ty parameter v poradí, museli by sa všetky nepovinné parametre odovzdať skôr, čo by mohlo mať negatívny vplyv na čitateľnosť a kompatibilitu dopredu.

Predstavte si napríklad funkciu `setCookie()` v Nette, ktorá má veľa argumentov:

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

Prvé tri argumenty (`$name`, `$value` a `$time`) sú povinné, ale ak sme chceli odovzdať argument `$httpOnly`, museli sme odovzdať všetky predchádzajúce a správne vypočítať poradie:

```php
$http->setCookie(
	'myCookie',
	'David má rád kone',
	'teraz',
	null, // cesta
	null, // doména
	null, // bezpečné
	true
);
```

Čo jednoducho nechcete robiť, ak nemusíte.

Elegantné písmo potom vyzerá takto:

```php
$http->setCookie(
    name: 'myCookie',
    value: 'David má rád kone',
    time: 'teraz',
    httpOnly: true
);
```

Tento typ syntaxe vyžaduje, aby sa názvy argumentov v cieľovej funkcii nikdy nemenili, pretože pri volaní budú stále typované. Vývojári ich aspoň budú môcť lepšie pomenovať.

Ak chceme použiť len jeden z argumentov, syntax môžeme skombinovať a skrátiť na jeden riadok:

```php
$http->setCookie('myCookie', 'David má rád kone', 'teraz', httpOnly: true);
```

Prvé 3 argumenty sa odovzdávajú pôvodným spôsobom, potom sa odovzdá nepovinný argument `httpOnly` (pretože je pomenovaný).

Atribúty
---------

Väčšina veľkých jazykov, ako napríklad Java alebo C#, už natívne obsahuje takzvané anotácie, čo je syntax jazyka, ktorá umožňuje pridávať metainformácie k iným jazykovým konštrukciám.

V PHP tento typ syntaxe dlho chýbal a obchádzal sa pomocou komentárov DOC, čo je klasický komentár nad metódou, len s dvoma hviezdičkami `/**`.

Tieto komentáre sa počas spracovania skriptu ignorujú a na ich rozbor a interpretáciu za behu sa musí pridať špeciálna používateľská logika prostredníctvom reflexie. Asi chápete, aký to môže mať vplyv na výkon, navyše syntax komentárov sa nedá vyžadovať a je veľmi ťažké ju skontrolovať v čase kompilácie (keď sa skript spracúva pred spustením) a opäť na to musíte použiť ďalšie nástroje mimo bežnej sady nástrojov PHP.

Aby sa zachovala spätná kompatibilita, PHP poskytuje atribúty so syntaxou podobnou alternatívnemu zápisu komentárov, ktorý neporušuje spustenie skriptu v staršom PHP.

Pôvodný zápis (používaný napríklad pre závislosti Inject v programe Nette Presenter):

```php
final class HomepagePresenter extends BasePresenter
{
	/** @inject */
	public EntityManager $entityManager;
}
```

Teraz môžete odstrániť komentár a použiť pôvodný atribút:

```php
use App\Attributes\Inject;

final class HomepagePresenter extends BasePresenter
{
	#[Inject]
	public EntityManager $entityManager;
}
```

Je veľmi dôležité, aby atribút už nebol len kusom reťazca v komentári, ale fyzickou triedou, ktorá je platným kódom PHP.

To je skvelé, pretože teraz môžete bezpečne overovať vstupy do atribútu a použitie atribútu sa vlastne stáva volaním jeho konštruktora, kde sa môže použiť iná logika. Teším sa, že to bude natívne podporovať Doctrine, ktorá používa anotácie pre všetko.

Implementácia samotného atribútu by potom mohla vyzerať takto:

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

V rámci atribútu možno opäť použiť prísnu logiku, napríklad kontrolu dátových typov argumentov, typov zväzkov a iných vlastností jazyka.

Výraz zhody
----------------

Nová jazyková konštrukcia `match()` je modernizovaným vylepšením starej dobrej konštrukcie `switch()` (ktorú sa snažím nepoužívať) a prináša množstvo skvelých funkcií (ktoré ma prinútia začať ju opäť používať).

Napríklad chceme zmeniť hodnotu premennej na základe vstupu:

```php
$pozdrav = match(bool $formal) {
    true => 'Dobrý deň,',
    false => 'Ahoj',
};
```

Dôležitou novou vlastnosťou syntaxe je, že nemusíme používať `break` (ako v prípade starého `switch`) a syntax je vo všeobecnosti oveľa úspornejšia.

Zároveň môžeme v rámci podmienky overiť viacero vstupov naraz (oddelených čiarkou) a prípadne vrátiť predvolenú hodnotu (ak žiadna nevyhovuje).

To sa hodí napríklad pri prepisovaní kódu podmienky HTTP na chybovú správu (určite to oceníte pri spracovaní kódov výnimiek):

```php
$message = match ($statusCode) {
    200, 300 => null,
    400 => 'nenájdené',
    500 => 'chyba servera',
    default => 'neznámy stavový kód',
};
```

Porovnávanie hodnôt sa vykonáva striktne pomocou operátora `===` (prepínač používa iba `==`), čo opäť dokazuje, že PHP sa riadi striktným návrhom. Preto vstup `'200`` (reťazec obsahujúci číslo) nebude v predchádzajúcom prípade akceptovaný.

Ak nezadáte hodnotu `default` a neexistuje žiadna zhoda, vyhodí sa chyba `UnhandledMatchError`.

Nová syntax umožňuje použiť na porovnávanie aj výraz alebo volanie funkcie (správa sa ako podmienka). V prípade chyby potom môžeme vyhodiť výnimku (keďže token `throw` je teraz výraz a môže byť použitý týmto spôsobom):

```php
$message = match ($statusCode) {
    200 => null,
    $this->checkServerError($statusCode) => throw new ServerError(),
    default => 'neznámy stavový kód',
};
```

Propagácia vlastností do konštruktora
-------------------------------------------------

Ide len o syntaktický cukor, ktorý bude užitočný na rýchle a jednoduché definovanie entity a jej vlastností priamo v konštruktore.


Napríklad pôvodný subjekt:

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

Dá sa skrátiť len na:

```php
final class User
{
    public function __construct(
        public string $name
    ) {}
}
```

Vlastnosť `$name` je overená voči dátovému typu `string` a jej hodnota sa dá čítať priamo z inštancie, pretože je to verejná vlastnosť. Ak v Nette použijete dodatočný SmartObject (čo v PHP 8 neodporúčam), môžete k súkromným vlastnostiam pristupovať aj tak, že najprv zavoláte ich getter metódu a táto syntax opäť všetko zjednoduší.

Typ návratu static
--------

V minulosti sme mohli ako návratovú hodnotu metódy použiť dátový typ `self`, ktorý však vracia inštanciu triedy, v ktorej je definovaný. Dátový typ `static` to vo všeobecnosti dokáže aj v prípade dedičnosti a vráti dátový typ triedy, z ktorej bola inštancia vykonaná, nie jej predka.

Napríklad:

```php
class BaseEndpoint
{
    public function getInstance(): static
    {
        return new static();
    }
}
```

Typ údajov zmiešaný
------------------

Typ `mixed` je teraz možné použiť ako argument funkcie alebo metódy. To znamená, že metóda musí vždy prijať nejaký vstup (a preto je povinným argumentom).

Ak môžete aspoň trochu, vždy používajte priamy dátový typ alebo aspoň union. Zmiešané je užitočné len vtedy, ak funkcia akceptuje naozaj čokoľvek. V praxi je toto použitie užitočné napríklad pre rôzne výpisové funkcie, ktoré prijímajú ľubovoľný vstup a musia ho vedieť zobraziť.

Typ `mixed` akceptuje nasledujúce typy: `string`, `int`, `float`, `null`, `bool`, `array`, `callable`, `object`, `resource`.

David potom použije zmiešaný typ pre svoju funkciu:

```php
function bdump(mixed $var): mixed
{
	Tracy\Debugger::barDump($var);
	return $var;
}
```

Hod žetónu ako výraz
------------------------

Token `throw` sa teraz stal výrazom, čo v praxi znamená, že výnimka môže byť vyhodená, keď je lambda funkcia `fn()` skrátená alebo keď je kontrolovaný ternárny operátor:

```php
$error = fn () => throw new \InvalidArgumentException('Vždy to vyhodí chybu.');

$userName = $user['názov'] ?? throw new \LogicException('Používateľ musí mať meno.');
```

Funkcia str_contains()
-----------------------

PHP konečne obsahuje natívnu funkciu na kontrolu, či predvolený reťazec obsahuje podreťazec.

Napríklad:

```php
if (str_contains('Honzík má rád mačky.', 'mačky')) {
	echo 'Táto funkcia sa zaoberá mačkami.';
}
```

V minulosti sa výskyt podreťazca overoval pomocou funkcie [strpos](/strpos-funkcia):

```php
if (strpos('Honzík má rád mačky.', 'mačky') !== false) {
	echo 'Táto funkcia sa zaoberá mačkami.';
}
```

Funkcie str_starts_with() a str_ends_with()
----------------------------------------------

Dvojica nových funkcií na kontrolu, či reťazec začína alebo končí podreťazcom:

```php
str_starts_with('Honzík má rád mačky.', 'Honzik'); // true

str_ends_with('Honzík má rád mačky.', 'mačky.'); // true
```

Funkcia get_debug_type()
-------------------------

Vylepšenie výstupu existujúcej funkcie [gettype](/function-gettype), ktorá vracala iba všeobecný typ odovzdanej premennej. Funkcia sa používa napríklad pri vyhodení výnimky, keď dostaneme neplatný vstup a chceme používateľovi povedať, čo vlastne odovzdal.

Keď zavoláme funkciu `gettype()` s premennou obsahujúcou inštanciu triedy `\App\User`, funkcia vráti `object`, takže nevieme, o akú triedu ide. Nová funkcia `get_debug_type()` vráti názov triedy.

Funkcia get_resource_id()
--------------------------

Táto funkcia vracia identifikátor externého zdroja z premennej.

Napríklad pripojenie k databáze MySql PHP rieši pomocou špeciálneho dátového typu `resource`, teraz je možné zistiť, aké ID mu bolo pridelené.

> **Historická poznámka:**
>
> Typ `resource` v PHP bol vytvorený v čase, keď ešte nevedel používať objekty a musel nejako vymyslieť, ako odovzdávať odkazy na niečo ako `dátový typ`. V budúcnosti môžete očakávať, že `resource` bude z jazyka úplne odstránený, preto je lepšie túto funkciu nepoužívať.

Rozšírenie ext-json je vždy k dispozícii
-------------------------------------

V minulosti bolo možné PHP skompilovať bez podpory json. Teraz bude json vždy k dispozícii, takže môžete odstrániť závislosť `ext-json` zo svojich súborov `composer.json` a vždy budete vedieť, že json možno použiť.

Prednosť konkatenácie
-------------------------------------------------------

Predstavte si niečo také:

```php
echo 'Celkový súčet:' . $a + $b;
```

Vykoná sa najprv pridanie čísla alebo sa najprv k reťazcu pridá premenná `$a` a potom sa celý nový reťazec pridá k `$b`?

Človek by očakával, že najprv sa vykoná sčítanie, ale to je pekný predpoklad. PHP v skutočnosti robí niečo podobné:

```php
echo ('Celkový súčet:' . $a) + $b;
```

PHP 8 sa teraz bude správať predvídateľne:

```php
echo 'Celkový súčet:' . ($a + $b);
```

Vo všeobecnosti je však vždy lepšie používať zátvorky na ohraničenie výrazu.

Stabilné objednávanie
---------------

Pred verziou PHP 8 sa triedenie reťazcov vykonávalo pomocou tzv. nestabilného algoritmu, čo znamená, že PHP nezaručovalo, že prvky s rovnakou (alebo inak ekvivalentnou) hodnotou nebudú vymenené. Nová verzia mení správanie všetkých triediacich funkcií na stabilné, takže triedenie prebieha vždy deterministicky a vždy dostanete rovnaký výstup.

To rieši napríklad prípady, keď sme hodnotenia používateľov zoraďovali podľa relevantnosti, ale niektoré hodnotenia mali rovnaké skóre. Teraz sa budú pri každom triedení zobrazovať v rovnakom poradí a nebudú sa priebežne vynechávať.

Ďalšie nové funkcie
---------------

PHP má mnoho ďalších menších nových funkcií a vylepšení. Napríklad chyby sa budú vyhadzovať inak (ale to nás, ktorí píšeme bezchybný kód, netrápi, však?).

Úplný zoznam zmien si môžete vždy pozrieť v oficiálnej dokumentácii a v príspevku RFC.

Čo mi chýba v novom PHP
-----------------------

Bol by som rád, keby PHP konečne podporovalo zložené typy polí, napríklad keď metóda vráti pole identifikátorov, musíme stále zadávať len `getIds(): array` a niečo ako `getIds(): int[]` by bolo oveľa lepšie. Možno sa toho čoskoro dočkáme a silná kontrola typu bude úplná.

Ďalšie zdroje
------------

David Grudl predniesol peknú prednášku o novinkách v Posobote. Odporúčam pozrieť si záznam:

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/ZYkwmcCUTCg" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Týmto sa chcem Davidovi poďakovať za jeho prednášku, pretože som z nej čerpal niektoré informácie pre tento článok. Najmä veci o prechode Nette na PHP 8 a ďalšie zákulisné tipy o PHP.
