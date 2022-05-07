Echo - Ausgabe im Quellcode
===========================

> id: d6a1a7bf-03bf-49ea-9947-d4d6753ca6e4
> slug:
> 	cs: echo
> 	de: echo---ausgabe-im-quellcode
> 
> perex:
> 	- Echo - výpis řetězce do stránky a na standardní výstup. Možnosti escapování a HTTP hlaviček.
> 	- Echo - gibt die Zeichenkette auf der Seite und in der Standardausgabe aus. Optionen für Escaping und HTTP-Header.
> 
> publicationDate: '2020-02-16 18:40:31'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '7d611691825ec537f58f387696d13e28'

Das "Echo"-Konstrukt wird verwendet, um eine Variable oder Zeichenkette in den Quellcode zu übertragen.

| Unterstützung: | Alle Versionen
|----------------|------
| Kurzbeschreibung: | Ausgabe einer oder mehrerer Zeichenketten
| Typ: | <a href="/befehle-und-funktionen">Befehl, Konstrukt</a> (keine Funktion)

Beschreibung
-----

```php
echo 'Hallo, Welt';
```

Sie sagt "Hallo Welt".

```php
$var = 'Text';
echo $var;
```

Druckt den Wert der Variablen `$var`, d.h. "Text".

**Echo** ist keine Funktion (es ist ein <a href="/befehle-und-funktionen">Befehl</a>), daher können Sie eine Klammer verwenden oder nicht. Daher ist es auch korrekt, `echo ('hello world');` zu schreiben.

> **Zusätzlicher Hinweis:** PHP behandelt **Echo** als einen Befehl (ein Konstrukt) und somit als einen Ausdruck. Der Klammerzusatz ist in diesem Fall optional. Wenn wir die Notation: `echo ('etwas');` verwenden, wird die **Echo**-Anweisung nicht zu einer Funktion und wird auch nicht als solche behandelt. Die Klammer bedeutet in diesem Fall, dass der genaue Wert des Ausdrucks eingeschlossen wird, ähnlich wie in der Mathematik.

Anführungszeichen
--------

Zeichenketten können in Anführungszeichen und Apostrophe eingeschlossen werden.

Also dies:

```php
echo "Hallo";
```

Es ist dasselbe wie das hier:

```php
echo 'Hallo';
```

Beachten Sie jedoch, dass jede Zeichenfolge mit demselben Typ von Anführungszeichen beginnen und enden muss und dass das Anführungszeichen in der Zeichenfolge nicht verwendet werden darf.

Wenn Sie zum Beispiel einen HTML-Link (oder einen beliebigen HTML-Code) ausgeben wollen, müssen Sie dem Anführungszeichen einen Schrägstrich voranstellen. Ein Schrägstrich bedeutet "genau dieses Zeichen", wird also in der Sprache nicht als Ausdruck verstanden.

```php
echo "<a href="index.php"">Linktext</a>";
```

Technischer Hinweis: Anführungszeichen haben in PHP eine <a href="/anführungszeichen-bedeutung">besonders Bedeutung</a>.

Parameter
---------

- arg" Ausgabeparameter.

Rückgabewerte
-----------------

Es wird kein Wert zurückgegeben.

Kann nicht als Variable verwendet werden.

Hinweis
--------

Hinweis: Da es sich um ein **Sprachkonstrukt** (Konstrukt = Befehl) handelt (nicht um eine Funktion), kann es nicht in eine Variable geladen werden.

Beispiel
-------

```php
echo "Hallo, Welt";

echo "echo kann mehrere Textzeilen ausgeben.
Aber Vorsicht mit dem HTML-Tag <br>, er wird nicht gedruckt. Dafür ist die Funktion nl2br() gedacht.";

$a = "php";				// Definition der Variablen

echo "Ich mag" . $a;	// Er schreibt: Ich mag php
```

**Echo** hat auch eine verkürzte Syntax, bei der es möglich ist, nur das Gleichheitszeichen nach dem öffnenden php-Tag zu verwenden.

```php
Ahoj <?=$jmeno;?>!
```

Dies ist nützlich, wenn wir schnell einige Informationen auf die Seite schreiben müssen. Zum Beispiel das laufende Jahr:

```php
Píše Jan Barášek © <?=date('Y');?>
```

> Diese verkürzte Syntax funktioniert nur, wenn verkürzte öffnende php-Tags aktiviert sind, d.h. die Direktive `short_open_tag` auf `on` gesetzt ist.

Operation
-------

Alle gängigen mathematischen Operationen können mit dem Befehl **echo** durchgeführt werden.

Eine ausführliche Erörterung der Mathematik findet sich in <a href="/mathematics">einem gesonderten Artikel</a>.

```php
echo 5 + 3 * 2; // druckt 11
```
