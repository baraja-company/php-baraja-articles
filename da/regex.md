Regelmæssige udtryk i PHP
=========================

> id: '32142161-6ace-4cfd-b472-b48031219e9b'
> slug:
> 	cs: regex
> 	da: regelmaessige-udtryk-i-php
> 
> perex:
> 	- 'Regulární výrazy a jejich kompletní vysvětlení. Hledání podřetězce, pokročilé nahrazování a generování řetězců.'
> 	- 'Regelmæssige udtryk og deres fuldstændige forklaring. Søgning efter delstrenge, avanceret substitution og generering af strenge.'
> 
> publicationDate: '2020-03-08 13:37:38'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '392c8f14e6943dd8f345a9a134e0de92'

Regelmæssige udtryk er værktøjer, der gør det nemt at søge, validere, sammenligne, opdele, sammenfatte og erstatte strenge i henhold til en maske (mønster). Det er et meget kraftfuldt og elegant værktøj til avanceret strengmanipulation.

Maske
-----

I begyndelsen skal vi først finde frem til det regulære udtryk, som vi vil udføre. Den indtastes som en tekststreng, som har en masse regler og konfigurationsmuligheder (dette er en meget kompleks teknik).

Til at begynde med er det vigtigt at bemærke, at det regulære udtryk evalueres sekventielt fra venstre til højre, og hvis der er flere måder at fortolke strengen på, bruges altid den størst mulige match (det opfører sig **hungrende** og forsøger at behandle så mange tegn som muligt).

Et regulært udtryks opførsel og behandlingsstrategi kan påvirkes af mange indstillinger.

Simpel kontrol af, at strengen er en gyldig e-mail
-----------------------------------------------

Hvordan kan vi simpelthen kontrollere, at strengen `jan@barasek.com` er en gyldig e-mail-adresse uden at skulle opdele den i komplekse dele eller gennemgå den tegn for tegn?

Regulære udtryk giver svaret (ovenstående udtryk er meget forenklet med henblik på eksemplet, og en rigtig implementering af validering af e-mailadresser bør være lidt mere kompliceret):

```php
$mail = 'jan@barasek.com';
$regex = '/^.+@.+\.(en|en|com)$/';

if (preg_match($regex, $mail)) {
   echo 'E-mail er gyldig';
} else {
   echo 'E-mail er ikke gyldig';
}
```

Lad os undersøge udtrykket `/^.+@.+.+\.(en|en|com)$/` lidt mere detaljeret:

Først skal vi pakke hele udtrykket ind i et par `/`-tegn (i begyndelsen og i slutningen), som fortæller, hvor udtrykket starter og slutter. `/` i slutningen af udtrykket efterfølges af eventuelle modifikatorer (indstillinger for udtryksbehandlingstilstand).

Når du behandler et udtryk, fortsætter du fra venstre side, tegn for tegn. Hver af dem har sin egen betydning, som er angivet i nedenstående tabel:

| Tegn | Betydning | Beskrivelse | Eksempel | Eksempel |
|------|-----------------|-------|-------------
| `^` | Start af streng | Tvinger strengen til at starte på dette sted. | Tvinger strengen til at starte med sekvensen `+420` (f.eks. nyttigt til validering af tal): `/^+420/`. |
| `$` | Slutning af streng eller linje | Tvinger strengen eller linjen til at slutte her. Linjeafslutningen unddrages derefter med `\z`. <a href="https://phpfashion.com/vite-co-znamena-v-regularnim-vyrazu">Detaljeret forklaring</a>. | Filnavnet skal være en tekstfil (sluttende med et punktum og derefter strengen "txt"): `/\.txt$/`. |
| `.` | Ethvert tegn | Fanger præcis ethvert tegn. | Kontrollerer, at strengen indeholder præcis ét tegn af et hvilket som helst tegn: `/^.$/`. |
| `\d` | Nummer | Registrerer tegn `0-9` | Registrerer et telefonnummer, der ikke indeholder mellemrum og har 9 cifre: `/^(\+420)?\d{9}$/`. |
| `\s` | Whitespace | Opfanger mellemrum, bindestreger og tabulatorer. | Tillader mellemrum mellem cifre i et telefonnummer i trecifrede cifre: `/^(\d{3}\s?){3}$/`. |
| `+` | Flere tegn, men mindst ét | Gentager det foregående deludtryk og forsøger at fange så meget som muligt. Deludtrykket skal gentages mindst én gang. | Fanger så mange cifre som muligt, men mindst ét: `/\d+/`. |
| `*` | Flere tegn, kan være ingen | Fungerer på samme måde som `+`, men giver mulighed for at fange en tom streng (værdien behøver ikke at være til stede). | Fanger så mange cifre som muligt, hvis ingen findes, fanger en tom streng: `/\d*/`. |
| `(` ` `)` | Parenteser | Angiver et deludtryk. Dette kan bruges til at omslutte flere forskellige tags og derefter kræve f.eks. gentagelse over dem, eller til at fange deres indhold i en variabel. | Lad os dele postnummeret op i to dele i henhold til mellemrummet, som er valgfrit, og der kan endda være mere end et: `/^(\d{3})\s*(\d{2})$/` |
| `\|` | Eller | Indeholder et underudtryk eller et andet underudtryk. | Tal, der begynder med `+420` eller `+421`: `/^+(420\|421)\s*\d+$/`. |
| `\.` | Escaping | Hvis vi ønsker at fange et tegn i et udtryk, der ellers har en særlig betydning, skal vi escaped det på samme måde som f.eks. strenge i PHP. | Fanger et par tal adskilt af et punktum (hvis vi ikke escapede punktummet, ville det blive opfattet som "ethvert tegn"): `/\d+\d+\d.\d+/`. |

For fuldstændighedens skyld vil jeg give den komplette form af valideringsreglen for e-mail, som den er implementeret af Nette:

```php
/**
 * Finde, om en streng er en gyldig e-mailadresse.
 */
public static function isEmail(string $value): bool
{
   $atom = "[-a-z0-9!#$%&'*+/=?^_`{|}~]"; // RFC 5322 ikke-angivne tegn i den lokale del
   $alpha = "a-z\x80-\xFF"; // overordnet mængde af IDN
   return (bool) preg_match("(^
      (\"([ !#-[\\]-~]*|\\\\[ -~])+\"|$atom+(\\.$atom+)*)  # quoted or unquoted
      @
      ([0-9$alpha]([-0-9$alpha]{0,61}[0-9$alpha])?\\.)+    # domain - RFC 1034
      [$alpha]([-0-9$alpha]{0,17}[$alpha])?                # top domain
   \\z)ix", $value);
}
```

`preg_match()` - validering efter mønster
------------------------------------

Den grundlæggende funktion til formatvalidering og parsing er `preg_match()`, den har 2 obligatoriske parametre, og den tredje kan bruges til at angive outputfeltet.

Eksempel:

```php
$psc = '272 01'; // Kladno

if (preg_match('/^(\d{3})\s*(\d{2})$/', $psc, $parser)) {
   echo 'Postnummeret er gyldigt [' . $parser[1] . ','. $parser[2] . ']';
} else {
   echo 'Postnummeret er ikke gyldigt';
}
```

Koden returnerer: `Kode er gyldig [272, 01]`.

Bemærk de enkelte parenteser, som vi har brugt til at opdele udtrykket i flere mindre dele. Dette gør det muligt at hente de enkelte deludtryk som arrayposter. Hele funktionen returnerer derefter `true` eller `false` afhængigt af, om det lykkedes at registrere strengen.

Nogle gange er det imidlertid meget vanskeligt at finde rundt i parentesernes numeriske rækkefølge, da tallet kan ændre sig, eller der kan simpelthen være for mange af dem. I dette tilfælde er det muligt at navngive parenteserne individuelt og derefter få adgang til nøglerne ved hjælp af deres navne.

For eksempel:

```php
$phone = '777 123 456';

preg_match('/^(?<operator>\d{3})\s*(?<tal>[0-9 ]+)$/', $phone, $parser);

echo $parser['operatør']; // returnerede 777
```

`preg_replace()` - erstatning efter mønster
----------------------------------------

Det er også muligt at erstatte strenge ved hjælp af regex, hvilket er særligt nyttigt til forskellige formatkorrektioner efter brug.

Lad os antage, at vi ønsker at gemme et telefonnummer, som brugeren har indtastet, i et heltal, da dette kræves af et bibliotek fra en tredjepart, men brugerne kan indtaste det i nogle ret vilde formater.

I så fald holder jeg mig til diktummet:

> "Vær gavmild i det, du modtager, og streng i det, du sender".

Derfor tilpasser vi automatisk formatet. Først bruger vi parsing til at bryde strengen op i de enkelte dele, og derefter folder vi den tilbage i overensstemmelse med parentesnumrene:

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

Konstruktion af en streng i henhold til et regulært udtryk
----------------------------------------

Regexer giver også god mening, når du skal generere nye strenge efter et komplekst mønster.

Ren PHP har ingen understøttelse for dette, men vi kan downloade et tredjepartsbibliotek <a href="https://github.com/icomefromthenet/ReverseRegex">ReverseRegex</a>, som kan gøre dette.

Vi kan f.eks. generere et sæt passwords baseret på regexet `[a-z]{10}`, og intet kan stoppe os:

```txt
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

Anvendelsen er som følger:

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

Jeg genererer f.eks. mine matematiske eksempler i Nette i Presenter på denne måde, og det er virkelig nemt:

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

Det vigtige for biblioteket er, at det stadig genererer det samme output for det samme input (selv om det ser ud til, at der er mange mulige strenge at matche for hvert regulært udtryk). Hvis vi ønsker at ændre det genererede udtryk tilfældigt, skal vi også ændre det `seed`, som outputstrengen genereres med. Enten kan du bruge frøintervallet eller måske funktionen `rand(1, 1e6)` til dette formål.

Opfangning og behandling af fejl
-----------------------------

I PHP er det et helvede at opfange fejl i regexes, men der er stadig en løsning.

Dette forklares i detaljer i artiklen <a href="https://phpfashion.com/zradne-regularni-vyrazy-v-php">Treachable regular expressions in PHP</a> af David Grudel.
