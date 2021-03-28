Globální proměnné v PHP
================================

> id: "1cdfc51a-31f4-4a5d-81f6-cfd92f86a9d4"
> slugCS: globalni-promenna
> publicationDate: "2019-08-22 20:48:46"
> mainCategoryId: "0e39aee9-2818-480c-8081-e0c2d039bb24"

Globální proměnné jsou kdykoli k dispozici v každé části aplikace a nemusíme je předávat.

> **Pozor:** Dobře navržená aplikace by neměla globální proměnné používat, protože porušují princip zapouzdřenosti a při neopatrném zacházení dochází k těžko odhalitelným chybám.

Příklad použití:

```php
$a = 1;
$b = 2;

function suma()
{
	global $a, $b;
	$b = $a + $b;
}

suma();

echo $b; // vypíše číslo 3, protože proměnná $b je globální
```

Všimněte si, že jsme získali proměnnou `$a` a `$b` mimo její přirozený kontext. Toto chování se označuje jako "magické", protože pokud jiná funkce přepíše aktuálně používané proměnné, tak dojde k nečekanému stavu aplikace.

Správně by se měla aplikace **zapouzdřit** a proměnné pokaždé předávat:

```php
$a = 1;
$b = 2;

function suma(int $a, int $b): int
{
	return $a + $b;
}

echo suma($a, $b); // vypíše 3
```

Díky tomuto můžeme funkci volat dynamicky s různými vstupními parametry a její výstup bude závislý jen na vstupech, nikoli okolním prostředí.

Získání vstupních parametrů z URL
---------------------------------

Snad jediný rozumný způsob využití globální proměnných je u parsování vstupu uživatele, v takovém případě hovoříme o <a href="/superglobalni-promenna">superglobální proměnné</a>.

V tomto případě jde o čistý návrh, protože by proměnná měla sloužit pouze pro čtení, nikoli pro zápis a navíc je v celé aplikaci stejná:

```php
function getNameFromUrl(): string
{
    return isset($_GET['name'])
    	? htmlspecialchars($_GET['name'])
    	: '';
}

echo getNameFromUrl();
```
