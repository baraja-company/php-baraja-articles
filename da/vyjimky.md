Undtagelser og opfangning af dem i PHP
======================================

> id: '61b0f178-bb1c-4166-9e8a-af49de2e2a8c'
> slug:
> 	cs: vyjimky
> 	da: undtagelser-og-opfangning-af-dem-i-php
> 
> publicationDate: '2020-02-16 22:18:18'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: d843fe11a092943db429cb8a28384a31

Undtagelser er værktøjerne i objektorienteret programmering, som giver en elegant måde at kaste og håndtere (behandle) programfejl på.

En undtagelse bliver først kastet (`thrown`), behandlet (`try`) og fanget (`catch`). Kun kastning er obligatorisk.

Filosofi for generering af undtagelser
-------------------------

Før fremkomsten af undtagelser var fejlhåndtering i programmering meget kompliceret, fordi vi var nødt til at stole på funktionernes returværdier og opfange dem på vores egen måde og opføre os i overensstemmelse hermed.

Faktisk håndhæver funktioner i sig selv ikke fejlhåndtering, hvilket kan føre til fatale problemer, men David har skrevet om dette i <a href="https://phpfashion.com/programatori-chyby-neignoruji">Programmører ignorerer ikke fejl</a>.

Et eksempel på glemt fejlhåndtering:

```php
// flytning fra disk til disk
copy('c:/oldfile', 'd:/ny fil');
unlink('c:/oldfile');
// hvis den første operation mislykkes, slettes filen uigenkaldeligt
```

Det skyldes, at den korrekte måde at håndtere output fra funktionen `copy()` på er ikke at fortsætte, men at der opstår en fejl. I tilfælde af gode gamle funktioner kan det se således ud:

```php
function backup(): bool
{
   if (copy('c:/oldfile', 'd:/ny fil')) {
      return unlink('c:/oldfile');
   }

   return false;
}
```

Vores `backup()`-funktion returnerer kun `true`, hvis `copy()`-funktionen ikke fejlede, og `unlink()`-funktionen returnerede `true`. Ellers returneres `false`.

Men er det nu sikkert for anvendelsen? Det er det ikke! For nu skal vi behandle output fra funktionen `backup()` på det tidspunkt, hvor vi kalder den, og hvis den fejler, ved vi ikke engang hvorfor. Kort sagt vil den returnere `false`, og vi skal selv opdage fejlen på en eller anden måde. I dette tilfælde er det vel godt at se, at programmører ofte opgiver fejlhåndtering eller simpelthen glemmer at håndtere noget, og at programmet derfor kaster fejl, der er svære at opdage.

Løsningen på dette problem er at bruge undtagelser, der tvinger behandlingen frem, og hvis de ikke behandles, går programmet helt ned, og vi finder altid ud af hvorfor.

Grundlæggende definition af undtagelser
--------------------------

I PHP er en undtagelse en særlig form for grænseflade, som implementeres af den native `Exception`-klasse, som vi vil bruge.

Hvis behandlingen af en del af programmet mislykkes, kaster vi simpelthen en undtagelse, der beskriver problemet:

```php
if (copy('c:/oldfile', 'd:/ny fil') === false) {
   throw new \Exception('Kan ikke kopiere filen "oldfile".');
}
```

En undtagelse kastes med nøgleordet `throw`, hvorefter der oprettes en instans af klassen med undtagelsen. Vi kan også få en instans på andre måder (f.eks. ved at overdrage den fra en variabel), og det at oprette en undtagelsesinstans medfører ikke, at den bliver kastet.

Det første argument i `\Exception`-klassens konstruktør accepterer teksten til undtagelsen, som skal forklare kortfattet, hvad der lige er sket. Det er god praksis også at medtage oplysninger om den operation, der udføres, og en henvisning til dataene. Hvis en filkopiering f.eks. er mislykkedes, er det god praksis at videregive filnavnet. Hvis SQL-forespørgselsafviklingen mislykkes, sender vi igen den forespørgsel, der udføres, videre. Dette vil hjælpe os meget senere, når vi skal håndtere fejl, fordi vi kan se præcis, hvad problemet er.

Håndtering af undtagelser
-----------------

Lad os f.eks. have en funktion `backup()`, der sikkerhedskopierer data og kan kaste et par fejl:

```php
function backup(): void
{
   if (copy('c:/oldfile', 'd:/ny fil')) {
      if (unlink('c:/oldfile') === false) {
         throw new \Exception('Kan ikke fjerne gammel fil.');
      }
   }

   throw new \Exception('Kan ikke kopiere backup-filer.');
}
```

Bemærk, at funktionen ikke returnerer noget output, og at vi angiver typen `void` i definitionen. Funktionen behøver ikke at returnere noget, fordi succes betragtes som en tilstand, hvor der ikke er nogen fejl, og vi behøver ikke at behandle et positivt scenario.

Hvis vi skulle bruge funktionen i en applikation uden behandling, f.eks. på følgende måde:

```php
echo 'Sikkerhedskopiering af filer...';
backup();
echo 'Sikkerhedskopiering afsluttet.';
```

Det er den normale måde, det vil fungere på. Men hvis der opstår en fejl, afsluttes scriptet automatisk, og undtagelsesteksten vises på output. Det er vigtigt, at den ikke fortsætter med at udføre koden, og vi ved, at der ikke vil ske nogen datakorruption.

Hvis vi ønsker at fortsætte udførelsen, skal vi **rense** fejlen, hvilket vi gør ved at bruge `try` og `catch`-konstruktionerne:

```php
echo 'Sikkerhedskopiering af filer...';
try {
   backup();
} catch (\Exception $e) {
   echo 'Backup mislykkedes:' . $e->getMessage();
}
echo 'Sikkerhedskopiering afsluttet.';
```

Hvis en undtagelse bliver kastet, kaldes koden i området `catch()` (som accepterer undtagelsen, hvis den passer til datatypen), og den interne kode udføres.

Vi får altid en instans af undtagelsesklassen, som f.eks. kan bruges til at vise en fejlmeddelelse, som håndteres af metoden `getMessage()`. Det er også nyttigt at kende metoden `getFile()`, som returnerer diskstien til den fil, der indeholder fejlen, `getCode()`, som returnerer fejlstatuskoden, og `getLine()`, som returnerer linjenummeret, hvor undtagelsen blev udløst.

Forberedte undtagelser
------------------------

Ud over den grundlæggende undtagelse `\Exception` indeholder PHP andre foruddefinerede undtagelsestyper, som er velegnede til forskellige brugssituationer.

| Datatype | Forklaring |
|------------|-----------|
| `LogicException` | Logisk fejl, forudsigelig ved programdesign |
| `BadFunctionCallException` | Fejl ved funktionsopkald; funktion ikke fundet; opkald ikke tilladt |
| `BadMethodCallException` | Fejl ved metodeopkald |
| `InvalidArgumentException` | Dårligt (ugyldigt) argument overført til funktion eller metode |
| `OutOfRangeException` | Værdi uden for rækkevidde af array eller samling |
| `LengthException` | Værdien overstiger den tilladte længde |
| `DomainException` | Værdien falder ikke inden for det krævede domæne eller område |
| `RuntimeException` | Fejl, der kun kan opdages ved kørselstid (f.eks. manglende skrivning til en fil) |
| `OverflowException` | Overløb af buffer eller aritmetisk operation, ofte forårsaget af behandling af flere data end forventet |
| `UnderflowException` | Underflow af en buffer eller aritmetisk operation, der blev overført færre data end forventet |
| `OutOfBoundsException` | Indeks uden for rækkevidde af array eller samling |
| `RangeException` | Værdien ligger ikke inden for det ønskede område |
| `UnexpectedValueException` | Uventet værdi (f.eks. returværdi af en funktion) |

Undtagelserne `LogicException` og `RuntimeException` bør forhindres ved hjælp af korrekt programdesign. Personligt bruger jeg dem kun i undtagelsestilfælde, f.eks. hvis det ikke lykkes at skrive til en fil eller kommunikere med en ekstern tjeneste.

Jeg anbefaler, at du slet ikke fanger `RuntimeException` og lader programmet fejle. Dette er normalt et alvorligt problem, som bør indberettes så hurtigt som muligt.
