Regulárne výrazy v jazyku PHP
=============================

> id: '32142161-6ace-4cfd-b472-b48031219e9b'
> slug:
> 	cs: regex
> 	sk: regularne-vyrazy-v-jazyku-php
> 
> perex: 'Regulární výrazy a jejich kompletní vysvětlení. Hledání podřetězce, pokročilé nahrazování a generování řetězců.'
> publicationDate: '2020-03-08 13:37:38'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '392c8f14e6943dd8f345a9a134e0de92'

Regulárne výrazy sú nástroje, ktoré umožňujú jednoducho vyhľadávať, overovať, porovnávať, rozdeľovať, zlučovať a nahrádzať reťazce podľa masky (vzoru). Je to veľmi výkonný a elegantný nástroj na pokročilú manipuláciu s reťazcami.

Maska
-----

Na začiatku musíme najprv vymyslieť skutočný regulárny výraz, ktorý budeme vykonávať. Zadáva sa ako textový reťazec, ktorý má množstvo pravidiel a konfiguračných možností (ide o veľmi zložitú techniku).

Na začiatok je dôležité poznamenať, že regulárny výraz sa vyhodnocuje postupne zľava doprava a ak existuje viacero spôsobov interpretácie reťazca, vždy sa použije najväčšia možná zhoda (správa sa **hladovo** a snaží sa spracovať čo najviac znakov).

Správanie a stratégiu spracovania regulárneho výrazu možno ovplyvniť prepínačmi, ktorých je veľa.

Jednoduché overenie, či je reťazec platný e-mail
-----------------------------------------------

Ako môžeme jednoducho skontrolovať, či je reťazec `jan@barasek.com` platnou e-mailovou adresou bez toho, aby sme ho museli rozdeľovať na zložité časti alebo ho prechádzať znak po znaku?

Odpoveď poskytujú regulárne výrazy (uvedený výraz je na účely príkladu veľmi zjednodušený a skutočná implementácia overovania e-mailovej adresy by mala byť o niečo zložitejšia):

```php
$mail = "jan@barasek.com;
$regex = '/^.+@.+\.(en|en|com)$/';

if (preg_match($regex, $mail)) {
   echo "E-mail je platný;
} else {
   echo "E-mail nie je platný;
}
```

Pozrime sa na výraz `/^.+@.+\.(en|en|com)$/` trochu podrobnejšie:

Najprv musíme celý výraz obaliť dvojicou znakov `/` (na začiatku a na konci), ktoré určujú, kde výraz začína a kde končí. Za znakom `/` na konci výrazu nasledujú všetky modifikátory (nastavenia režimu spracovania výrazu).

Pri spracovaní výrazu postupujete z ľavej strany znak po znaku. Každý z nich má svoj vlastný význam, ktorý je uvedený v nasledujúcej tabuľke:

| Znak | Význam | Popis | Príklad |
|------|-----------------|-------|-------------
| `^` | Začiatok reťazca | Vynucuje, že reťazec musí začínať v tomto bode. | Vynucuje, že reťazec musí začínať postupnosťou `+420` (užitočné napríklad pri overovaní čísel): `/^+420/` |
| `$` | Koniec reťazca alebo riadku | Vynucuje, že reťazec alebo riadok musí končiť na tomto mieste. Koniec riadku sa potom potvrdí pomocou `\z`. <a href="https://phpfashion.com/vite-co-znamena-v-regularnim-vyrazu">Podrobné vysvetlenie</a>. | Názov súboru musí byť textový súbor (ukončený bodkou a potom reťazcom "txt"): `/\.txt$/`. |
| `.` | Ľubovoľný znak | Zachytí úplne ľubovoľný znak. | Overí, či reťazec obsahuje presne jeden ľubovoľný znak: `/^.$/`. |
| `\d` | Číslo | Rozpozná znaky `0-9` | Rozpozná telefónne číslo, ktoré neobsahuje medzery a má 9 číslic: `/^(\+420)?\d{9}$/``. |
| `\s` | Biele medzery | Zachytenie medzier, pomlčiek a tabulátorov. | Povolenie medzier medzi číslicami v trojčíslí: `/^(\d{3}\s?){3}$/``.
| `+` | Viac znakov, ale aspoň jeden | Opakuje predchádzajúci podvýraz a snaží sa zachytiť čo najviac znakov. Podvýraz sa musí opakovať aspoň raz. | Zachytí čo najviac číslic, ale aspoň jednu: `/\d+/`. |
| `*` | Viac znakov, môže byť aj žiadny | Funguje rovnako ako `+`, ale umožňuje zachytiť prázdny reťazec (hodnota nemusí byť prítomná). | Zachytí čo najviac číslic, ak žiadna neexistuje, zachytí prázdny reťazec: `/\d*/`.
| `(` `)` | Zátvorky | Označuje podvýraz. To možno použiť na uzavretie niekoľkých rôznych značiek a potom vyžadovať napríklad ich opakovanie alebo zachytenie ich obsahu v premennej. | Rozdeľme poštové smerovacie číslo na 2 časti podľa medzery, ktorá je nepovinná a môže ich byť aj viac ako jedna: `/^(\d{3})\s*(\d{2})$/` |
| `\|` | Alebo | Obsahuje podvýraz alebo iný podvýraz. | Číslo začínajúce na `+420` alebo `+421`: `/^+(420\|421)\s*\d+$/`. |
| `\.` | Escapovanie | Ak chceme vo výraze zachytiť znak, ktorý má inak špeciálny význam, musíme ho escapovať rovnako ako napríklad reťazce v PHP. | Zachytí dvojicu čísel oddelených bodkou (ak by sme bodku neescapovali, chápali by sme ju ako "akýkoľvek znak"): `/\d+\.\d+/`.

Len pre úplnosť uvediem úplnú podobu pravidla validácie pre e-mail, ako ho implementovala spoločnosť Nette:

```php
/**
 * Zistí, či je reťazec platná e-mailová adresa.
 */
public static function isEmail(string $value): bool
{
   $atom = "[-a-z0-9!#$%&'*+/=?^_`{|}~]"; // RFC 5322 necitované znaky v lokálnej časti
   $alpha = "a-z\x80-\xFF"; // nadmnožina IDN
   return (bool) preg_match("(^
      (\"([ !#-[\\]-~]*|\\\\[ -~])+\"|$atom+(\\.$atom+)*)  # quoted or unquoted
      @
      ([0-9$alpha]([-0-9$alpha]{0,61}[0-9$alpha])?\\.)+    # domain - RFC 1034
      [$alpha]([-0-9$alpha]{0,17}[$alpha])?                # top domain
   \\z)ix", $value);
}
```

`preg_match()` - overenie podľa vzoru
------------------------------------

Základnou funkciou pre validáciu a parsovanie formátu je `preg_match()`, má 2 povinné parametre a tretí môže byť použitý na špecifikáciu výstupného poľa.

Príklad:

```php
$psc = '272 01'; // Kladno

if (preg_match('/^(\d{3})\s*(\d{2})$/', $psc, $parser)) {
   echo 'PSČ je platné [' . $parser[1] . ', '. $parser[2] . ']';
} else {
   echo "Poštové smerovacie číslo nie je platné;
}
```

Kód vráti: `Kód je platný [272, 01]`.

Všimnite si jednoduché zátvorky, ktorými sme výraz rozdelili na niekoľko menších častí. To potom umožňuje získať jednotlivé podvýrazy ako položky poľa. Celá funkcia potom vráti `true` alebo `false` podľa toho, či bol reťazec úspešne zachytený.

Niekedy je však orientácia v číselnom poradí zátvoriek veľmi náročná, pretože sa môže zmeniť ich počet alebo ich môže byť jednoducho príliš veľa. V tomto prípade je možné zátvorky pomenovať jednotlivo a potom pristupovať ku kľúčom pomocou ich názvov.

Napríklad:

```php
$phone = '777 123 456';

preg_match('/^(?<operátor>\d{3})\s*(?<číslo>[0-9 ]+)$/', $phone, $parser);

echo $parser["operátor]; // vrátil 777
```

`preg_replace()` - nahradenie podľa vzoru
----------------------------------------

Reťazce je možné nahradiť aj pomocou regexu, čo je užitočné najmä pri rôznych opravách formátu po používateľovi.

Predpokladajme, že chceme uložiť telefónne číslo zadané používateľom v celočíselnom tvare, pretože to vyžaduje knižnica tretej strany, ale používatelia ho môžu zadávať v dosť divokých formátoch.

V takom prípade sa držím diktátu:

> "Buďte štedrí v tom, čo prijímate, a prísni v tom, čo posielate".

Preto automaticky upravujeme formát. Najprv pomocou rozboru rozdeľujeme reťazec na jednotlivé časti a potom ho zložíme späť podľa čísel zátvoriek:

```php
function formatPhoneNumber(string $phoneNumber): int
{
   return (int) preg_replace(
      '/^(\+\d{3})\s*(\d{3})\s*(\d{3})\s*(\d{3})$/',
      '$2$3$4',
      $phoneNumber
   );
}

echo formatPhoneNumber('+420 777 123 456');
```

Konštrukcia reťazca podľa regulárneho výrazu
----------------------------------------

Regexy majú veľký zmysel aj pri generovaní nových reťazcov podľa zložitého vzoru.

Čisté PHP to nepodporuje, ale môžeme si stiahnuť knižnicu tretej strany <a href="https://github.com/icomefromthenet/ReverseRegex">ReverseRegex</a>, ktorá to dokáže.

Môžeme napríklad chcieť vygenerovať sadu hesiel na základe regexu `[a-z]{10}` a nič nás nezastaví:

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

Použitie je nasledovné:

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

Takto napríklad generujem svoje matematické príklady v programe Nette v aplikácii Presenter a je to možné naozaj ľahko:

```php
public function actionRegex(): void
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

Dôležité pre knižnicu je, že pre ten istý vstup generuje stále rovnaký výstup (aj keď sa môže zdať, že pre každý regulárny výraz existuje veľa možných reťazcov). Ak chceme náhodne zmeniť generovaný výraz, musíme zmeniť aj `seed`, pomocou ktorého sa generuje výstupný reťazec. Na tento účel je užitočné buď prechádzanie intervalu semien, alebo funkcia `rand(1, 1e6)`.

Zachytávanie a spracovanie chýb
-----------------------------

V PHP je zachytávanie chýb v regexoch peklo, ale aj tak existuje riešenie.

Podrobne je to vysvetlené v článku <a href="https://phpfashion.com/zradne-regularni-vyrazy-v-php">Dosiahnuteľné regulárne výrazy v PHP</a> od Davida Grudela.
