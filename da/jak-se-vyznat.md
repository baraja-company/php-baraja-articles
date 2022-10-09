Sådan forstår du PHP-kode
=========================

> id: d8d452da-37b4-4502-a573-a27afe7ea37a
> slug:
> 	cs: jak-se-vyznat
> 	da: sadan-forstar-du-php-kode
> 
> publicationDate: '2020-04-11 18:45:23'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: c3012a19e6dd01770fad9ab456a5b6b3

De fleste sprog kan skrives på forskellige måder med det samme resultat. Når du har skrevet koden, vil du sandsynligvis læse den engang i fremtiden og rette den eller tilføje nye funktioner.

For at undgå at skulle tænke på koden hele tiden og for at kunne navigere godt i den, findes der derfor en række værktøjer og måder at "skrive kode rigtigt" direkte i PHP på, eller at bygge kode på en måde, der direkte understøtter dens fremtidige læsbarhed (selv for et andet menneske).

> **Author's note:**
>
> Erfaringen viser, at kode bliver forældet så hurtigt, at selv forfatteren af programmet selv opfatter sin egen kode som fremmed efter et halvt år. Så hvis vi skriver den korrekt fra starten, vil det ikke forhindre dens fremtidige udvidelsesmuligheder.
>
> I den virkelige udvikling er det hemmeligt vist, at ensartet kodeformatering og indførelse af regler generelt forhindrer en række fejl.

TL;DR
-----

Læsevenlighed af kode er ofte forbundet med formatering og skriveregler.

Inden for udviklingsteams giver det mening at opstille formelle regler for, hvordan vi formaterer og vedligeholder kode.

Personligt bruger jeg (i 2022) kodningsstandarden for Nette-rammen, og reglerne evalueres automatisk i hvert commit. Se artiklen om [brug af GitHub CI] (https://php.baraja.cz/github-actions-nejlepsi-ci-pro-rok-2021#hotove-priklady) for at få flere oplysninger.

Installation og kørsel af kodningsstandardtesten sker med et par kommandoer:

```shell
composer create-project nette/coding-standard temp/coding-standard ^3 --no-progress --ignore-platform-reqs
php temp/coding-standard/ecs check src
```

Noter i koden
---------------

Noterne har ingen indflydelse på behandlingen af koden og er kun til brug for programmøren. For større, mere komplette kodestykker er det vigtigt at skrive en note, der forklarer, hvad koden er beregnet til, og hvordan den i princippet fungerer.

```php
// Definitioner af variabler
$a = 5;
$b = 3;
$c = 2;

// Summen af alle tal
$sum = $a + $b + $c;

// Liste for brugere
echo $sum;
```

Bemærkningen starter med et par skråstreger (`///`) og er gyldig indtil slutningen af linjen. Den kan bruges overalt.

Notatet skal ikke forklare den specifikke implementering af algoritmen, men snarere dens generelle principper. Det skyldes, at koden kan ændres flere gange med tiden, og i så fald bør vi også rette noten.

> **Author's note:**
>
> Det er ofte sådan, at kode ikke gør præcis det, som beskrivelsen forklarer. Det skyldes primært, at programmøren har begået en fejl et eller andet sted. Bemærkningen bør derfor beskrive de generelle principper, så vi kan ændre koden i overensstemmelse hermed. Men glem aldrig, at den eneste sandhed om, hvad der rent faktisk sker i et program, kun beskrives af den faktiske kode, og at noten ikke har nogen indflydelse på dette.

Grafisk adskillelse af kodedele
----------------------------

Når man designer et program, er det vigtigt at adskille de logiske blokke fra hinanden. Normalt er de opdelt i funktioner, metoder eller, i tilfælde af grundlæggende kode, i det mindste i kommentarer.

I en længere algoritme beskriver jeg normalt først hele princippet i algoritmen i begyndelsen og nummererer derefter de enkelte steder i koden, så udvikleren bedre kan forstå den specifikke funktionalitet baseret på dem.

```php
/**
 * Funktionen beregner det aritmetiske gennemsnit.
 *
 * 1. Få en liste over numre
 * 2. Få summen og antallet af tal
 * 3. Beregn og udskriv gennemsnittet
 */

// 1.
$numbers = [1, 3, 8, 12];

// 2.
$sum = array_sum($numbers);
$count = count($numbers);

// 3.
echo 'Gennemsnittet er:' . ($sum / $count);
```

Tegnene `/**` indleder en kommentar på flere linjer, der gælder op til `*/`-markøren. For at gøre det let at læse er det en god idé at sætte en asterisk i begyndelsen af hver linje.

Bemærkninger til dokumentation
----------------------

Dokumentationskommentarer bruges normalt til at beskrive og dokumentere funktioner (adfærd, parametre, returværdier, forfatter osv.).

I tidligere versioner af PHP (før `7.0`) blev datatyper endnu ikke brugt, så typen af en bestemt variabel blev beskrevet direkte i kommentaren.

```php
/**
 * @author Jan Barášek <jan@barasek.com>
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

Dokumentationskommentarer kaldes hovedsagelig "dokumentation", fordi de har et på forhånd aftalt format, som forstås af specifikke udviklingsmiljøer (og editorer), men også af automatiserede værktøjer til at generere dokumentation eller kontrollere kode.

Skriver du kode på tjekkisk eller engelsk?
-----------------------------

Jeg skriver al kode udelukkende på engelsk (herunder funktionsnavne, variabler, kommentarer, ...).

Dette har en række fordele:

- Udvikleren kan træne sine engelsksprogede medarbejdere proaktivt med det samme.
- En stor del af programmet bruger biblioteker fra tredjeparter, der er på engelsk, så det opretholder automatisk konsistensen
- De fleste af de avancerede ting har slet ikke nogen engelsk oversættelse
- Jeg er sikker på, at du kan komme i tanke om mange andre eksempler

PHP kræver ikke direkte engelsk, og du kan skrive alt på engelsk. Jeg ser brugen af engelsk mere som en slags investering i fremtiden og som en mulighed for at udvide koden med andre mennesker, som ikke har engelsk som modersmål.

Komplet lokaliseret kode på engelsk bruges også i virksomheder, så det er godt at øve sig på engelsk fra starten.

Rækkefølge af numeriske operationer
------------------------

Husk altid på, at PHP runder tal, når du udfører numeriske operationer. Dette kan ofte være en gene, da ethvert resultat med decimaltal er ledsaget af en vis unøjagtighed.

En god løsning synes at være først at øge tallene og derefter beregne med de størst mulige tal. På denne måde er der statistisk set mindre forvrængning.

Eksempel:

```php
echo 10 / 3; // Han skriver 3.33333333333333333
```

I nogle tilfælde kan du også bruge det trick, at du slet ikke bruger decimaler og beregner alt som hele tal. I dette tilfælde er der ikke tale om en sådan forvrængning:

```php
echo 1 / 2 * 2; // dette er værre, fordi 1/2 = 0,5*2 = 1
echo 2 * 1 / 2; // dette er bedre, fordi 2*1 = 2/2 = 1
```

Når du løser store, komplekse numeriske operationer, skal du bruge brøker til at skrive tal.
