Globale Variablen in PHP
========================

> id: '1cdfc51a-31f4-4a5d-81f6-cfd92f86a9d4'
> slug:
> 	cs: globalni-promenna
> 	de: globale-variablen-in-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '31e78a9abf19252ef62315230a3731b5'

Globale Variablen sind jederzeit in jedem Teil der Anwendung verfügbar und müssen nicht übergeben werden.

> **Warnung:** Eine gut konzipierte Anwendung sollte keine globalen Variablen verwenden, da sie das Kapselungsprinzip verletzen und bei unvorsichtiger Handhabung schwer zu entdeckende Fehler verursachen können.

Beispiel für die Verwendung:

```php
$a = 1;
$b = 2;

function suma(): void
{
	global $a, $b;
	$b = $a + $b;
}

suma();

echo $b; // gibt die Zahl 3 aus, da die Variable $b global ist
```

Man beachte, dass wir die Variablen `$a` und `$b` außerhalb ihres natürlichen Kontextes erhalten haben. Dieses Verhalten wird als "magisch" bezeichnet, denn wenn eine andere Funktion die derzeit verwendeten Variablen überschreibt, tritt in der Anwendung ein unerwarteter Zustand ein.

Korrekterweise sollte die Anwendung die Variablen **einkapseln** und jedes Mal übergeben:

```php
$a = 1;
$b = 2;

function suma(int $a, int $b): int
{
	return $a + $b;
}

echo suma($a, $b); // druckt 3
```

Dadurch können wir die Funktion dynamisch mit verschiedenen Eingabeparametern aufrufen, und ihre Ausgabe hängt nur von den Eingaben und nicht von der Umgebung ab.

Eingabeparameter von URL abrufen
---------------------------------

Die einzige sinnvolle Verwendung globaler Variablen ist vielleicht das Parsen von Benutzereingaben. In diesem Fall sprechen wir von <a href="/superglobale-variable">superglobalen Variablen</a>.

In diesem Fall handelt es sich um ein sauberes Design, da die Variable nur lesbar und nicht schreibbar sein sollte und außerdem in der gesamten Anwendung gleich ist:

```php
function getNameFromUrl(): string
{
    return isset($_GET['Name'])
    	? htmlspecialchars($_GET['Name'])
    	: '';
}

echo getNameFromUrl();
```
