PHP 8 je venku - kompletní přehled novinek
================================

> id: "8b6ce751-195f-41d2-82c6-1af4be3e86b5"
> slugCS: php-8-kompletni-prehled-novinek
> publicationDate: "2020-11-26 11:53:54"
> mainCategoryId: "17545205-215b-4962-b910-0d67ad1e933a"

Dnes 26. listopadu 2020 byla po několika letech vydána nová hlavní verze PHP 8, která obsahuje tučnou sadu novinek. Jde o jednu z nějvětších aktualizací za poslední dobu, která si zaslouží speciální článek.

V tomto článku si shrneme všechny hlavní novinky a rozdíly v syntaxi i možnostech proti starší verzi. Většina novinek je zpětně kompatibilní a přináší vylepšení chování, které se vám bude líbit.

> **Důležitá informace:** PHP 8 je nyní ve fázi `feature freeze`, což znamená, že již nelze přidávat nové chování a jen se opravují bugy. S kompatibilitou tedy můžete počítat a plně si odladit své aplikace.

Union typy
----------

PHP se obecně poslední roky překlápí z čistě dynamického jazyka, kde jakákoli proměnná mohla obsahovat cokoli do striktní podoby, kdy předem jasně víme, jaký datový typ bude v jaké proměnné, parametru, argumentu nebo property. Použití [datových typů](/datove-typy) zůstává stále dobrovolné, nicméně použití silného typování doporučuji a sám používám na všech projektech.


Union typy vyjadřují kolekci více typů, přičemž argument nebo property přijímá jakýkoli v nich.

Například:

```php
function validatePsc(string|int $psc): bool
{
	// implementace
}
```

Funkce `validatePsc()` v proměnné `$psc` akceptuje datový typ `string` (řetězec) a `int` (celé číslo).

V předchozí verzi PHP 7.4 tento zápis nebyl možný a obcházel se typicky [komentářem](/dokumentacni-komentare):

```php
/**
 * @param string|int $psc
 */
function validatePsc($psc): bool
{
	// implementace
}
```

Tento anotační komentář však PHP ignoruje (jde přeci o komentář) a museli jsme kontrolu provádět dodatečně externím nástrojem, jako je například PhpStan, kterou řada vývojářů ignorovala. Nyní se kontrola provádí přímo v runtime (při běhu aplikace) a nejde obejít.

Určitý typ union typu PHP už ale zná od verze 7, kdy bylo možné k hlavnímu typu říct, že může být také `nullable`, tj. akceptuje hlavní datový typ a k tomu navíc hodnotu `null`.

To se zapisovalo dvěma způsoby, přičemž každý měl jiný význam:

```php
function setPhone(?string $phone): void
{
	// implementace
}

// nebo

function setPhone(string $phone = null): void
{
	// implementace
}

// nebo kombinace

function setPhone(?string $phone = null): void
{
	// implementace
}
```

Všechny zápisy říkají, že telefonní `int` (celé číslo) je buď `string` (řetězec), nebo `null`.

- První zápis vyžaduje vždy předání hodnoty
- Druhý zápis nevyžaduje předání žádné hodnoty, když se nepředá nic, jako výchozí hodnota se použije `null` (jde o nepovinný argument)
- Třetí zápis je kombinace možností a chová se jako druhý zápis

Při použití union typů už nebude možné použít zápis s otazníkem a musíme datový typ `null` striktně definovat, například:


```php
function setPhone(string|int|null $phone = null): void
{
	// implementace
}
```

Telefonní číslo nyní musí být `string`, `int` nebo `null`.

Union typy mají ještě celou řadu využití, které si pokročilí vývojáři přečtou v dokumentaci nebo implementaci konkrétních knihoven.


JIT - rychlejší zpracování scriptů
----------------------------------

Kompilátor JIT (just in time - právě včas) přináší značné zlepšení výkonu komplikace (parsování a pochopení) scriptu. Toto chování se ale může lišit v kontextu webových požadavků.

Jestli máte JIT aktivní lze nově vidět v Tracy baru v rámci Nette framework a další podrobnosti najdete v [samostatném článku](https://stitcher.io/blog/php-jit) (anglicky).

O kompilaci lze obecně říct to, že se PHP snaží zpracovat kód dopředu, aby při zpracování konkrétního požadavku nemuselo procházet fyzický soubor se scriptem, ten parsovat a intepretovat. V minulosti se toto řešilo přes rozšíření OPCache (které mají servery a hostingy defaultně dostupné) a zlepšovalo to rychlost zpracování zhruba o polovinu.

Obecně platí pravidlo, že pokud máte pomalou aplikaci, tak je vždy lepší zvolit vhodný algoritmus pro zpracování konkrétní úlohy, než provádět mikrooptimalizace v kódu. Obvykle velké zdržení způsobuje čekání na databázi a její pomalé dotazy, ukládání session, čekání na uvolnění pevného disku a další hardwarové operace.

Operátor nullsafe (optional chaining)
-------------------------------------

Velmi často je potřeba v reálné aplikaci ověřit existenci návratové hodnoty (že není `null`) z jedné metody a poté podmíněně volat další. Na to se výborně hodí [ternární operátory](/ternarni-operator), které nicméně pracují pouze s jednou podmínkou a nejde je zanořovat. Nullsafe operátor umožňuje zanoření nativně.

> **TIP:** Prakticky stejné chování již nyní podporuje šablonovací systém Latte, který ovšem tento typ syntaxe přepisuje do nativního PHP kódu, proto můžete nullsafe operátor používat i na starších verzích PHP (od verze PHP 7). Čest Davidovi za tuto úpravu!

Použití je pak snadné:

```php
$orderId = $order?->getId();
```

Proměnná `$orderId` obsahuje buď hodnotu, kterou vrátí metoda `getId()`, nebo `null`, pokud je v proměnné `$order` hodnota `null` a metoda `getId()` by tedy nešla zavolat.

Tento typ problému se v PHP 7 obcházel následující syntaxí přes ternární operátor:

```php
$orderId = isset($order) ? $order->getId() : null;
```

Případně podmínkou:

```php
if (isset($order)) {
	$orderId = $order->getId();
} else {
	$orderId = null;
}
```

Zápis lze napsat i zanořeně dále do volání. Ukázku jsem převzal z [dokumentace Latte](https://latte.nette.org/cs/syntax#toc-volitelne-retezeni-s-nullsafe-operatorem), která to dokonale popisuje:

```php
$orderName = $order->item?->name;

// to samé jako:

$orderName = isset($order->item) ? $order->item->name : null;
```

Typické použití je při vypisování složitějších struktur v šabloně, třeba v Latte to vypadá takto (ukázka převzata z dokumentace):

```php
{$user?->address?->street}
// znamená cca ($user !== null) && ($user->address !== null) ? $user->address->street : null

{$items[2]?->count}
// zamená cca ($items[2] !== null) ? $items[2]->count : null

{$user->getIdentity()?->name}
// zamená cca $user->getIdentity() !== null ? $user->getIdentity()->name : null
```

V reálném kódu to může vypadat třeba tak, že chceme zjistit zemi zákazníka na základě čtení jeho profilu (a data máte v databázi uloženy hezky přes relace, jak se to má správně dělat), pak to ve starém PHP vypadalo třeba takto:

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

Nově to lze zkrátit do jediného řádku:

```php
$country = $session?->user?->getAddress()?->country;
```

Použití nullsafe operátoru také brání různým chybám, které nešlo v PHP 7 jednoduše odhalit nezkušeným vývojářem.

Například tento zápis vygeneruje fatální chybu:

```php
var_dump($invoice->getDate()->format('Y-m-d') ?? null);

// vrátí: Fatal error: Uncaught Error: Call to a member function format() on null
```

Správně má syntaxe vypadat takto:

```php
var_dump($invoice->getDate()?->format('Y-m-d'));

// vrátí: null
```

Pojmenované argumenty
---------------------

Ve starém dobrém PHP se volání funkcí s argumenty muselo zapisovat tak, že se argumenty předávaly přesně v tom pořadí, v jakém je definuje cílová funkce. Na tom není nic špatného, nicméně při použití řady parametrů s podobnou hodnotou to mohlo způsobit horší čitelnost. Nebo pokud jsme chtěli předat až n-tý parametr v pořadí, tak se musely předat i všechny nepovinné parametry předtím, což se mohlo negativně projevit na čitelnosti i dopředné kompatibilitě.

Představte si třeba funkci `setCookie()` v Nette, která má opravdu hodně argumentů:

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

První tři argumenty (`$name`, `$value` a `$time`) jsou povinné, pokud jsme ale chtěli předat ještě argument `$httpOnly`, museli jsme předat všechny předchozí a správně dopočítat pořadí:


```php
$http->setCookie(
	'mojeCookie',
	'David má rád koně',
	'now',
	null, // path
	null, // domain
	null, // secure
	true
);
```

Což zkrátka nechcete dělat, když nemusíte.

Elegantní zápis pak vypadá:

```php
$http->setCookie(
    name: 'mojeCookie',
    value: 'David má rád koně',
    time: 'now',
    httpOnly: true
);
```

Tento typ syntaxe vyžaduje, aby se názvy argumentů v cílové funkci nikdy nezměnily, protože budou napsané i při volání. Aspoň je budou vývojáři lépe pojmenovávat.

Pokud chceme použít jen některý z argumentů, lze syntaxi kombinovat a zestručnit jen do jednoho řádku:

```php
$http->setCookie('mojeCookie', 'David má rád koně', 'now', httpOnly: true);
```

První 3 argumenty se předají původním způsobem, potom se předá nepovinný argument `httpOnly` (protože je pojmenovaný).

Attributy
---------

Většina velkých jazyků, jako je Java nebo C# už nativně obsahují tzv. anotace, což je nativní syntaxe jazyka, která umožňuje přidávat meta informace k jiným jazykovým konstruktům.

V PHP tento typ syntaxe dlouho chyběl a obcházel se použitím DOC komentářů, což je klasický komentář nad metodou, akorát má dvě hvězdičky `/**`.

Při zpracování scriptu se tyto komentáře ignorují a musí se přidávat speciální uživatelská logika, která je přes reflexe přímo za běhu scriptu parsuje a intepretuje. Asi chápete, jaký to pak může mít dopad na výkon, navíc syntaxi komentářů nelze vyžadovat a velmi těžko se v compiletime (při zpracování scriptu ještě před spuštěním) kontroluje a musí se na to opět použít další nástroje mimo běžnou výbavu PHP.

Aby PHP zachovalo zpětnou kompatibilitu, přináší atributy se syntaxí podobající se alternativnímu zápisu komentářů, což nerozbije spuštění scriptu na starém PHP.

Původní zápis (použití například pro Inject závislosti v Nette Presenteru):

```php
final class HomepagePresenter extends BasePresenter
{
	/** @inject */
	public EntityManager $entityManager;
}
```

Nově lze komentář odebrat a použít nativní attribut:

```php
use App\Attributes\Inject;

final class HomepagePresenter extends BasePresenter
{
	#[Inject]
	public EntityManager $entityManager;
}
```

Velmi důležité je zejména to, že attribut už není jen nějaký kus stringu v komentáři, ale je to fyzická třída, která je validní PHP kód.

To je skvělé, protože lze nyní bezpečně validovat vstupy do atributu a použití attributu se vlastně stává voláním jeho konstruktoru, kde lze použít i další logiku. Už se těším, až to bude nativně podporovat Doctrine, která anotace používá úplně pro všechno.

Implementace samotného attributu pak může vypadat třeba takto:

```php
#[Attribute]
class Inject
{
    public $value;
 
    public function __construct($value)
    {
        $this->value = $value;
    }
}
```

V rámci attributu lze použít opět striktní logiku, jako je kontrola datových typů argumentů, union typy a další vychytávky jazyka.

Match expression
----------------

Nový jazykový konstrukt `match()` je modernizované vylepšení starého dobrého `switche()` (který se snažím nepoužívat) a přináší řadu skvělých vlastností (kvůli kterým ho zase používat začnu).

Například chceme upravit hodnotu proměnné na základě vstupu:

```php
$pozdrav = match(bool $formal) {
    true => 'Dobrý den',
    false => 'Ahoj',
};
```

Důležitá novinka v syntaxi je zejména to, že nově nemusíme použít `break` (jako to má starý `switch`) a syntaxe je obecně mnohem úspornější.

Zároveň lze v rámci podmínky ověřovat více vstupů najednou (oddělené čárkou) a případně vrátit výchozí hodnotu (když nevyhovuje žádná).

To se hodí třeba při přepisu stavového HTTP kódu na chybovou hlášku (určitě to oceníte při zpracování kódů výjimek):

```php
$message = match ($statusCode) {
    200, 300 => null,
    400 => 'not found',
    500 => 'server error',
    default => 'unknown status code',
};
```

Porovnání hodnot se provádí striktně operátorem `===` (switch používá jen `==`), což ukazuje opět na to, že PHP jde cestou striktního návrhu. Vstup `'200'` (string obsahující číslo) proto nebude v předchozím případě akceptován.

Pokud neuvedeme hodnotu pro `default` a nedojde ke shodě, vyhazuje se chyba `UnhandledMatchError`.

Nová syntaxe také umožňuje pro porovnání použít výraz nebo volání funkce (chová se jako podmínka). V případě chyby pak můžeme vyhodit výjimku (protože se token `throw` nově stav výrazem a lze takto použít):

```php
$message = match ($statusCode) {
    200 => null,
    $this->checkServerError($statusCode) => throw new ServerError(),
    default => 'unknown status code',
};
```

Propagace vlastností (properties) do konstruktoru
-------------------------------------------------

Jde jen o syntaktický cukr, který se bude hodit pro rychlou a jednoduchou definici entit a její properties rovnou v konstruktoru.


Například původní entita:

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

Lze zkrátit jenom na:

```php
final class User 
{
    public function __construct(
        public string $name
    ) {}
}
```

Property `$name` se validuje vůči datovému typu `string` a její hodnota lze číst rovnou z instance, protože jde o public property. Pokud v Nette používáte navíc SmartObject (ten pro PHP 8 spíše nedoporučuji), tak lze přistupovat i k privátním properties tím, že se volá nejprve jejich getter metoda a tato syntaxe to opět zjednoduší.

Návratový typ static
--------

Už v minulosti jsme mohli jako návratovou hodnotu metody použít datový typ `self`, který ovšem vrací instanci právě té třídy, kde je definován. Datový typ `static` to umí obecně i v případě dědičnosti a vrátí datový typ třídy, z které se prováděla instance, nikoli jejího předka.

Například:

```php
class BaseEndpoint
{
    public function getInstance(): static
    {
        return new static();
    }
}
```

Datový typ mixed
------------------

Nově lze jako argument funkce nebo metody použít typ `mixed`. Ten znamená to, že metoda musí nějaký vstup vždy přijmout (a jde tedy o povinný argument).

Pokud můžete aspoň trochu, vždy používejte přímý datový typ, nebo aspoň union. Mixed se hodí jen v případě, kdy funkce přijímá opravdu cokoli. V praxi se použití hodí například pro různé dumpovací funkce, které přijímají libovolný vstup a musí ho umět zobrazit.

Typ `mixed` akceptuje tyto typy: `string`, `int`, `float`, `null`, `bool`, `array`, `callable`, `object`, `resource`.

David pak mixed typ určitě použije pro jeho funkci:

```php
function bdump(mixed $var): mixed
{
	Tracy\Debugger::barDump($var);
	return $var;
}
```

Token throw jako výraz
------------------------

Token `throw` se nově stal výrazem, to v praxi znamená to, že lze výjimku vyhodit při zkrácené lambda funkci `fn()`, nebo třeba při ověření ternárního operátoru:


```php
$error = fn () => throw new \InvalidArgumentException('Toto vždycky vyhodí chybu.');

$userName = $user['name'] ?? throw new \LogicException('Uživatel musí mít jméno.');
```

Funkce str_contains()
-----------------------

PHP konečně obsahuje nativní funkci pro ověření, že výchozí řetězec obsahuje nějaký podřetězec.

Například:

```php
if (str_contains('Honzík má rád kočky.', 'kočky')) {
	echo 'Funkce zpracovává kočky.';
}
```

V minulosti se výskyt podřetězce ověřoval funkcí [strpos](/funkce-strpos):

```php
if (strpos('Honzík má rád kočky.', 'kočky') !== false) {
	echo 'Funkce zpracovává kočky.';
}
```

Funkce str_starts_with() a str_ends_with()
----------------------------------------------

Dvojice nových funkcí pro ověření, jestli řetězec začíná nebo končí podřetězcem:

```php
str_starts_with('Honzík má rád kočky.', 'Honzík'); // true

str_ends_with('Honzík má rád kočky.', 'kočky.'); // true
```

Funkce get_debug_type()
-------------------------

Vylepšení výstupu již existující funkce [gettype](/funkce-gettype), která vracela jen obecný typ předané proměnné. Funkce se používá třeba při vyhození výjimky, kdy dostaneme nevalidní vstup a chceme uživateli říct, co reálně předal.

Když voláme funkci `gettype()` s proměnnou obsahující instanci třídy `\App\User`, funkce vrátí `object`, takže nevíme, o jakou třídu jde. Nová funkce `get_debug_type()` vrátí název třídy.

Funkce get_resource_id()
--------------------------

Funkce vrátí identifikátor externího zdroje z proměnné.

Například připojení k MySql databázi řeší PHP tak, že používá speciální datový typ `resource`, nyní lze zjistit, jaké ID mu bylo přiděleno.

> **Historická poznámka:**
>
> Typ `resource` v PHP vznikl v době, když ještě neumělo objekty a muselo se nějak vyřešit předávání referencí na něco jako "datový typ". Do budoucna se dá očekávat, že se `resource` z jazyka úplně odebere, proto tuto funkci raději nepoužívejte.

Rozšíření ext-json je vždy dostupné
-------------------------------------

V minulosti šlo PHP zkompilovat bez podpory pro json. Nyní bude json vždy dostupný, proto můžete závislost `ext-json` bezepčně odebrat ze svých `composer.json` souborů a vždy budete vědět, že lze json použít.

Priorita zřetězení operátorů (Concatenation precedence)
-------------------------------------------------------

Představte si něco jako:

```php
echo 'Součet: ' . $a + $b;
```

Provede se nejprve sčítání čísel, nebo se nejprve připojí proměnná `$a` k řetězci a až poté se bude celý nový řetězec sčítat s `$b`?

Člověk by čekal, že se nejprve provede sčítání, což je ale milný předpoklad. PHP ve skutečnosti vykoná něco jako toto:

```php
echo ('Součet: ' . $a) + $b;
```

PHP 8 se nyní zachová předvídatelně:

```php
echo 'Součet: ' . ($a + $b);
```

Obecně ale vždy raději používejte závorku k ohraničení výrazu.

Stabilní řazení
---------------

Před PHP 8 se řazení řetězců provádělo tzv. nestabilním algoritmem, což znamená to, že PHP negarantovalo, že prvky se stejnou (nebo jinak ekvivalentní) hodnotou neprohodí. Nová verze mění chování všech řadídích funkcí na stabilní, proto se řazení provede vždy deterministicky a dostanete vždy stejný výstup.

Řeší to například případy, kdy jsme řadili hodnocení uživatelů podle relevance, ale některá hodnocení měla stejný počet bodů. Nyní se při každém řazení zobrazí ve stejném pořadí a nebudou průběžně přeskakovat.

Ostatní novinky
---------------

PHP obsahuje ještě mnoho dalších drobných novinek a vylepšení. Například se jinak budou vyhazovat chyby (ale to nás, co píšeme kód bez chyb vůbec netrápí, že?).

Celkový přehled změn můžete vždy vidět v oficiální dokumentaci a příspěvku RFC.

Co mi v novém PHP chybí
-----------------------

Líbilo by se mi, kdyby PHP konečně podporovalo složené typy polí, například když metoda vrací pole identifikátorů, tak musíme stále uvádět jen `getIds(): array` a mnohem lepší by bylo něco jako: `getIds(): int[]`. Třeba se brzy dočkáme a bude tím silná typová kontrola kompletní.

Další zdroje
------------

Na Posobotě David Grudl měl hezkou přednášku o novinkách. Doporučuji se podívat na záznam:

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/ZYkwmcCUTCg" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Tímto Davidovi za přednášku moc děkuji, protože jsem z ní čerpal některé informace pro tento článek. Zejména věci o směřování Nette k PHP 8 a další zákulisní tipy o PHP.
