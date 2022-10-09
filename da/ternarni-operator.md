Ternære operatorer i PHP (?:) - betingelse i én linje
=====================================================

> id: '1191be9b-ff9d-4725-b0b4-17b60de2b935'
> slug:
> 	cs: ternarni-operator
> 	da: ternaere-operatorer-i-php-betingelse-i-en-linje
> 
> perex:
> 	- Ternární operátor umožňuje zapsat jednoduchou podmínku do jednoho řádku jako výraz.
> 	- Med den ternære operatør kan du skrive en simpel betingelse på én linje som et udtryk.
> 
> publicationDate: '2019-11-26 11:59:18'
> mainCategoryId: f46a0d80-fbe4-4be8-a5e4-04a8d29b0afc
> sourceContentHash: '20ff690401289869eae2a2584fae7568'

Med den ternære operatør kan du forkorte en simpel betingelse til en enkelt linje på et sted, hvor analysering er unødvendig, kompleks eller helt uhensigtsmæssig.

TL;DR
------

De ternære operatorer er en forkortelse for `if` og `else` betingelser, så du behøver ikke at bruge dem. De er nyttige til gentagne stykker verifikationslogik. Brug altid formatet `(betingelse ? positiv logik : negativ logik)` inklusive parenteser. Anvendes til kort verifikation for at gøre koden klarere og kortere.

Grundlæggende definitioner
------------------

Ofte har vi en betingelse i form af f.eks:

```php
$a = 5;
$b = 3;

if ($a > $b) {
     echo 'Det er større';
} else {
     echo 'Den er mindre';
}
```

Så hvis vi ønsker at skrive en enkelt sætning, skal vi bruge 4 linjer kode, som kunne reduceres til én.

```php
$a = 5;
$b = 3;

echo 'Det er' . ($a > $b ? 'større' : 'mindre');
```

Generelt skrives den ternære operator med tre dele (derfor kaldes den "ternær"):

```php
(condition ? 'ja' : 'fra')
```

Ternære operatorer bruges meget ofte i praksis, f.eks. til at angive lige rækker i en tabel:

```php
$pole = [3, 1, 4, 1, 5, 9, 2];

for ($i = 0; $pole[$i]; $i++) {
     echo '<tr class=""' . ($i % 2 ? 'fra' : 'selv') . '">';

          // Dette fungerer på en eller anden måde med et regneark...
          // for eksempel: echo '<td>' . $field[$i] . '</td>';

     echo '</tr>';
}
```

Eksempel på anvendelse af den ternære operatør
------------------------------------

Vi har ofte brug for at kontrollere, om der findes en variabel værdi, og bruge den straks, hvis det er nødvendigt. Hvis den ikke findes, skal vi returnere standardværdien.

Den klassiske fremgangsmåde er at gøre det på denne måde:

```php
$a = 5;
$b = 8;

$c = $a ? $a : $b;
```

Hvis variablen `$a` findes, skal du bruge `$a` som værdi, ellers `$b`.

Nogle gange får vi dog værdien fra en funktion:

```php
$a = 5;
$b = 3;
$default = 42;

$c = my_function($a, $b) ? my_function($a, $b) : $default;
```

Denne metode er meget ineffektiv med hensyn til systemressourcer. Først skal funktionen kaldes, og hvis den findes, kaldes den igen for at få værdien, som gemmes i variablen `$c`.

Dette kan bedre håndteres via en hjælpevariabel:

```php
$a = 5;
$b = 3;
$helper = my_function($a, $b);
$default = 42;

$c = $helper ? $helper : $default;
```

Uhensigtsmæssig brug
------------------

Det er ikke altid værd at bruge den ternære operatør, fordi den har en tendens til at skabe forvirring, når du skriver komplicerede og indlejrede betingelser.

Se selv et eksempel:

```php
$valid = true;
$lang = 'Fransk';

$x = $valid
     ? ($lang === 'Fransk' ? 'oui' : 'ja')
     : ($lang === 'Fransk' ? 'ikke' : 'fra');
```

Ville du vide med et blik, at variablen `$x` ville indeholde værdien `oui`?

Efter lidt øvelse kan du måske gøre det, men det er ikke det rigtige svar. :)

Kontrol af, at værdien (ikke) findes, og brug af standardværdien
--------------------

De ternære operatorer er mest effektive til rutinemæssig verifikation af værdiernes (ikke-)eksistens og brug af andre standardindstillinger.

Vi ønsker f.eks. at kontrollere, om der findes en hovedkategori for en artikel, og hvis ikke, skal vi sende en erstatningsmeddelelse:

```php
echo $mainCategory ?? 'Kategorien findes ikke';
```

Operatoren `??` (to spørgsmålstegn) kontrollerer, om variablen `$mainCategory` findes og ikke er `null`. Den fungerer på samme måde som funktionen `isset()`.

Validering af tomheden af en værdi
-----------------------------

En meget ofte nyttig konstruktion til at verificere, at outputværdien ikke er tom (dvs. ikke `null`, `0`, `false` eller `''*(tom streng)*).

Dette kan skrives på følgende måde:

```php
$a = 5;
$b = 3;
$default = 42;

$c = my_function($a, $b) ?: $default;
```

Først kaldes `function($a, $b)`, derefter testes dens værdi, og hvis den ikke er tom, overføres den straks til variablen `$c`, ellers bruges variablen `$default`.

Operatoren `?:` fungerer som operatoren `!=` i en betingelse (falsk uanset datatype), og kan f.eks. huskes ved at ligne en smiley med Elvis-hår.

Understøttelse af ældre versioner af PHP
----------------------------

Operatoren `?:` har fungeret siden PHP 7. I ældre versioner må vi nøjes med betingelsen `if (...)`, som kan give samme resultat. Husk, at ternære operatorer i virkeligheden bare er en genvej til at skrive det samme, som betingelser håndterer.

Hvis en værdi ikke eksisterer, kan det kontrolleres med funktionen `isset()`, som kontrollerer, at variablen eksisterer og ikke er tom (`null`).

I stedet for kode:

```php
$a = 5;
$b = 3;

$c = $a ?? $b;
```

Derefter skriver vi det ældre alternativ ned:

```php
$a = 5;
$b = 3;

$c = isset($a) && $a ?? $b;
```

> **Varsling:** Rækkefølgen af `isset()` og selve variablen er vigtig. Hvis vi vendte rækkefølgen om, og variablen ikke fandtes, ville det give anledning til en fejl ved adgang til den ikke-eksisterende variabel.
