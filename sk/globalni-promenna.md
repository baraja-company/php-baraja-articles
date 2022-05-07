Globálne premenné v PHP
=======================

> id: '1cdfc51a-31f4-4a5d-81f6-cfd92f86a9d4'
> slug:
> 	cs: globalni-promenna
> 	sk: globalne-premenne-v-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '31e78a9abf19252ef62315230a3731b5'

Globálne premenné sú k dispozícii kedykoľvek v ktorejkoľvek časti aplikácie a nie je potrebné ich odovzdávať.

> **Upozornenie:** Dobre navrhnutá aplikácia by nemala používať globálne premenné, pretože porušujú princíp zapuzdrenia a pri neopatrnej manipulácii môžu spôsobiť ťažko odhaliteľné chyby.

Príklad použitia:

```php
$a = 1;
$b = 2;

function suma(): void
{
	global $a, $b;
	$b = $a + $b;
}

suma();

echo $b; // vypíše číslo 3, pretože premenná $b je globálna
```

Všimnite si, že premenné `$a` a `$b` sme získali mimo ich prirodzeného kontextu. Toto správanie sa označuje ako "magic", pretože ak iná funkcia prepíše aktuálne používané premenné, v aplikácii nastane neočakávaný stav.

Správne by mala aplikácia **zapúzdriť** a zakaždým odovzdať premenné:

```php
$a = 1;
$b = 2;

function suma(int $a, int $b): int
{
	return $a + $b;
}

echo suma($a, $b); // vytlačí 3
```

Vďaka tomu môžeme funkciu volať dynamicky s rôznymi vstupnými parametrami a jej výstup bude závisieť len od vstupov, nie od prostredia.

Získavanie vstupných parametrov z adresy URL
---------------------------------

Jediné rozumné použitie globálnych premenných je snáď len pri analyzovaní používateľských vstupov, v takom prípade hovoríme o <a href="/superglobal-variable">superglobálnych premenných</a>.

V tomto prípade ide o čistý návrh, pretože premenná by mala byť len na čítanie, nie na zápis, a navyše je rovnaká v celej aplikácii:

```php
function getNameFromUrl(): string
{
    return isset($_GET['name'])
    	? htmlspecialchars($_GET['name'])
    	: '';
}

echo getNameFromUrl();
```
