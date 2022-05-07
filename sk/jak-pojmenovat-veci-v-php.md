Ako pomenovať premenné, funkcie, metódy a triedy
================================================

> id: f70ac75d-422f-4696-88d5-9e1b843e060a
> slug:
> 	cs: jak-pojmenovat-veci-v-php
> 	sk: ako-pomenovat-premenne-funkcie-metody-a-triedy
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1'
> sourceContentHash: '10510114a0160c0826c9ee1f54c95b03'

Aby bol v kóde poriadok, je dôležité zvoliť jasné pravidlá pre odvodzovanie názvov. Táto stránka slúži ako prehľad pomerne populárnych prístupov, ktoré používa veľké množstvo programátorov vrátane mňa a ľudí, s ktorými spolupracujem.

Ak pracujete vo vývojovom tíme, určite používajte jeho pravidlá, ale pre všeobecný vývoj je stále prospešné vytvoriť si niekoľko dobrých návykov.

Keďže pojem celej syntaxe PHP je naozaj široký, rozdelil som tu príručku do mnohých kategórií, ktoré sú úzko špecializované.

Ak hľadáte rýchle riešenie, odporúčam preštudovať <a href="https://www.php-fig.org/psr/psr-4/">štandard PSR-4</a>.

Vkladanie skriptov, prepojenie na HTML, načítanie súborov
---------------------------------------------------

Každý skript musí začínať značkou `<?php`.

Ak na konci súboru nie je **žiadne HTML**, nemal by byť ukončený (aby sa zabránilo bielemu znaku na konci stránky).

Načítanie ostatných súborov by sa malo riadiť nasledujúcimi pravidlami:

- Ak súbor nie je kritický a môže byť pri vykresľovaní stránky vynechaný (napríklad dodatočné informácie v pätičke), mal by byť načítaný prostredníctvom include: `include 'file.php';`
- Kritické súbory iba prostredníctvom konštrukcie require: `require 'file.php';`
- Ak pracujeme s objektmi, použijeme konštrukciu require na načítanie autoloadera, ktorý sa sám postará o načítanie ostatných tried (väčšina frameworkov implementuje toto správanie).


Ak je to aspoň trochu možné, mali by sme oddeliť logiku získavania údajov od vykresľovania do HTML, t. j. použiť model MVC (model - view - controller).

Najprv teda pripravíme údaje napríklad v aplikácii Presenter:

`Presenter.php`

```php
$cisla = [1, 2, 3];

$data = [];
$data["čísla] = $cisla; // odovzdať údaje šablóne
include 'renderCisel.php'; // načítanie šablóny
```

A potom ho nakreslite do šablóny:

`renderCisel.php`

<table>
<?php
    foreach ($data['cisla'] as $cislo) {
        echo '<tr><td>' . $cislo . '</td></tr>';
    }
?>
</table>

Tento prístup používa väčšina frameworkov a je užitočný na zvýšenie prehľadnosti kódu. V neskoršej časti vývojového procesu by mal tento prístup používať každý skúsený programátor, ktorý chce vyvíjať prehľadné aplikácie (pri veľkých aplikáciách je tento prístup nevyhnutnosťou).

Názvy premenných
----------------

Ak premenná obsahuje pole hodnôt alebo iných objektov, mala by byť pomenovaná v množnom čísle:

```php
$numbers = [1, 2, 3];
```

Pretože potom môžeme jednoducho iterovať hodnoty podľa jedného čísla:

```php
foreach ($numbers as $number) {
    // spracovanie čísla
}
```

Názov zložený z viacerých slov sa spojí do jedného dlhého slova so syntaxou cameCase, t. j. prvé slovo začína malým písmenom, každé ďalšie slovo veľkým písmenom:

```php
$promenna = "Ahoj!;
$seznamUzivatelu = [
   "Jan Barášek,
   "Barack Obama,
   "Steve Jobs,
   'Stephen Wolfram',
];
$maxFilesInDirectory = 12;
$nameOfPhpScript = 'index.php'; // Veľkosť skratky PHP bola zmenšená
```

Názvy funkcií a metód
--------------------

Funkcie a metódy by mali mať vo svojom názve vždy jasne uvedené, čo robia. V názve môžu byť často uvedené aj očakávané vstupné parametre a návratová hodnota.

Skúste uhádnuť, čo robia nasledujúce funkcie a aká je ich návratová hodnota:

```php
getUserById($id);
saveErrorToLog($message);
createDefaultDirectory($path);
setAuthors(["Jan Barášek, "Chuck Norris]);
getCurrentTime();
```

Celý trik spočíva v prvom slove v názve, z ktorého je jasné, akú metódu funkcia použije. Zvyčajne sa dodržiava nasledujúca konvencia:

- `get` - načítanie údajov ako poľa alebo objektu, vstupné parametre určujú hľadanú entitu
- `save` - uloženie do súboru alebo databázy
- `create` - vytvorenie entity (napríklad vytvorenie inštancie objektu)
- `set` - uloženie údajov do vopred definovanej premennej (vo vnútri funkcie)

Názvy tried
----------

Trieda je veľká entita, ktorá obsahuje veľké množstvo vlastností a metód, preto by mala tiež začínať veľkým písmenom. Trieda by tiež mala niesť len jednu entitu (a popisovať jej vlastnosti), preto by mala byť pomenovaná jediným názvom. Ak potrebujeme pracovať s viacerými entitami, môžeme každú inštanciu jednoducho uložiť do poľa.

Príklad:

```php
class User
{
    public string $username;

    public string $password;

    public string $role;
}

class Users
{
    /** @var User[] */
    public array $users;

    public function addUser(User $user): void
    {
        $this->users = array_push($this->users, $user);
    }
}
```

Trieda **User** sa špecializuje na informácie len o jednom konkrétnom používateľovi. Ak chceme pracovať s viacerými používateľmi, vytvoríme ďalšiu triedu (envelope), ktorá nesie pole inštancií konkrétnej entity.

Na tento účel môžu byť často užitočné aj továrne, ktoré nám umožňujú jednoducho vytvárať podobné objekty a recyklovať pôvodné inštancie, čo vedie k prehľadnejšiemu kódu a zároveň šetrí systémové zdroje.

Oblasť názvov - Oblasti názvov
---------------------------

Hoci je oblasť názvov nezávislá od fyzického adresára, v ktorom je skript dostupný, je dobrým zvykom aspoň čiastočne rešpektovať rozloženie projektu (čo vedie k lepšiemu systému vytvárania nových názvov, ktorý je takto jednoznačnejší).

Ja osobne pomenúvam menné priestory podľa spoločného podadresára pre triedy daného typu.

Príklady:

```php
App\Presenters; // Toto sú mená všetkých prezentujúcich
App\Model;      // Toto je názov všeobecného modelu
App\Model\Math; // Toto je názov modelu, ktorý pracuje s matematikou
```

Pre správne automatické načítanie tried je dobré dodržiavať <a href="https://jakpsatphp.cz/PSR4/">štandard PSR-4</a>.

Vlastné konvencie (napríklad vo firemnom tíme)
-----------------------------------------

A ako sa volá ten váš? Uvítam tipy na zlepšenie tohto článku.

Vo všeobecnosti však vlastné konvencie v rámci tímu nemajú veľký zmysel, pretože sa kód ťažšie prenáša na iné frameworky a keď zamestnáte nového kolegu, musí sa naučiť aktuálne konvencie. Preto je najlepšie dodržiavať štandard `PSR-4`.
