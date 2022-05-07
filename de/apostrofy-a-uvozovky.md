Apostrophe und Anführungszeichen
================================

> id: '526ad995-3412-446e-bb56-9627dff8e29e'
> slug:
> 	cs: apostrofy-a-uvozovky
> 	de: apostrophe-und-anfuehrungszeichen
> 
> perex:
> 	- Použití uvozovek a apostrofů pro ohraničení řetězců v PHP. Escapování řetězců a řešení kontextů.
> 	- Verwendung von Anführungszeichen und Apostrophen zur Abgrenzung von Strings in PHP. String-Escaping und Kontextauflösung.
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1'
> sourceContentHash: c5b0bf8b74238134be5348f886591e2a

Sie können entweder Anführungszeichen oder Apostrophe zur Abgrenzung von Zeichenketten verwenden. Ich persönlich bevorzuge nur **Apostrophe**, es sei denn, es handelt sich um ein besonderes Zeilenumbruchzeichen oder einen Tabulator.

Dafür gibt es eine Reihe von Gründen, gehen wir sie konstruktiv durch.

> Der Hauptgrund, keine Anführungszeichen zu verwenden, ist die Sicherheit und die unsachgemäße Behandlung von Datentypen.

Verwendung von HTML-Tags innerhalb einer Zeichenkette
--------------------------------

Wenn wir HTML-Code in einer Zeichenkette zurückgeben müssen und die Zeichenkette in Anführungszeichen einbetten, müssen wir sie ziemlich umständlich escapen:

```php
return "<a href=\"{$link}\">{$text}</a>";
```

Oder machen Sie dasselbe auf eine lesbarere Art und Weise, indem Sie die Anführungszeichen beibehalten, ohne sie zu escapen:

```php
return '<a href="' . $link . '">' . $text . '</a>';
```

Größerer visueller Abstand der Zeichenfolge von der Variablen
---------------------------------------------

Es gibt nichts Schlimmeres, als Variablen in einer Zeichenkette direkt übereinander zu platzieren, so dass man nicht auf Anhieb erkennen kann, was eine Variable und was eine Zeichenkette ist:

```php
$url = "{$baseImageUrl}/{$dirName}/{$basename}.{$ext}";
```

Diese Schreibweise hat keine negativen Auswirkungen auf die Funktionalität, lediglich die einzelnen Variablen und festen Zeichen aus der Zeichenkette werden zu dicht aneinandergereiht, was die Lesbarkeit verschlechtert.

Versuchen wir es noch einmal und machen es lesbarer:

```php
$url = $baseImageUrl . '/' . $dirName . '/' . $basename . '.' . $ext;
```

Der Hauptvorteil, den ich sehe, ist die Sauberkeit rund um die Variablen, wo keine unnötigen Zeichen im Namen im Weg sind.

Außerdem ist es einfacher, die Zeichenkette umzubrechen und es gibt keine umbrechenden Zeichen in der Zeichenkette ;)

```php
$url = $baseImageUrl
       . '/' . $dirName
       . '/' . $basename . '.' . $ext;
```

Es ist nicht möglich, eine Funktion innerhalb von Anführungszeichen aufzurufen
---------------------------------------

Aber eine Variable kann das, weshalb "etwas" außerhalb der Zeichenkette angehängt wird (vor allem bei Funktionen), während Variablen wieder in die Zeichenkette geschrieben werden. Und manchmal hängt der Programmierer die Variable sogar an die Zeichenkette an. Kurz gesagt: Verwirrung über Verwirrung.

Warum können wir die Dinge nicht einheitlich angehen?

Ungeeignete Zuordnung eines anderen Datentyps zu einer Zeichenkette
---------------------------------------------------

Betrachten Sie den folgenden Funktionsaufruf:

```php
echo getFullName("{$user->name}");

function getFullName(string $name): string
{
	// ... Implementierung ...
}
```

Es ist möglich, Variablen in Anführungszeichen zu setzen, was dazu führt, dass sie als String umgeschrieben werden. Befindet sich die Variable jedoch in der Zeichenkette selbst und hat einen anderen Datentyp als string, kann die Information über den ursprünglichen Datentyp verfälscht werden. Wenn zum Beispiel das Konstrukt `$user->name` `false` oder `null` zurückliefert, können wir nicht sagen, dass es sich um einen Fehler handelt.

Eine Anwendung sollte eine Fehlerbedingung immer erkennen und ordnungsgemäß fehlschlagen, anstatt sie zu ignorieren. Eine korrekte Fehlermeldung führt zu einer besseren Fehlersuche in der Zukunft.

Gelegentlich kann es zu Überschreibungen kommen, insbesondere weil eine Funktion einen bestimmten Datentyp erfordert:

```php
trim("{$returnText}");
```

In diesem Fall bin ich eher geneigt, sie aufzuschreiben:

```php
trim ((string) $returnText);
```

was nicht so "magisch" ist, und es ist auch für weniger erfahrene Benutzer offensichtlich, was mit der Variablen passiert ist.

Löschen des Wertes "null" aus der Datenbank
----------------------------------

Angenommen, wir rufen den Namen eines Hotels aus einer Datenbank ab:

```php
return "{$row->hotel->name}";
```

Was aber, wenn der Name nicht existiert und `null` ist? Durch die Verwendung von Anführungszeichen haben wir einen potenziell schwer zu erkennenden Fehler erzeugt, da er in eine leere Zeichenfolge umgeschrieben wird. Wir möchten jedoch eine echte "Null" zurückgeben. Es macht einen Unterschied, ob der Datensatz nicht existiert oder zwar existiert, aber leer ist.

Die richtige Vorgehensweise wäre also, nichts anzugeben:

```php
return $row->hotel->name;
```

Erfordert die Funktion die Rückgabe eines bestimmten Datentyps und ist sie diesbezüglich streng? Wenn wir die Spezifikation nicht erfüllen können, sollten wir eine Ausnahme auslösen.

```php
function getHotelName(int $hotelId): string
{
   // Durchführung

   if ($row->hotel->name === null) {
      throw new \Exception('Der Hotelname existiert nicht.');
   }

   return $row->hotel->name;
}
```

Unstimmigkeiten bei der Kodierung von Zeichen und Datentypen
--------------------------------------------

Es ist nicht ungewöhnlich, dass man auf Konstruktionen wie diese stößt:

```php
echo "{$returnCode}" . self::CRLF;
```

Was besser geschrieben wird als:

```php
echo $returnCode . "\n";
```

Die Verwendung von Anführungszeichen um die Variable herum ist in diesem Fall unnötig und erschwert nur die Lesbarkeit, da es sich um zusätzliche unnötige Zeichen handelt. Gleichzeitig ist der Zeilenumbruch des Typs `CRLF` nicht mehr zeitgemäß und es ist besser, nur `\n` zu verwenden, das in der `UTF-8`-Kodierung enthalten ist.

Der Datentyp für den Code könnte noch validiert werden. Zum Beispiel, dass es sich um eine Zahl handelt und möglicherweise eine Ersetzung zurückgibt. Dies kann mit dem <a href="/ternary-operator">ternary operator</a> geschehen:

```php
echo (is_int($returnCode) ? $returnCode : '?') . "\n";
```

> Hinweis: Die Verwendung der Funktion `is_int()` deutet auf ein schlechtes Anwendungsdesign hin, denn es sollte nie passieren, dass wir den Datentyp einer Variablen nicht kennen.

Umhüllung der Ausgabezeichenkette mit Apostrophen
---------------------------------------

Oft ist es notwendig, den Ausgabestring einer Variablen im Ausnahmetext in Anführungszeichen oder Apostrophe einzuschließen. Wenn die Zeichenfolge mit demselben Zeichen beginnt und endet, ist es nicht möglich, bei mehreren Zeichenfolgen schnell und eindeutig festzustellen, wo die Zeichenfolge beginnt und endet, wenn ein Apostroph darin vorkommt:

```php
throw new \Exception(__METHOD__ . ": Unsupported data type '{$fileType}'");
```

Ich persönlich löse das Problem, indem ich sie in einen anderen Zeichentyp einfüge (eckige Klammern funktionieren bei mir gut), so dass der Anfang und das Ende der Zeichenkette elegant zu erkennen sind:

```php
throw new \Exception(__METHOD__ . ': Nicht unterstützter Datentyp [' . $fileType . ']');
```

Oder:

```php
throw new \Exception("Status '{$status}' is invalid");
```

Kann umgetauscht werden gegen:

```php
throw new \Exception('Status [' . $status . '] ist ungültig');
```

Parsing nach Zeilen
--------------------

Anführungszeichen sind z. B. für das Parsing durch eine neue Zeile nützlich:

```php
foreach(explode("\n", $text) as $line) {
	// Durchführung
}
```

Sie wurden speziell für diesen Zweck geschaffen.

Wenn Sie jedoch ein komplexeres Dokument verarbeiten müssen (z. B. Quellcode einer Programmiersprache oder eine HTML-Seite), empfehle ich **Tokenizer** zusammen mit <a href="/regex">regulären Ausdrücken</a>.

Kombinieren Sie nicht zwei Methoden des Einschließens
-----------------------------------

Wenn Sie aus irgendeinem Grund trotzdem Anführungszeichen verwenden, sollten Sie sie wenigstens durchgängig beibehalten.

Dies ist ein Fall der Abschreckung:

```php
throw new \Exception("Das Bild in der URL existiert nicht. AntwortGröße:" . strlen($result) . ')');
```

Dabei wird der Anfang der Zeichenfolge durch Anführungszeichen und das Ende durch Apostrophe abgegrenzt.

Dieser Fehler tritt insbesondere bei der Verwendung von automatischen Formatierungswerkzeugen (wie **Code Sniffer**) auf, die versuchen, Konventionen zu vereinheitlichen, was aber nicht immer gelingt.
