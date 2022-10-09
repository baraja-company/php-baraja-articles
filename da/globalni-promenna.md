Globale variabler i PHP
=======================

> id: '1cdfc51a-31f4-4a5d-81f6-cfd92f86a9d4'
> slug:
> 	cs: globalni-promenna
> 	da: globale-variabler-i-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '31e78a9abf19252ef62315230a3731b5'

Globale variabler er tilgængelige når som helst i enhver del af programmet og behøver ikke at blive overgivet.

> **Advarsel:** Et godt designet program bør ikke bruge globale variabler, da de overtræder indkapslingsprincippet og kan forårsage fejl, der er svære at opdage, hvis de håndteres uhensigtsmæssigt.

Eksempel på anvendelse:

```php
$a = 1;
$b = 2;

function suma(): void
{
	global $a, $b;
	$b = $a + $b;
}

suma();

echo $b; // udskriver tallet 3, fordi variablen $b er global
```

Bemærk, at vi har fået variablerne `$a` og `$b` uden for deres naturlige sammenhæng. Denne opførsel kaldes "magisk", fordi hvis en anden funktion tilsidesætter de variabler, der er i brug, vil programmet opleve en uventet tilstand.

Hvis det er korrekt, skal programmet **indkapsle** og videregive variablerne hver gang:

```php
$a = 1;
$b = 2;

function suma(int $a, int $b): int
{
	return $a + $b;
}

echo suma($a, $b); // udskriver 3
```

Takket være dette kan vi kalde funktionen dynamisk med forskellige inputparametre, og dens output vil kun afhænge af inputparametrene og ikke af omgivelserne.

Hentning af inputparametre fra URL
---------------------------------

Måske er den eneste fornuftige anvendelse af globale variabler i analysering af brugerinput, og i så fald taler vi om <a href="/superglobal-variable">superglobale variabler</a>.

I dette tilfælde er det et rent design, fordi variablen skal være skrivebeskyttet og ikke skrivebeskyttet, og desuden er den den samme i hele programmet:

```php
function getNameFromUrl(): string
{
    return isset($_GET['navn'])
    	? htmlspecialchars($_GET['navn'])
    	: '';
}

echo getNameFromUrl();
```
