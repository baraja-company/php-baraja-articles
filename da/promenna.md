Variabler i PHP
===============

> id: b89774e7-143c-4a8c-8dc6-3b3d2c78d5b7
> slug:
> 	cs: promenna
> 	da: variabler-i-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '23e4d537f18a3b2e1946c79be1aacf52'

Denne side er en komplet oversigt over, hvordan variabler fungerer i PHP. Teksten er skrevet i en lidt teknisk stil og er måske ikke helt forståelig for begyndere. Hvis du er interesseret i det helt grundlæggende, kan du læse <a href="/første-script">begyndervejledningen</a> og <a href="/principper-for-første-script">principper for skrivning af variabler</a>.

Beskrivelse
-----

En variabel er en virtuel placering i arbejdshukommelsen, den er defineret ved `name` og `data type`. Inden for datatypen har variablen så et `indhold`.

Internt repræsenterer PHP variabler som en såkaldt hashtabel, dvs. alle variabler er gemt i en tabel, som er meget hurtig at søge i, så den tid, der kræves for at få adgang til hver enkelt variabel, er *næsten* konstant.

Eksempler på skrivning:

```php
$a = 10;
$b = 'cat';
$c = true;
```

Hver linje i prøven angiver definitionen af en variabel. Navnet på hver variabel begynder med dollartegnet `$` efterfulgt af selve navnet. Ligningstegnet kan bruges til at tildele en værdi til variablen.

Variablerne gemmes internt i hukommelsen som en hashtabel:

| Navn | Type | Forkortelse | Forkortelse | Værdi |
|-------|---------|---------|---------|
| `$a` | heltal | int | int | `10` |
| `$b` | string | string | string | `cats` |
| `$c` | boolean | bool | `true` | `true` |

Variable typer
---------------

Variabler klassificeres efter adgangsrettigheder og anvendelse:

- <a href="/local-variable">lokale variabler</a> (gyldige inden for kontekst, dvs. inden for funktioner og metoder),
- <a href="/global-variabel">globale variabler</a> (gyldige i hele scriptet),
- <a href="/superglobal-variabel">Superglobale variabler</a> (gyldige i hele servermiljøet - typisk brugerdata),
- <a href="/promenna-variabel">variable variabler</a> (en særlig variabel, der indeholder en henvisning (link) til en anden variabel).

* `Globale variabler` og `variable variabler` bør ikke bruges, da de bidrager til ulæselig kode og "magisk" (uventet) programadfærd.*

Tilladt indhold af variabler
--------------------------

Variabler kan indeholde alt det, som deres aktuelle datatype tillader. Hvis datatypen ikke er angivet, bestemmes den automatisk på grundlag af indholdet (dette anbefales ikke, da det kan føre til fejl).

Datatyper fungerer uafhængigt af hinanden, så vi kan bruge næsten alle typer. Men hvis vi ønsker at udføre en sammenlægningsoperation, skal vi altid sikre, at der konverteres til ét format.

Et godt eksempel er f.eks. addition og multiplikation af tal:

```php
$x = 5;       // heltal
$y = 3;       // heltal
$z = $y + $y; // variablen $z vil blive sammensat på grundlag af flere variabler
```

I dette tilfælde står PHP over for spørgsmålet om, hvilken datatype den nyoprettede variabel `$z` skal have. Hvis de er af samme datatype, og operationen er mulig, arves datatypen.

Nogle gange kan vi dog udføre en operation med flere datatyper:

```php
$x = 1;       // heltal
$y = 3.14159; // float
$z = $y + $y; // float
```

I dette tilfælde sammenlægger vi integer og float. Output vil være et decimaltal, så der anvendes float. I dette tilfælde vil PHP gøre noget, der kaldes **dynamisk repartitionering**.

Vi kan dog ikke altid stole på denne adfærd. Hvordan vil du f.eks. gerne flette et tal og en streng?

```php
$x = 256;     // heltal
$y = 'Hey! Hey!'; // float
$z = $y + $y; // ???
```

Datatyper (oversigt over de vigtigste)
--------------------------------------

PHP er et fortolket sprog, så det har nogle særheder i forhold til andre programmeringssprog, og en af dem er, at vi ikke behøver at angive datatyper for variabler, dvs. at hver variabel ændrer dynamisk sin datatype i overensstemmelse med dens indhold (medmindre andet er angivet). Alligevel er det godt at kende i det mindste de grundlæggende datatyper, hvilket især er nyttigt, når du optimerer programmer eller arbejder med databaser.

Notationen kan se således ud:

```php
$x = (int) 25; // opretter en variabel af typen heltal
```

<a href="/datove-typy">Oversigt over datatyper</a>.

Arv af datatyper
-----------------------

Hvilken datatype vil variablen `$x` have, hvis vi kun kender denne del af kildekoden?

```php
$x = $y;
```

Det afhænger af datatypen for variablen `$y`, som både værdien og datatypen arves fra. I dette tilfælde kender vi ikke variablen `$x`, så vi kan ikke fortsætte med at evaluere koden, og der vil blive sendt en fejlmeddelelse.

Dynamisk tilsidesættelse
---------------------

Lad os have følgende 2 variabler:

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

Repræsentation af strenge som arrays
------------------------------

Alle strenge gemmes internt som et array af tegn. Det vil sige, at hvert tegn har sit eget **indeks** og kan refereres. Hvis vi ikke angiver et indeks, arbejdes der med hele strengen.

```php
$x = 'Lad os programmere i PHP!';
$n = 3;

echo $x;		// dette udskriver hele indholdet af variablen $x
echo $x[0];		// dette udskriver nultegnet for variablen $x
echo $x[$n];	// dette udskriver det niende tegn i variablen $x
```

> **Bemærk:** PHP-numre fra nul, dvs. at nultegnet er "P" og det første tegn er "r".
>
> > Desuden er tegn byte-omskiftet. F.eks. er tegnet "no" i UTF-8-kodning 2 bytes langt, så tegnindekset i strengen passer ikke til den reelle position ved rulning, og der bruges 2 indeks til at gemme tegnet.

Eksistensen af et array-element bør altid kontrolleres med funktionen `isset()`:

```php
if (isset($x[$n])) {
    echo $x[$n];
}
```

Alternativt kan du skrive det pænt ned med en ternær operatør:

```php
echo $x[$n] ?? '';
```

Kopiering af variabler
---------------------

Lad os have følgende variabel:

```php
$q = 'Lorem ipsum, ...';
```

Og kopier derefter dens værdi til den næste variabel:

```php
$qi = $q;
```

Heldigvis sker der ingen kopiering, og PHP gemmer blot en reference til værdien i en **hashtabel**. Værdien vil kun blive kopieret, når værdien af en af variablerne ændres. Denne adfærd håndteres af en komponent, der generelt kaldes **Garbridge collector**.
