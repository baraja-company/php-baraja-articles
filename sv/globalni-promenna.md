Globala variabler i PHP
=======================

> id: '1cdfc51a-31f4-4a5d-81f6-cfd92f86a9d4'
> slug:
> 	cs: globalni-promenna
> 	sv: globala-variabler-i-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '31e78a9abf19252ef62315230a3731b5'

Globala variabler är tillgängliga när som helst i alla delar av programmet och behöver inte överföras.

> **Varning:** Ett väldesignat program bör inte använda globala variabler eftersom de bryter mot inkapslingsprincipen och kan orsaka svårdetekterade fel om de hanteras slarvigt.

Exempel på användning:

```php
$a = 1;
$b = 2;

function suma(): void
{
	global $a, $b;
	$b = $a + $b;
}

suma();

echo $b; // skriver ut siffran 3 eftersom variabeln $b är global
```

Observera att vi har fått variablerna `$a` och `$b` utanför deras naturliga sammanhang. Detta beteende kallas "magiskt" eftersom om en annan funktion åsidosätter de variabler som används för tillfället kommer programmet att uppleva ett oväntat tillstånd.

På rätt sätt ska programmet **inkapsla** och överföra variablerna varje gång:

```php
$a = 1;
$b = 2;

function suma(int $a, int $b): int
{
	return $a + $b;
}

echo suma($a, $b); // skriver ut 3
```

Tack vare detta kan vi anropa funktionen dynamiskt med olika ingångsparametrar och dess resultat beror endast på ingångarna, inte på miljön.

Hämta inmatningsparametrar från URL
---------------------------------

Kanske är den enda rimliga användningen av globala variabler att analysera användarinmatning, och i så fall talar vi om <a href="/superglobal-variable">superglobala variabler</a>.

I det här fallet är det en ren design eftersom variabeln ska vara skrivskyddad, inte skrivskyddad, och dessutom är den densamma i hela programmet:

```php
function getNameFromUrl(): string
{
    return isset($_GET['namn'])
    	? htmlspecialchars($_GET['namn'])
    	: '';
}

echo getNameFromUrl();
```
