Datatyper i PHP
===============

> id: cd63818c-259a-484e-b006-86c35b362019
> slug:
> 	cs: datove-typy
> 	da: datatyper-i-php
> 
> perex:
> 	- 'Datové typy v PHP: String, integer, float, boolean, pole (array), objekt a další.'
> 	- 'Datatyper i PHP: string, heltal, float, boolean, array, objekt og meget mere.'
> 
> publicationDate: '2019-08-23 15:44:28'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: fcb2c647973c5f94e601c789b0a882f1

Alle data, der behandles i PHP, er af en bestemt type. F.eks. et heltal, en streng eller en boolean (sand/falsk).

Grundlæggende datatyper
--------------------

Grundlæggende typer kaldes også **primitive datatyper** eller <a href="/function-is-scalar">skalartyper</a>.

| Type | Navn | Beskrivelse |
|---------|-----------------|-------|
| `int` | Integer | (`integer`) Indeholder kun heltal. Den maksimale værdi bestemmes af antallet af bits. På 32-bit PHP er intervallet fra `-2.147.483.648` til `-2.147.483.647` (~ ± 2 milliarder), 64-bit PHP har et interval fra `-9.223.372.036.854.775.808` til `-9.223.372.036.854.775.807` (~ ± 9 quintillioner). Den maksimale værdi kan altid fås ved at kalde konstanten `PHP_INT_MAX`. Hvis den maksimale værdi af et heltal overskrides, vil PHP repræsentere tallet som `float` og automatisk overskrive det.
| `float` | Floating Point Decimal | Dette er en variant af floating point-tal, for hvilken reglen *"jo mindre jo mere nøjagtigt "* gælder. Tallet lagres internt som en såkaldt **mantisse** og **eksponent**, så du lagrer faktisk 2 tal, som operationen `mantisse * (2^eksponent)` udføres mellem, hvilket gør det muligt at lagre et meget stort talområde. Dette bruger princippet om, at vi for store tal ikke altid har brug for at kende deres nøjagtige værdi, men at vi ønsker at spare så meget hukommelse som muligt. Tal af typen `float` behøver ikke at blive gemt nøjagtigt og bør ikke bruges til at beregne penge.
| `string` | String | String | Indeholder en sekvens af tegn, der er afgrænset af anførselstegn eller apostroffer. Den maksimale længde er kun begrænset af kapaciteten af arbejdshukommelsen. En streng kan gemmes i enhver kodning, indeholde emoji eller binære data.
| `bool` | Boolean | (`boolean`) Boolsk værdi fra boolsk algebra, kan kun indeholde `true` eller `false`.
| `null` | Tom værdi | Den tomme værdi `null` er nyttig i tilfælde, hvor vi ønsker at udtrykke, at noget ikke eksisterer. En artikel har f.eks. ingen kategori. Nogle gange erstattes `null` fejlagtigt af null (`0`) eller tomme strenge (`''`), men dette er ikke en god løsning til at udtrykke ikke-eksistens.

**Varsling:** Typen `null` er ikke skalar.

Nul (0) vs. nul
----------------

Mange udviklere har svært ved at forstå forskellen mellem `0` (nul) og `null` (en ikke-eksisterende værdi), når de starter udviklingen.

Denne forskel kan forklares meget godt og humoristisk med følgende billede:

<img src="{$baseUrl}/images/0-vs-null.jpg" alt="0 vs. null" class="w-100 mb-3">

Manuel ompakning
--------------------

Nogle typer kan konverteres mellem hinanden. Måldatatypen angives ved hjælp af runde parenteser og kan angives overalt i programmet, f.eks. når en værdi oplistes.

For eksempel:

```php
$pi = 3.14;

echo $pi;       // udskriver 3.14

echo (int) $pi; // udskriver 3
```

Dynamisk genopfyldning
---------------------

Overvej følgende 2 variabler:

```php
$x = 10;
$y = '10';
```

Hvad er forskellen mellem indholdet af variablerne `$x` og `$y`?

Variablen `$x` er et tal, `$y` er en streng (med en "1" og en "0"), så hvis vi bare gemmer variablen i hukommelsen og ikke udfører nogen operation, vil det påvirke værdien. F.eks. vil de følgende 2 indtastninger give det samme resultat:

```php
echo $x + 5;	// udskriver 15
echo $y + 5;	// udskriver 15
```

I det andet tilfælde sker den såkaldte **dynamiske overskrivning**, dvs. at variablen konverterer sin datatype, så der kan udføres en beregningsoperation på den. Man kan ikke altid stole på denne adfærd, og den er mere en korrigerende adfærd for at rette dårligt skrevne begynderskripter. Hvis det er muligt, skal du altid skrive tal med en datatype til lagring af tallene, da det øger deres præcision og gør dem lettere at bruge i fremtiden.

> **Note:** Det er vigtigt at bemærke, at vi ikke kan konvertere datatyper helt vilkårligt, så det er ikke altid muligt. Hvis du overskriver en datatype til en anden (inkompatibel) datatype, kan konverteringen enten slet ikke finde sted, eller det oprindelige indhold kan blive ødelagt eller helt ødelagt og erstattet med en anden. Hvis du f.eks. omskriver en streng til et heltal (og der gemmes en tekst, som ikke er et tal, i variablen), gemmes værdien `1` i variablen i stedet for den numeriske værdi.

Sammenligning af typer
----------------

Når du sammenligner værdier, skal du tage hensyn til forskellige datatyper.

Generelt bruges operatoren `==` til generel sammenligning af to værdier (uanset type), mens `===` sammenligner både værdier og datatype.

For eksempel:

```php
$a = '';
$b = null;

if ($a == $b) {
    // Det vil blive evalueret som TRUE, fordi
    // datatypen konverteres.
}

if ($a === $b) {
    // Udfører en meget mere stringent validering
    // og det vil ikke blive godkendt, fordi det er en anden
    // indhold og en anden datatype, og derfor
    // denne kode vil aldrig køre.
}
```
