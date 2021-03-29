Regulární výrazy v PHP
======================

> id: "32142161-6ace-4cfd-b472-b48031219e9b"
> slugCS: regex
> perex: "Regulární výrazy a jejich kompletní vysvětlení. Hledání podřetězce, pokročilé nahrazování a generování řetězců."
> publicationDate: "2020-03-08 13:37:38"
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec

Regulární výrazy jsou nástroje, které umožňují jednoduše hledat, validovat, porovnávat, rozdělovat, skládat a nahrazovat řetězce podle masky (vzoru). Jedná se o velmi silný a elegantní nástroj pro pokročilou manipulaci s řetězci.

Maska
-----

Na začátku je nejprve nutné vymyslet samotný regulární výraz, který budeme spouštět. Zadává se formou textového řetězce, který má hromadu pravidel a možností konfigurace (jde o hodně komplexní techniku).

Pro začátek je důležité si uvědomit, že se regulární výraz vyhodnocuje postupně z levé části do pravé a pokud existuje více způsobů, jak řetězec interpretovat, tak se vždy použije největší možná shoda (chová se tzv. **hladově** a snaží se zpracovat co nejvíce znaků).

Chování a strategii zpracování regulárního výrazu lze ovlivnit pomocí přepínačů, kterých je celá řada.

Jednoduché ověření, že je řetězec platný e-mail
-----------------------------------------------

Jak jednoduše zjistit, že je řetězec `jan@barasek.com` platná e-mailová adresa, aniž bychom jej museli složitě rozdělovat na části, nebo procházet znak po znaku?

Regulurní výrazy na to dávají odpověď (uvedený výraz je hodně zjednodušený pro potřeby příkladu a reálná implementace validace e-mailové adresy by měla být o něco složitější):

```php
$mail = 'jan@barasek.com';
$regex = '/^.+@.+\.(cz|sk|com)$/';

if (preg_match($regex, $mail)) {
   echo 'E-mail je validní';
} else {
   echo 'E-mail je nevalidní';
}
```

Pojďme si výraz `/^.+@.+\.(cz|sk|com)$/` rozebrat o něco podrobněji:

Nejprve je nutné celý výraz obalit do dvojice znaků `/` (na začátku a na konci), které říkají, kde výraz začíná a kde končí. Za `/` na konci výrazu se dále zapisují případné modifikátory (nastavení režimu zpracování výrazu).

Při zpracování výrazu se postupuje od levé části znak po znaku. Každý má svůj význam, který říká následující tabulka:

| Znak | Význam          | Popis | Příklad |
|------|-----------------|-------|-------------
| `^`  | Začátek řetězce | Vynutí, že na tomto místě musí řetězec začínat. | Vynutí, že řetězec musí začínat sekvencí `+420` (vhodné třeba pro validaci čísla): `/^\+420/`. |
| `$`  | Konec řetězce nebo řádku | Vynutí, že zde musí končit řetězec nebo řádek. Zajištění konce řádku se pak dělá přes `\z`. <a href="https://phpfashion.com/vite-co-znamena-v-regularnim-vyrazu">Podrobné vysvětlení</a>. | Název souboru musí být textový soubor (končít tečkou a pak řetězcem "txt"): `/\.txt$/`. |
| `.`  | Jakýkoli znak   | Odchytí úplně jakýkoli znak. | Ověří, že řetězec obsahuje právě jeden jakýkoli znak: `/^.$/`. |
| `\d` | Číslo           | Odchytí znaky `0-9` | Odchytí telefonní číslo, které neobsahuje mezery a má 9 cifer: `/^(\+420)?\d{9}$/`. |
| `\s` | Bílý znak       | Odchytí mezery, odřádkování a tabulátory. | Povolí mezi číslicemi v telefonním čísle mít mezery v trojčíslí: `/^(\d{3}\s?){3}$/`. |
| `+`  | Více znaků, ale minimálně jeden | Opakuje předchozí podvýraz a snaží se odchytit co největší kus. Podvýraz se musí opakovat minimálně jednou. | Odchytí co nejvíc číslic bude možné, minimálně však jedno: `/\d+/`. |
| `*`  | Více znaků, může být i žádný | Funguje stejně jako znak `+`, akorát umožňuje odchytit i prázdný řetězec (nemusí se hodnota vyskytovat). | Odchytí co nejvíc číslic bude možně, pokud žádné nebude existovat, odchytí prázdný řetězec: `/\d*/`. |
| `(` `)` | Závorky      | Označují podvýraz. Tímto je možné uzavřít více různých značek a pak nad nimi vyžadovat například opakování, nebo odchytit jejich obsah do proměnné. | Rozdělíme si PSČ na 2 části podle mezery, která je nepovinná a může se jich tam vyskytovat dokonce více: `/^(\d{3})\s*(\d{2})$/` |
| `\|` | Nebo            | Obsahuje podvýraz, nebo jiný podvýraz. | Číslo začíná na `+420` nebo `+421`: `/^\+(420\|421)\s*\d+$/`. |
| `\.` | Escapování      | Pokud chceme ve výrazu odchytit znak, který má jinak speciální význam, musíme jej oescapovat stejně, jako se escapují například řetězce v PHP. | Odchytí dvojici čísel oddělených tečkou (pokud bychom neescapovali tečku, tak by byla pochopena jako "libovolný znak"): `/\d+\.\d+/`. |

Jenom pro úplnost uvedu kompletní podobu validačního pravidla pro e-maily tak, jak jej implementuje Nette:

```php
/**
 * Finds whether a string is a valid email address.
 * @param  string
 * @return bool
 */
public static function isEmail($value)
{
   $atom = "[-a-z0-9!#$%&'*+/=?^_`{|}~]"; // RFC 5322 unquoted characters in local-part
   $alpha = "a-z\x80-\xFF"; // superset of IDN
   return (bool) preg_match("(^
      (\"([ !#-[\\]-~]*|\\\\[ -~])+\"|$atom+(\\.$atom+)*)  # quoted or unquoted
      @
      ([0-9$alpha]([-0-9$alpha]{0,61}[0-9$alpha])?\\.)+    # domain - RFC 1034
      [$alpha]([-0-9$alpha]{0,17}[$alpha])?                # top domain
   \\z)ix", $value);
}
```

`preg_match()` - ověření podle vzoru
------------------------------------

Základní funkce pro ověřování formátu a parsování je `preg_match()`, má 2 povinné parametry a třetím lze určit výstupní pole.

Příklad:

```php
$psc = '272 01'; // Kladno

if (preg_match('/^(\d{3})\s*(\d{2})$/', $psc, $parser)) {
   echo 'PSČ je validní [' . $parser[1] . ', '. $parser[2] . ']';
} else {
   echo 'PSČ je nevalidní';
}
```

Kód vrátí: `PSČ je validní [272, 01]`.

Všimněte si jednotlivých závorek, pomocí kterých jsme výraz rozdělili na několik menších částí. Díky tomu je pak možné získat jednotlivé podvýrazy jako položky pole. Celá funkce poté vrací `true` nebo `false` podle toho, jestli se řetězec podařilo odchytit.

Někdy je ovšem orientace v číselném pořadí závorek velice náročné, protože se může počet změnit, nebo jich může být zkrátka mooooc. V takovém případě je možné závorky jednotlivě pojmenovat a pak přistupovat ke klíčům pomocí jejich názvů.

Například:

```php
$phone = '777 123 456';

preg_match('/^(?<operator>\d{3})\s*(?<number>[0-9 ]+)$/', $phone, $parser);

echo $parser['operator']; // vrátí 777
```

`preg_replace()` - nahrazení podle vzoru
----------------------------------------

Pomocí regexů je dále možné řetězce nahrazovat, což je obzvlášť užitečné při různých opravách formátu po uživateli.

Dejme tomu, že chceme do integeru uložit uživatelem vložené telefonní číslo, protože to vyžadujte knihovna třetí strany, ale uživatelé jej mohou vložit v docela dost divokých formátech.

V takovém případě se držím poučky:

> "Buď velkorysý v tom, co přijímáš, a striktní v tom, co odesíláš"

Proto si formát automaticky přizpůsobíme. Nejprve využijeme rozparsování řetězce na jednotlivé části a poté jej podle čísel závorek zase složíme:

```php
function formatPhoneNumber($phoneNumber) {
   return (int) preg_replace(
      '/^(\+\d{3})\s*(\d{3})\s*(\d{3})\s*(\d{3})$/',
      '$2$3$4',
      $phoneNumber
   );
}

echo formatPhoneNumber('+420 777 123 456');
```

Sestavení řetězce dle regulárního výrazu
----------------------------------------

Regexy dávají také skvělý smysl při generování nových řetězců dle složitého vzoru.

Čisté PHP pro toto nemá žádnou oporu, ale můžeme si stáhnout knihovnu třetí strany <a href="https://github.com/icomefromthenet/ReverseRegex">ReverseRegex</a>, co toto dokáže.

Můžeme chtít například na základě regexu `[a-z]{10}` vygenerovat sadu hesel a nic nám v tom nebude bránit:

```
jmceohykoa
aclohnotga
jqegzuklcv
ixdbpbgpkl
kcyrxqqfyw
jcxsjrtrqb
kvaczmawlz
itwrowxfxh
auinmymonl
dujyzuhoag
vaygybwkfm
```

Použití je následující:

```php
use ReverseRegex\Lexer;
use ReverseRegex\Random\SimpleRandom;
use ReverseRegex\Parser;
use ReverseRegex\Generator\Scope;

require 'vendor/autoload.php';

$lexer = new  Lexer('[a-z]{10}');
$gen   = new SimpleRandom(10007);
$result = '';

$parser = new Parser($lexer, new Scope(), new Scope());
$parser->parse()->getResult()->generate($result, $gen);

echo $result;
```

Já si tímto způsobem generuji například matematické příklady v Nette v Presenteru a je to možné s opravdovou lehkostí:

```php
public function actionRegex()
{
   $lexer = new Lexer('\d{1,3}[\+\-\*\/]');
   $parser = new Parser($lexer, new Scope(), new Scope());
   for ($i = 0; $i <= 10; $i++) {
      $result = '';
      $gen = new SimpleRandom($i);
      $parser->parse()->getResult()->generate($result, $gen);
      dump($result);
   }
   $this->terminate();
}
```

Důležité pro knihovnu je to, že pro stejný vstup generuje stále stejný výstup (i přesto, že by se mohlo zdát, že pro každý regulární výraz může odpovídat mnoho možných řetězců). Pokud chceme měnit vygenerovaný výraz náhodně, je potřeba měnit také `seed`, podle kterého se výstupní řetězec generuje. K tomu se hodí buď procházení intervalu seedů, nebo třeba funkce `rand(1, 1e6)`.

Odchytávání chyb a zpracováné
-----------------------------

V PHP je odchytávání chyb v regexech celkem peklo, ale i tak existuje řešení.

Podrobně to vysvětluje článek <a href="https://phpfashion.com/zradne-regularni-vyrazy-v-php">Zrádné regulární výrazy v PHP</a> od Davida Grudla.
