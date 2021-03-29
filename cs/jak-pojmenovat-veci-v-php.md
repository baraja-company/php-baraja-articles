Jak pojmenovat proměnné, funkce, metody a třídy
===============================================

> id: f70ac75d-422f-4696-88d5-9e1b843e060a
> slugCS: jak-pojmenovat-veci-v-php
> publicationDate: "2019-08-22 20:48:46"
> mainCategoryId: "0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1"

Pro udržení pořádku v kódu je důležité zvolit jednoznačná pravidla, jak budeme odvozovat názvy. Tato stránka slouží jako přehled relativně oblíbených přístupů velkého množství programátorů, které používám i já sám, případně lidé, s kterými pracuji.

Pokud pracujete v nějakém teamu vývojářů, tak jistě používejte jejich pravidla, nicméně pro obecný vývoj je stejně výhodné stanovit pár dobrých návyků.

Protože pojetí celé syntaxe PHP je opravdu rozsáhlé, tak jsem zdejší příručku rozdělil do mnoha kategorií, které se úzce specializují.

Pokud hledáte rychlé řešení, doporučuji nastudovat <a href="https://www.php-fig.org/psr/psr-4/">standard PSR-4</a>.

Vkládání scriptů, propojení s HTML, načítání souborů
---------------------------------------------------

Každý script musí začínat značkou `<?php`.

Pokud na konci souboru **není HTML**, tak by se neměl ukončovat (aby nemohl vzniknout bílý znak na konci stránky).

Načítání dalších souborů by mělo probíhat podle následujících pravidel:

- Pokud soubor není kritický a lze se bez něj obejít při vykreslování stránky (třeba dodatečné informace v patičce), měl by se načítat přes include: `include 'soubor.php';`
- Kritické soubory jedině přes konstrukt require: `require 'soubor.php';`
- Pokud pracujeme s objekty, tak konstruktem require načteme autoloader, který se o načítání dalších tříd postará sám (toto chování implementuje většina frameworků).


Pokud to je aspoň trochu možné, tak by se měla oddělovat logika získávání dat od renderu do HTML, tj. používat model MVC (model - view - controler).

Takže si nejprve data připravíme třeba v Presenteru:

`Presenter.php`
```php
$cisla = [1, 2, 3];

$data = [];
$data['cisla'] = $cisla; // data předáme do šablony
include 'renderCisel.php'; // načteme šablonu
```


A následně vykreslíme v šabloně:

`renderCisel.php`
```html
<table>
<?php
	foreach ($data['cisla'] as $cislo) {
		echo '<tr><td>' . $cislo . '</td></tr>';
	}
?>
</table>
```


Tento přístup používá většina frameworků a je vhodný pro zvýšení přehlednosti kódu. V pozdější části vývoje by tímto způsobem měl přemýšlet každý zkušenější programátor, který chce rozvíjet přehledně strukturované aplikace (pro velké aplikace je tento přístup přímo nutností).

Názvy proměnných
----------------

Pokud proměnná obsahuje pole hodnot nebo jiných objektů, měla by se jmenovat množným číslem:

```php
$numbers = [1, 2, 3];
```


Protože poté můžeme hodnoty jednoduše iterovat jednotným číslem:

```php
foreach ($numbers as $number) {
	// zpracování čísel
}
```


Název složený z více slov se spojuje do jednoho dlouhého slova s cameCase syntaxí, tj. první slovo začíná malým písmenem, každé další velkým:

```php
$promenna = 'Ahoj!';
$seznamUzivatelu = [
   'Jan Barášek',
   'Barack Obama',
   'Steve Jobs',
   'Stephen Wolfram',
];
$maxFilesInDirectory = 12;
$nameOfPhpScript = 'index.php'; // U zkratky PHP došlo ke zmenšení znaků
```


Názvy funkcí a metod
--------------------

Funkce a metody by vždy ve svém názvu měly jasně říkat, co dělají. Často jde do názvu také zakomponovat i očekávané vstupní parametry a návratovou hodnotu.

Zkuste odhadnout, co dělají následující funkce a jakou mají návratovou hodnotu:

```php
getUserById($id);
saveErrorToLog($message);
createDefaultDirectory($path);
setAuthors(['Jan Barášek', 'Chuck Norris']);
getCurrentTime();
```

Celý trik je v prvním slově v názvu, který jasně říká, jakou metodou bude funkce pracovat. Obvykle se dodržuje následující konvence:

- `get` - získání dat jako pole nebo objekt, vstupní parametry upřesňují hledanou entitu
- `save` - uložení do souboru nebo databáze
- `create` - vytvoření entity (například vytvoření instance objektu)
- `set` - uložení dat do předem nastavené proměnné (uvnitř funkce)

Názvy tříd
----------

Třída je velká entita, která obsahuje velké množství properties a metod, proto by také měla začínat velkým písmenem. Třída by také měla nést pouze jednu entitu (a popisovat její vlastnosti), proto by se měla jmenovat jednotným číslem. Pokud potřebujeme pracovat s více entitami, tak stačí jednotlivé instance uložit do pole.

Příklad:

```php
class User
{

	/**
	 * @var string
	 */
	public $username;

	/**
	 * @var string
	 */
	public $password;

	/**
	 * @var string
	 */
	public $role;

}

class Users
{

	/**
	 * @var User[]
	 */
	public $users;

	public function addUser(User $user)
	{
		$this->users = array_push($this->users, $user);
	}

}
```

Třída **User** se specializuje pouze na informaci o jednom konkrétním uživateli. Pokud chceme pracovat s více uživateli, tak si vytvoříme další třídu (obálku), která ponese pole instancí konkrétní entity.

K tomuto se také často mohou hodit Továrny, které umožňují snadno vytvářet podobné objekty a původní instance recyklovat, což vede k přehlednějšímu kódu a zároveň šetříme systémové prostředky.

Namespace - Jmenné prostory
---------------------------

Namespace je sice nezávislý na fyzickém adresáři, kde je script dostupný, nicméně je dobrým zvykem, když aspoň částečně respektuje rozložení projektu (což vede k lepšímu systému pro tvorbu nových názvů, který je takto více jednoznačný).

Já osobně pojmenovávám namespace podle společného podadresáře pro třídy daného typu.

Příklady:

```php
App\Presenters; // Takto se jmenují všechny presentery
App\Model;      // Takto se jmenuje obecný model
App\Model\Math; // Takto se jmenuje model pracující s matematikou
```

Pro správně nastavený autoloading tříd je dobré dodržovat <a href="http://jakpsatphp.cz/PSR4/">standard PSR-4</a>.

Vlastní konvence (třeba ve firemním týmu)
-----------------------------------------

A jak pojmenováváte Vy? Ocením tipy, jak zlepšit tento článek.

Obecně ale vlastní konvence v rámci týmu moc nedávají smysl, protože je kód pak těžko přenositelný do jiných frameworků a při přijetí nového kolegy se musí zaučovat aktuálně nastaveným zvykům. Nejlepší je proto dodržovat `standard PSR-4`.
