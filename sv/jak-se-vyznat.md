Hur man förstår PHP-kod
=======================

> id: d8d452da-37b4-4502-a573-a27afe7ea37a
> slug:
> 	cs: jak-se-vyznat
> 	sv: hur-man-foerstar-php-kod
> 
> publicationDate: '2020-04-11 18:45:23'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: c3012a19e6dd01770fad9ab456a5b6b3

De flesta språk kan skrivas på olika sätt med samma resultat. När du väl har skrivit koden kommer du förmodligen att läsa den någon gång i framtiden och korrigera den eller lägga till nya funktioner.

För att undvika att behöva tänka på koden hela tiden och för att navigera i den på ett bra sätt finns det därför en uppsättning verktyg och sätt att "skriva rätt kod" direkt i PHP, eller att bygga kod på ett sätt som direkt stöder dess framtida läsbarhet (även för en annan människa).

> **Författarens anmärkning:**
>
> Erfarenheten visar att koden föråldras så snabbt att till och med programförfattaren själv upplever sin egen kod som främmande efter ett halvår. Om vi skriver den korrekt från början kommer det inte att hindra den från att utvidgas i framtiden.
>
> I verklig utveckling visar det sig i hemlighet att enhetlig kodformatering och införandet av regler i allmänhet förhindrar ett antal fel.

TL;DR
-----

Lättläst kod har ofta att göra med formatering och skrivregler.

Inom utvecklingsteam är det vettigt att fastställa formella regler för hur vi formaterar och underhåller kod.

Personligen använder jag (2022) kodningsstandarden för Nette-ramverket och reglerna utvärderas automatiskt i varje överföring. Se artikeln [using GitHub CI] (https://php.baraja.cz/github-actions-nejlepsi-ci-pro-rok-2021#hotove-priklady) för mer information.

Installation och körning av kodningsstandardtestet sker med ett par kommandon:

```shell
composer create-project nette/coding-standard temp/coding-standard ^3 --no-progress --ignore-platform-reqs
php temp/coding-standard/ecs check src
```

Anmärkningar i koden
---------------

Anteckningarna har ingen inverkan på kodbehandlingen och är endast avsedda för programmeraren. När det gäller större, mer kompletta kodstycken är det viktigt att skriva en not som förklarar vad koden är till för och hur den fungerar i princip.

```php
// Definitioner av variabler
$a = 5;
$b = 3;
$c = 2;

// Summan av alla tal
$sum = $a + $b + $c;

// Lista för användare
echo $sum;
```

Anteckningen börjar med ett par snedstreck (`///`) och är giltig till slutet av raden. Den kan användas var som helst.

Anteckningen ska inte förklara det specifika genomförandet av algoritmen, utan snarare dess allmänna principer. Detta beror på att koden kan ändras flera gånger med tiden, och i så fall bör vi också korrigera notisen.

> **Författarens anmärkning:**
>
> Det är ofta så att kod inte gör exakt det som beskrivs i beskrivningen. Detta beror främst på att programmeraren har gjort ett misstag någonstans. I anmärkningen bör därför de allmänna principerna beskrivas så att vi kan ändra koden i enlighet med dem. Men glöm aldrig att den enda sanningen om vad som faktiskt händer i ett program beskrivs endast av den faktiska koden, och att anteckningen inte har någon inverkan på den.

Grafisk separering av koddelar
----------------------------

När man utformar ett program är det viktigt att skilja de logiska blocken från varandra. Vanligtvis är de uppdelade i funktioner, metoder eller, när det gäller grundläggande kod, åtminstone i kommentarer.

I en längre algoritm brukar jag först beskriva hela principen för algoritmen i början och sedan numrera de enskilda platserna i koden så att utvecklaren bättre kan förstå den specifika funktionaliteten utifrån dem.

```php
/**
 * Funktionen beräknar det aritmetiska medelvärdet.
 *
 * 1. Få en lista med nummer
 * 2. Få fram summan och antalet tal
 * 3. Beräkna och skriv ut genomsnittet
 */

// 1.
$numbers = [1, 3, 8, 12];

// 2.
$sum = array_sum($numbers);
$count = count($numbers);

// 3.
echo 'Genomsnittet är:' . ($sum / $count);
```

Tecknen `/**` börjar en kommentar på flera rader som gäller fram till markeringen `*/`. För att det ska vara lätt att läsa är det bra att placera en asterisk i början av varje rad.

Kommentarer till dokumentationen
----------------------

Dokumentationskommentarer används vanligtvis för att beskriva och dokumentera funktioner (beteende, parametrar, returvärden, författare osv.).

I tidigare versioner av PHP (före `7.0`) användes inte datatyper ännu, så typen av en viss variabel beskrevs direkt i kommentaren.

```php
/**
 * @författare Jan Barášek <jan@barasek.com>
 * @license MIT
 * @link https://php.baraja.cz
 * @param float[] $numbers
 */
function average(array $numbers): float
{
    $sum = array_sum($numbers);
    $count = count($numbers);

    return $sum / $count;
}
```

Dokumentationskommentarer kallas "dokumentation" främst för att de har ett fördefinierat format som förstås av specifika utvecklingsmiljöer (och redaktörer), men även av automatiserade verktyg för att generera dokumentation eller kontrollera kod.

Skriva kod på tjeckiska eller engelska?
-----------------------------

Jag skriver all kod endast på engelska (inklusive funktionsnamn, variabler, kommentarer, ...).

Detta har flera fördelar:

- Utvecklaren kan träna sin engelska på ett proaktivt sätt direkt.
- En stor del av programmet använder bibliotek från tredje part som är på engelska, så det upprätthåller automatiskt konsistensen.
- De mest avancerade sakerna har ingen engelsk översättning alls.
- Du kan säkert komma på många andra exempel.

PHP kräver inte direkt engelska och du kan skriva allt på engelska. Jag ser användningen av engelska mer som en slags investering för framtiden, och en möjlighet att lätt utvidga koden för andra personer som inte har engelska som modersmål.

Även företag använder helt lokaliserad kod på engelska, så det är bra att öva på engelska redan från början.

Ordning av numeriska operationer
------------------------

Tänk alltid på att PHP avrundar tal när du utför numeriska operationer. Detta kan ofta vara besvärligt, eftersom alla resultat med decimaltal är behäftade med en viss felaktighet.

En bra lösning verkar vara att först öka siffrorna och sedan beräkna med de största möjliga siffrorna. På så sätt blir det statistiskt sett mindre snedvridning.

Exempel:

```php
echo 10 / 3; // Han skriver 3.333333333333333
```

I vissa fall kan du också använda dig av ett knep som går ut på att inte använda decimaler alls och beräkna allt som ett helt tal. I det här fallet finns det ingen sådan snedvridning:

```php
echo 1 / 2 * 2; // Detta är värre eftersom 1/2 = 0,5*2 = 1
echo 2 * 1 / 2; // detta är bättre eftersom 2*1 = 2/2 = 1
```

När du löser stora, komplexa räkneoperationer använder du bråk för att skriva tal.
