Hur man namnger variabler, funktioner, metoder och klasser
==========================================================

> id: f70ac75d-422f-4696-88d5-9e1b843e060a
> slug:
> 	cs: jak-pojmenovat-veci-v-php
> 	sv: hur-man-namnger-variabler-funktioner-metoder-och-klasser
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1'
> sourceContentHash: '10510114a0160c0826c9ee1f54c95b03'

För att hålla ordning i koden är det viktigt att välja tydliga regler för hur vi tar fram namn. Den här sidan är en översikt över de relativt populära tillvägagångssätt som används av ett stort antal programmerare, inklusive mig själv och personer som jag arbetar med.

Om du arbetar i ett utvecklingsteam kan du absolut använda deras regler, men för allmän utveckling är det lika bra att skapa några goda vanor.

Eftersom begreppet hela PHP-syntaxen är väldigt brett har jag delat upp guiden i många kategorier som är mycket specialiserade.

Om du letar efter en snabb lösning rekommenderar jag att du studerar <a href="https://www.php-fig.org/psr/psr-4/">SPR-4-standarden</a>.

Infoga skript, länka till HTML, ladda filer
---------------------------------------------------

Varje skript måste börja med taggen `<?php`.

Om det inte finns **utan HTML** i slutet av filen ska den inte avslutas (för att förhindra att ett vitt tecken visas i slutet av sidan).

Laddning av andra filer bör följa följande regler:

- Om filen inte är kritisk och kan tas bort vid rendering av sidan (t.ex. ytterligare information i sidfoten) bör den laddas via include: `include 'file.php';`
- Kritiska filer endast via require-konstruktionen: `require 'file.php';`
- Om vi arbetar med objekt använder vi require-konstruktionen för att ladda autoloadern, som själv tar hand om laddningen av de andra klasserna (de flesta ramverk implementerar detta beteende).


Om det åtminstone är lite möjligt bör vi separera logiken för datahämtning från HTML-återgivningen, dvs. använda MVC-modellen (model - view - controller).

Vi förbereder alltså först data i till exempel Presenter:

`Presenter.php`

```php
$cisla = [1, 2, 3];

$data = [];
$data['nummer'] = $cisla; // överföra data till mallen
include 'renderCisel.php'; // ladda in mallen
```

Sedan ritar du den i mallen:

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

Detta tillvägagångssätt används av de flesta ramverk och är användbart för att öka kodens tydlighet. Under den senare delen av utvecklingsprocessen bör detta tillvägagångssätt användas av alla erfarna programmerare som vill utveckla tydligt strukturerade program (för stora program är detta tillvägagångssätt ett måste).

Namn på variabler
----------------

Om en variabel innehåller en array av värden eller andra objekt ska den namnges i plural:

```php
$numbers = [1, 2, 3];
```

Då kan vi helt enkelt iterera värdena med ett enda nummer:

```php
foreach ($numbers as $number) {
    // Behandling av siffror
}
```

Ett namn som består av flera ord kombineras till ett långt ord med cameCase-syntaxen, dvs. det första ordet börjar med en liten bokstav och varje efterföljande ord med en stor bokstav:

```php
$promenna = 'Hallå! Hallå!';
$seznamUzivatelu = [
   'Jan Barášek',
   'Barack Obama',
   'Steve Jobs',
   'Stephen Wolfram',
];
$maxFilesInDirectory = 12;
$nameOfPhpScript = 'index.php'; // PHP-förkortningen har minskat i storlek
```

Namn på funktioner och metoder
--------------------

Funktioner och metoder ska alltid ange tydligt i sitt namn vad de gör. Ofta kan de förväntade inparametrarna och returvärdet också inkluderas i namnet.

Försök att gissa vad följande funktioner gör och vad de returnerar:

```php
getUserById($id);
saveErrorToLog($message);
createDefaultDirectory($path);
setAuthors(['Jan Barášek', 'Chuck Norris']);
getCurrentTime();
```

Hela tricket ligger i det första ordet i namnet, som gör det tydligt vilken metod som funktionen kommer att använda. Följande konvention brukar följas:

- `get` - hämtar data som en matris eller ett objekt, ingångsparametrarna anger den enhet som söks.
- `save` - sparar till en fil eller en databas
- `create` - skapar en enhet (t.ex. en instans av ett objekt).
- `set` - sparar data till en fördefinierad variabel (i en funktion).

Klassnamn
----------

En klass är en stor enhet som innehåller ett stort antal egenskaper och metoder, så den bör också börja med en stor bokstav. En klass ska också bara innehålla en enhet (och beskriva dess egenskaper), så den ska namnges med ett singulärt namn. Om vi behöver arbeta med flera enheter kan vi bara lagra varje instans i en array.

Exempel:

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

Klassen **User** är specialiserad på information om endast en specifik användare. Om vi vill arbeta med flera användare skapar vi en annan klass (envelope) som innehåller en rad instanser av en viss enhet.

Fabriker kan också ofta vara användbara för detta, eftersom de gör det möjligt att enkelt skapa liknande objekt och återanvända de ursprungliga instanserna, vilket leder till tydligare kod samtidigt som systemresurser sparas.

Namnområde - Namnområden
---------------------------

Namespace är oberoende av den fysiska katalogen där skriptet finns tillgängligt, men det är bra att åtminstone delvis respektera projektets layout (vilket leder till ett bättre system för att skapa nya namn som är mer entydigt på detta sätt).

Personligen namnger jag namnområden enligt den gemensamma underkatalogen för klasser av den typen.

Exempel:

```php
App\Presenters; // Här är namnen på alla föredragshållare
App\Model;      // Detta är namnet på den allmänna modellen
App\Model\Math; // Detta är namnet på den modell som arbetar med matematik.
```

För korrekt autoladdning av klasser är det en bra idé att följa <a href="https://jakpsatphp.cz/PSR4/">PSR-4-standarden</a>.

Anpassade konventioner (t.ex. i ett företagsteam)
-----------------------------------------

Och vad kallar du dina för? Jag skulle uppskatta tips om hur jag kan förbättra den här artikeln.

Generellt sett är det dock inte särskilt klokt att använda egna konventioner inom ett team, eftersom det gör det svårare att anpassa koden till andra ramverk och du måste lära dig de aktuella konventionerna när du anställer en ny kollega. Det är därför bäst att följa standarden `PSR-4`.
