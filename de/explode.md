PHP-Funktion Explode - Zeichenkette nach Trennzeichen aufteilen
===============================================================

> id: '4dc7cec6-0e96-4a6b-aee8-32c8817ba11e'
> slug:
> 	cs: explode
> 	de: php-funktion-explode---zeichenkette-nach-trennzeichen-aufteilen
> 
> perex:
> 	- Rozdělení řetězce na více částí podle oddělovače.
> 	- Aufteilung der Kette in mehrere Teile je nach Trennzeichen.
> 
> publicationDate: '2019-11-26 11:39:36'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '66725772be4aae0df8af399e7ce2ba07'

Explode wird verwendet, um eine Zeichenkette einfach durch ein Trennzeichen zu trennen.

- Sie gibt die einzelnen Ergebnisse als Array zurück, das von Null an nummeriert ist,
- Sie können kein Array einfügen, nur die Zeichenkette wird eingegeben,
- kann das Begrenzungszeichen während des Parsens nicht ändern, kann nicht mehrere Begrenzungszeichen auswählen.

| Unterstützung | PHP 4 und höher |
|---------------|-----------------|
| Kurzbeschreibung | Aufteilung einer Zeichenkette in ein Array durch Trennzeichen.
| Anforderungen | Keine
| Hinweis | Kann kein Array einfügen, nur eine Zeichenkette.

Oft müssen wir eine Zeichenkette nach einer einfachen Regel aufteilen. Zum Beispiel eine durch Komma getrennte Liste von Zahlen.

Die Funktion explode() eignet sich hierfür hervorragend, da sie das Trennzeichen (das die Zeichenkette trennt) als ersten Parameter und die Daten selbst als zweiten Parameter annimmt:

```php
$cisla = '3, 1, 4, 1, 5, 9';
$parser = explode(',', $cisla);

foreach ($parser as $cislo) {
	echo $cislo . '<br>';
	// Hier können wir die Zahlen weiter verarbeiten
}
```

Was aber, wenn die Zahlen durch Kommas getrennt sind, aber Leerzeichen um sie herum stehen?

Die Lösung besteht darin, die kleinste gemeinsame Teilzeichenkette zu analysieren und dann unerwünschte Zeichen um sie herum zu entfernen (Leerzeichen und andere Leerzeichen):

```php
$cisla = '3, 1,4, 1 , 5 ,9';
$parser = explode(',', $cisla);

foreach ($parser as $cislo) {
     echo trim($cislo) . '<br>';
     // Hier können wir die Zahlen weiter verarbeiten
}
```

In diesem Fall entfernt die Funktion `trim()` auf elegante Weise die Leerzeichen um die Zeichen herum (Leerzeichen, Tabulatoren, Zeilenumbrüche, ...) und liefert nur saubere Daten.

Limit, Begrenzung der Anzahl der geparsten Entitäten in einem Array
--------------------------

> TIPP: Für viele Beispiele ist explode() nicht geeignet und es ist viel besser, reguläre Ausdrücke zu verwenden.

Häufig sollen die Daten jedoch nur bis zu einer bestimmten Entfernung analysiert werden, wofür der dritte (optionale) Parameter limit verwendet werden kann.

Nehmen wir an, wir haben durch Doppelpunkte getrennte strukturierte Daten, bei denen wir den Inhalt nach dem ersten Doppelpunkt abrufen und die anderen Doppelpunkte ignorieren wollen.
Beispiel:

```php
$cas = 'Format: "j.n.Y - H:i"';
```

Wenn wir die Zeichenkette nur analysieren würden als:

```php
$parser = explode(':', $cas);
```

Wir würden diese 3 Teilzeichenfolgen erhalten (in anderen Fällen könnten es viel mehr sein):

```php
'Format'
'"j.n.Y - H'
'i"'
```

Daher setzen wir eine Grenze für die Anzahl der zu erhaltenden Elemente (und möglicherweise werden alle anderen Elemente an das Ende des letzten Elements angehängt):

```php
$parser = explode(':', $cas, 2);

// zurückkommen:
echo $parser[0]; // Format
echo $parser[1]; // "j.n.Y - H:i"
```

> **Hinweis:** Unerwünschte Anführungszeichen können aus der Zeichenkette entfernt werden, zum Beispiel mit der Funktion "trim()":

```php
echo trim($parser[1], '"'); // der zweite Parameter gibt die zu entfernende Zeichenkarte an
```

Between, eine Zeichenkette zwischen zwei Zeichenketten einfügen
--------------------------

Oft müssen wir eine Zeichenkette abrufen, die von zwei anderen Zeichenketten begrenzt wird. In PHP gibt es dafür keine direkte Funktion, aber wir können sie selbst schreiben:

```php
function between(string $left, string $right, string $data): string
{
   $l = explode($left, $data);
   $r = explode($right, $l[1]);

   return $r[0];
}
```

Reguläre Ausdrücke
--------------------------

Eine viel elegantere Aufteilung und Arbeit mit Strings kann mit <a href="/regex">regulären Ausdrücken</a> erreicht werden, die ich auf einer separaten Seite bespreche.

Ähnliche Funktionen
--------------------------

- <a href="/funktion-implode">Implode()</a> - Verkettung eines Arrays in eine Zeichenkette

Beispiel für das Parsen von IP-Adressen
--------------------------

```php
$ip = '10.0.0.138';
$parser = explode('.', $ip);
echo $parser[0]; // druckt "10"
echo $parser[1]; // druckt "0";
```

Die Variable "$ip" enthält eine Eingabezeichenkette, die nach dem Trennzeichen "." geparst wird, die Rückgabe ist ein Array. Das Parsen erfolgt bis zum Ende der Zeichenkette, sofern keine Begrenzung angegeben ist.

Parameter
--------------------------

| # | Typ | Beschreibung
|---|--------|------|
| 1 | string | Trennzeichenfolge.
| 2 | Zeichenkette | Geprüfte Zeichenkette.
| 3 | int | Parsing-Grenze. Dies ist ein optionaler Parameter. Beispiel:

```php
$text = 'PHP ist meine Lieblingssprache!';
$parser = explode('', $text, 1);

echo $parser[0]; // druckt das erste Wort
echo $parser[1]; // druckt den Rest des Textes
echo $parser[2]; // gibt nichts aus, da ein Limit gesetzt wurde!
```

Rückgabewerte
--------------------------

Der Rückgabewert ist ein Array mit einer geparsten Zeichenkette.

Die Indizes sind von Null bis "X" nummeriert, sofern keine Begrenzung angegeben ist.

Unterschiede zu früheren Versionen
--------------------------

| PHP-Version | Beschreibung |
|-----------|-------|
| 5.1.0 | Unterstützung für negative Grenzwerte für Durchgänge hinzugefügt.
| 4.0.1 | Optionaler Parameter **limit** hinzugefügt.

Tipps und Hinweise
--------------------------

Bei Verwendung eines negativen **Limits** wird die Anzahl der Elemente ab dem Ende der Zeichenkette angegeben.

Beispiel:

```php
$str = 'eins|zwei|drei|vier';

// positive Grenze
print_r(explode('|', $str, 2));

// negative Grenze (seit PHP 5.1)
print_r(explode('|', $str, -1));
```

Die folgenden Felder werden zurückgegeben:

```php
Array
(
    [0] => one
    [1] => two|three|four
)

Array
(
    [0] => one
    [1] => two
    [2] => three
)
```
