Grundsätze des Schreibens von Variablen
=======================================

> id: '4c8e631b-b305-4169-8885-4f9506155999'
> slug:
> 	cs: zasady-promennych
> 	de: grundsaetze-des-schreibens-von-variablen
> 
> publicationDate: '2020-02-16 16:26:08'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '5528d9b73c1d1c07c330a58e4aeaa06a'

Dies ist der zweite Teil einer Reihe von Tutorials zu PHP. In dieser Folge werden wir uns die Grundregeln für das Schreiben von Variablen ansehen.

Diese Seite ist nur ein kurzer Überblick. Wenn Sie eine detaillierte technische Beschreibung aller Funktionen suchen, habe ich einen <a href="/variable">separaten Artikel</a> geschrieben.

Grundlegende Syntax
--------------------------

Variablen in PHP beginnen mit dem Dollarzeichen `$`, unmittelbar gefolgt von dem Namen.

```php
$zvire = 'Katze';
```

Strings (Zeichenfolgen) werden in Anführungszeichen oder Apostrophe gesetzt:

```php
$a = "Anführungszeichen";
$b = 'Apostrophe';
```

Ziffern werden nicht in Anführungszeichen gesetzt:

```php
$a = 5;
$b = 10;
$c = 3.14159;
```

Der Variablenname darf nur aus Zeichen des englischen Alphabets und Zahlen bestehen. Der Name beginnt immer mit einem Buchstaben.

Besteht der Name aus mehr als einem Wort, ist es üblich, die "CamelCase"-Syntax zu verwenden (erster Buchstabe klein und jedes weitere Wort beginnt mit einem Großbuchstaben):

```php
$kocka = 'Kitty';
$rychlyPocitac = 'Natürlich ist es meins!';
$pocetRohuJednorozce = 1;
```

Der Name darf keine Leerzeichen, Bindestriche, Haken, Kommas, Anführungszeichen, Klammern oder andere Sonderzeichen enthalten. Das einzige erlaubte Sonderzeichen ist der "Unterstrich".

Dezimalzahlen sind mit einem Punkt zu schreiben:

```php
$pi = 3.14159;
```

Es kann oft nützlich sein, mathematische Operationen direkt bei der Definition einer Variablen durchzuführen:

```php
$a = 5;
$b = 3;
$c = $a + $b;	// 5 + 3 addieren
echo $c;		// druckt 8
```

Korrektes Einfügen eines Anführungszeichens oder Apostrophs
--------------------------

Anführungszeichen und Apostrophe dürfen nicht willkürlich kombiniert werden. Wenn wir beispielsweise ein Anführungszeichen verwenden wollen, müssen wir die Zeichenfolge auch mit dem Anführungszeichen beenden und dürfen es nicht innerhalb der Zeichenfolge verwenden.

Dies ist daher falsch:

```php
echo "<img src="obrazek.gif">";
```

Denn es ist nicht klar, wo die Kette beginnt und endet. Anführungszeichen und Apostrophe können nicht verschachtelt werden.

Eine mögliche Lösung heißt <a href="/escapovani">escaping</a>, wobei dem problematischen Zeichen ein Backslash vorangestellt wird.

```php
echo "<img src="image.gif">";
```

Der Backslash bedeutet, dass das nächste Zeichen genau das sein wird, das wir verwenden wollen.

Für die Ausgabe von HTML-Code ist es jedoch besser, die gesamte Zeichenkette in Apostrophe zu setzen und die Anführungszeichen dann auf normale Weise zu verwenden:

```php
echo '<img src="image.gif">';
```

Alternativ kann sie auch umgekehrt werden:

```php
echo "<img src='bild.gif'>";
```

Füllen einer Variablen aus einer URL-Adresse oder einem Formular
--------------------------

Adressen, die ein Fragezeichen enthalten, enthalten Informationen über die Eingabevariablen, so bezeichnet zum Beispiel `index.php?page=contacts` die Variable `page` mit dem Wert `contacts`. Der Wert dieser Variablen wird als `$_GET['page']` gelesen.

Das Fragezeichen steht in keinem Zusammenhang mit dem Namen der Datei auf der Festplatte. Es ist immer dieselbe Datei, der wir in der Adresse Parameter übergeben.

Ich erörtere dieses Thema ausführlich in meinem Artikel über <a href="/methods-odesilani-dat">Methoden der Datenübermittlung</a>.

Definieren des Inhalts einer Variablen anhand einer Adresse
--------------------------

Einige Variablen sind bereits zum Zeitpunkt der Ausführung des Skripts verfügbar (und können daher sofort verwendet werden), diese werden **superglobale Variablen** genannt. Wenn wir zum Beispiel einen Wert aus einer URL lesen wollen, verwenden wir die Variable `$_GET`.
Die Verwendung ist wie folgt:

```php
$a = $_GET['a'];

echo $a;
```

Dieses Skript gibt das, was es in der URL nach dem Fragezeichen hat, in den Quellcode aus.

> Achtung, diese Probe ist nicht sicher! Würde ein böswilliger Besucher z. B. HTML-Code in die URL eingeben, würde dieser in die Seite eingefügt und ausgeführt werden. Daher müssen wir die Ausgabe immer behandeln; die Funktion `htmlspecialchars()` wird dafür verwendet.

```php
$a = $_GET['a'];

echo htmlspecialchars($a);
```

> Wenn wir auf die Seite zugreifen, ohne den Parameter `?a=anything` anzugeben, wird die Variable `$_GET['a']` nicht existieren und PHP wird eine Fehlermeldung ausgeben. Wir müssen diese Bedingung mit einer Bedingung behandeln und nichts tun, wenn die Variable nicht vorhanden ist (oder alternativ einen anderen Inhalt ausgeben). Das Vorhandensein der Variablen kann mit der Funktion `isset()` überprüft werden.

```php
if (isset($_GET['a'])) {
    $a = $_GET['a'];

    echo htmlspecialchars($a);
} else {
    echo 'Die Variable "a" existiert nicht!';
}
```

Beispiel mit Zählung
--------------------------

Mit den Variablen aus der URL-Adresse können wir z.B. Hundestücke machen, sie addieren und das Ergebnis direkt schreiben:

```php
echo $_GET['a'] + $_GET['b'];
```

Wenn wir mehrere Eingabeparameter in die URL aufnehmen wollen, müssen wir sie durch ein kaufmännisches Und (`&`) trennen. Die Adresse kann wie folgt aussehen: "index.php?a=5&b=3".

Verknüpfung von Texteingaben (Strings)
--------------------------

Wir können auch einfach 2 Texteingaben (Strings) verknüpfen. Dies geschieht mit Hilfe des Punktoperators. Sie können einen Link in einer Variablen oder bei der Auflistung erstellen.

```php
$a = 'Hund';
$b = 'Katze';

echo $a . 'a' . $b;
```

Hier steht "Hund und Katze".
