Tärnära operatörer i PHP (?:) - villkor på en rad
=================================================

> id: '1191be9b-ff9d-4725-b0b4-17b60de2b935'
> slug:
> 	cs: ternarni-operator
> 	sv: taernaera-operatoerer-i-php---villkor-pa-en-rad
> 
> perex:
> 	- Ternární operátor umožňuje zapsat jednoduchou podmínku do jednoho řádku jako výraz.
> 	- Med den ternära operatorn kan du skriva ett enkelt villkor på en rad som ett uttryck.
> 
> publicationDate: '2019-11-26 11:59:18'
> mainCategoryId: f46a0d80-fbe4-4be8-a5e4-04a8d29b0afc
> sourceContentHash: '491dbbbaeceba356a030e2501f5c0e2b'

Med den ternära operatorn kan du förkorta ett enkelt villkor till en enda rad på en plats där analysering är onödig, komplex eller helt olämplig.

TL;DR
------

De ternära operatörerna är förkortningar för villkoren `if` och `else`, så du behöver inte använda dem. De är användbara för ständigt återkommande delar av verifieringslogiken. Använd alltid formatet `(villkor ? positiv logik : negativ logik)` inklusive parenteser. Används för korta verifieringar för att göra koden tydligare och kortare.

Grundläggande definitioner
------------------

Ofta har vi ett villkor av följande form, till exempel:

```php
$a = 5;
$b = 3;

if ($a > $b) {
     echo 'Den är större';
} else {
     echo 'Den är mindre';
}
```

Om vi vill skriva en enkel mening måste vi alltså använda fyra rader kod, vilket skulle kunna minskas till en.

```php
$a = 5;
$b = 3;

echo 'Det är' . ($a > $b ? 'större' : 'mindre');
```

I allmänhet skrivs den ternära operatören med tre delar (det är därför den kallas "ternär"):

```php
(podmínka ? pokud je pravda : pokud není pravda)
```

Ternära operatorer används mycket ofta i praktiken, till exempel för att ange jämna rader i en tabell:

```php
$pole = [3, 1, 4, 1, 5, 9, 2];

for ($i = 0; $pole[$i]; $i++) {
     echo '<tr class=""' . ($i % 2 ? 'från' : 'även') . '">';

          // Det här fungerar på något sätt med ett kalkylblad...
          // till exempel: echo '<td>' . $field[$i] . '</td>';

     echo '</tr>';
}
```

Exempel på användning av den ternära operatorn
------------------------------------

Ofta måste vi kontrollera att det finns ett variabelt värde och använda det omedelbart om det behövs. Om det inte finns, vill vi återge standardvärdet.

Det klassiska tillvägagångssättet är att göra så här:

```php
$a = 5;
$b = 8;

$c = $a ? $a : $b;
```

Om variabeln `$a` finns, använd `$a` som värde, annars `$b`.

Ibland får vi dock värdet från en funktion:

```php
$a = 5;
$b = 3;
$default = 42;

$c = funkce($a, $b) ? funkce($a, $b) : $default;
```

Denna metod är mycket ineffektiv när det gäller systemresurser. Först måste funktionen anropas, och om den finns anropas den igen för att få fram värdet, som lagras i variabeln `$c`.

Detta skulle kunna hanteras bättre med hjälp av en hjälpvariabel:

```php
$a = 5;
$b = 3;
$pomocnaPromenna = funkce($a, $b);
$default = 42;

$c = $pomocnaPromenna ? $pomocnaPromenna : $default;
```

Olämplig användning
------------------

Den ternära operatorn är inte alltid värd att använda eftersom den tenderar att skapa förvirring när du skriver komplicerade och inbäddade villkor.

Se själv ett exempel:

```php
$valid = true;
$lang = 'Franska';

$x = $valid
     ? ($lang === 'Franska' ? 'oui' : 'ja')
     : ($lang === 'Franska' ? 'ej' : 'från');
```

Skulle du veta att variabeln `$x` skulle innehålla värdet `oui`?

Efter lite övning kanske du kan göra det, men det är inte rätt svar. :)

Kontrollera att värdet inte finns (eller inte finns) och använda standardvärdet.
--------------------

De ternära operatörerna är mest kraftfulla när det gäller rutinmässig verifiering av att värden (inte) existerar och användning av andra standardvärden.

Vi vill t.ex. kontrollera om det finns en huvudkategori för en artikel och om så inte är fallet, skicka ut ett ersättningsmeddelande:

```php
echo $mainCategory ?? 'Kategorin finns inte';
```

Operatören `??` (två frågetecken) kontrollerar om variabeln `$mainCategory` finns och inte är `null`. Den fungerar på samma sätt som funktionen `isset()`.

Validering av ett värdens tomhet
-----------------------------

En mycket ofta användbar konstruktion för att kontrollera att utdatavärdet inte är tomt (dvs. inte `null`, `0`, `false` eller `''*(tom sträng)*).

Detta kan skrivas på följande sätt:

```php
$a = 5;
$b = 3;
$default = 42;

$c = funkce($a, $b) ?: $default;
```

Först anropas `function($a, $b)`, sedan testas dess värde och om det inte är tomt överförs det omedelbart till variabeln `$c`, annars används variabeln `$default`.

Operatören `?:` fungerar som operatören `!=` i ett villkor (falsk oavsett datatyp) och kan komma ihåg genom att se ut som en smiley med Elvis-hår, till exempel.

Stöd för äldre versioner av PHP
----------------------------

Operatorn `?:` har fungerat sedan PHP 7. I äldre versioner måste vi nöja oss med villkoret `if (...)`, som kan ge samma resultat. Kom ihåg att ternära operatörer egentligen bara är en genväg för att skriva samma sak som hanteras av villkor.

Att ett värde inte finns kan kontrolleras med funktionen `isset()`, som kontrollerar att variabeln finns och inte är tom (`null`).

Istället för kod:

```php
$a = 5;
$b = 3;

$c = $a ?? $b;
```

Sedan skriver vi ner det äldre alternativet:

```php
$a = 5;
$b = 3;

$c = isset($a) && $a ?? $b;
```

> **Varning:** Ordningen av `isset()` och själva variabeln spelar roll. Om vi vänder på ordningen och variabeln inte finns skulle det uppstå ett åtkomstfel till den obefintliga variabeln.
