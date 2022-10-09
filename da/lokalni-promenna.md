Lokale variabler i PHP
======================

> id: '9c3fbf4e-8612-4599-8127-d66b9ec5e980'
> slug:
> 	cs: lokalni-promenna
> 	da: lokale-variabler-i-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: cffee22c4dbcc2ec331b73235349c409

Lokale variabler er kun gyldige inden for kroppen af **<a href="/prikazy-a-function">funktion</a>** eller **metode** (i objektorienteret programmering).

Hvis vi arbejder i forbindelse med et almindeligt script, sker alt som forventet:

```php
$x = 5;

echo $x; // udskriver 5
```

Men når vi definerer en brugerdefineret funktion, ændres opførslen en smule:

```php
$x = 5;

function mojeFunkce(): int
{
    $x = 3;

    echo $x; // udskriver 3
}

echo $x;     // udskriver 5
```

Årsagen er, at variablen $x kun eksisterer i forbindelse med den aktuelle funktion eller metode. Denne adfærd er tilsigtet.

Hvis vi ønsker at overføre en værdi fra den omgivende kode til en funktion, skal vi kalde den med de nødvendige parametre:

```php
echo mojeFunkce(5);	// udskriver 6

function mojeFunkce(int $x): int
{
    return $x + 1;
}
```

Værdier indtastes i funktioner ved hjælp af parametre. Hvis du vil sende yderligere variabler ind i funktionen ud over parametrene, kan du bruge <a href="/global-variable">globale variabler</a>, men det er ikke en god idé.

> Brugen af lokale variabler gør en stor forskel, når man programmerer et større program. Hvis vi ikke kunne skelne mellem variablers gyldighed på tværs af kontekster, ville det være umuligt at garantere, at en variabel ikke ville blive overstyret et sted, hvor vi ikke regner med den (hvilket er grunden til, at globale variabler er onde).

```php
$x = 5;
$y = 3;

function soucet(int $x, int $y): int
{
    return $x + $y;
}

echo $x;             // udskriver 5
echo soucet($x, $y); // udskriver 8
```
