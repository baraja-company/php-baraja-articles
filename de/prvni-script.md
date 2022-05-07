Wie Sie Ihr erstes PHP-Skript schreiben
=======================================

> id: '2d6986dc-f24b-4ae5-b24c-d5bb9bf94512'
> slug:
> 	cs: prvni-script
> 	de: wie-sie-ihr-erstes-php-skript-schreiben
> 
> perex:
> 	- Jak napsat první PHP script? Úvod do PHP pro začátečníky.
> 	- Wie schreibt man sein erstes PHP-Skript? Einführung in PHP für Anfänger.
> 
> publicationDate: '2020-02-16 16:26:08'
> mainCategoryId: f4a34087-1b51-4761-8128-4459dfe83d8a
> sourceContentHash: a5fab0e5edf99323140e497a6fe52322

Bevor wir unser erstes PHP-Skript schreiben, müssen wir zunächst theoretisch erklären, wie man eine Seite mit PHP lädt.

1. zunächst ruft der Nutzer eine bestimmte URL in seinem Webbrowser auf, z. B. `https://baraja.cz`
2. Dann erstellt der Webbrowser eine Anfrage an den Webserver, die an das Internet gesendet wird. Sie enthält Informationen über die angeforderte Seite, grundlegende Browser-Identifikation und -Einstellungen, Informationen über Cookies und so weiter.
3. Diese "Anfrage" wird über das Internet an einen Webserver (meist "Apache") weitergeleitet, der die Anfrage liest und eine Antwort zusammenstellt.
4. Da es sich bei der aufgerufenen URL um ein PHP-Skript handelt und die Anfrage eine Datei namens "index.php" betrifft, liest "Apache" die Datei "index.php" aus dem Stammverzeichnis auf der Festplatte und übergibt sie an den "PHP-Interpreter", ein Programm, das den PHP-Code selbst verarbeiten und daraus "HTML-Code" erstellen kann, der an den Benutzer zurückgesendet wird.
5. Sobald der HTML-Code kompiliert ist, wird die Antwort an den Benutzer zurückgeschickt (dies wird als "Antwort" bezeichnet) und der Webbrowser rendert die Seite auf normale Weise, als ob es sich um reines HTML handeln würde.

> Beachten Sie, dass der Webbrowser nichts über den Inhalt des PHP-Skripts erfährt, sondern nur das generierte HTML verarbeitet, so dass Ihre Skripte und Serverinhalte sicher bleiben.

Lassen Sie uns das erste Skript erstellen
----------------------------

> Das Schreiben Ihres ersten Skripts setzt voraus, dass Sie einen Webserver auf Ihrem Computer haben. Für Windows ist <a href="https://www.apachefriends.org/index.html">XAMPP</a> am besten geeignet (laden Sie PHP Version 7.0 oder höher herunter), und <a href="https://www.apachefriends.org/index.html">XAMPP</a> funktioniert auf dem Mac genauso wie unter Windows. Für Linux empfehle ich <a href="https://wiki.ubuntu.cz/servery/apache_s_mysql_a_php">LAMP-Server</a> (diese Website läuft auch auf dem Lamp-Server).

Der Name der PHP-Skriptdatei muss mit der Erweiterung `.php` enden, damit der Webserver weiß, dass wir sie nach den PHP-Regeln verarbeiten wollen. Erstellen wir also zum Beispiel eine Datei "index.php", die den Code für die Hauptseite unserer Website enthalten wird.

Öffnen der Datei
--------------------------

Öffnen Sie diese Datei in einem geeigneten Texteditor zum Schreiben von Quellcode.

Unter Windows ist zum Beispiel <a href="https://www.sublimetext.com">Sublime Text</a> ein guter Ausgangspunkt, da es die Syntax (Sprachregeln) schön einfärbt und den Code leichter lesbar macht. Später empfehle ich den Kauf von <a href="https://www.jetbrains.com/phpstorm/">PhpStorm</a>, das in vielen Unternehmen eingesetzt wird und die Möglichkeit bietet, mehrere Personen einzuprogrammieren.

Die Grundstruktur einer HTML-Seite schreiben
--------------------------

Die Grundstruktur einer HTML-Seite kennen Sie wahrscheinlich schon:

```html
<!DOCTYPE HTML>
<html>
    <head>
    <title>Můj první PHP script</title>
    <meta charset="UTF-8">
    </head>
    <body>

    </body>
</html>
```

Der gesamte HTML-Code wird auf normale Weise gehandhabt und ist eine große Hilfe bei der Gestaltung der Website. PHP nutzt in hohem Maße die Prinzipien von HTML und CSS.

Trennung von PHP-Skript und HTML-Code
--------------------------

PHP ist in erster Linie eine Schablonensprache, die an geeigneten Stellen des Codes benutzerdefinierte Inhalte erzeugt. Um klar unterscheiden zu können, was HTML und was PHP ist, müssen wir einen Trennungs-Tag verwenden.

Derzeit ist es am besten, die Notation mit `<?php` und `?>` zu verwenden.

```php
// Hier ist der PHP-Code
?>
```

> Wir verwenden den ">"-Terminator, wenn wir einen anderen HTML-Code verwenden wollen. Wenn sich am Ende des PHP-Skripts kein weiterer HTML-Code befindet, ist es besser, den `?>`-Tag nicht einzufügen, damit am Ende der Seite keine unnötigen Leerzeichen und Zeilenumbrüche entstehen, die der Texteditor einfügen kann.

In der Vergangenheit wurde häufig der `<?`-Tag anstelle von `<?php` verwendet, aber er wird nicht immer unterstützt.

Wrapper-Tags können an beliebiger Stelle im HTML-Code platziert werden, z. B. im Hauptteil der Seite:

```html
<!DOCTYPE HTML>
<html>
    <head>
    <title>Můj první PHP script</title>
    <meta charset="UTF-8">
    </head>
    <body>
    <?php
        // tady bude PHP kód
    ?>
    </body>
</html>
```

Grundlegende Konstruktionen
--------------------------

Zu den grundlegenden Bausteinen gehören:

- <a href="/echo">Auflistung des Inhalts im Quellcode</a>
- <a href="/variabel">Variablen</a>
- <a href="/if">Bedingungen</a>

In diesem Abschnitt wird eine einfache Auflistung von Inhalten in Quellcode unter Verwendung von Variablen demonstriert.

Das Prinzip der Quellcodekonstruktion
--------------------------------

Alle Konstrukte (Sprachausdrücke), Anweisungen und Funktionen werden durch Semikolons getrennt, um eindeutig festzulegen, von wo bis wo das aktuelle Konstrukt gültig ist.

Auf ein Semikolon folgt in der Regel ein Zeilenumbruch.

Symbolisch geschrieben:

```php
příkaz;
další příkaz;
proměnná x = její hodnota;

vypsat proměnnou x;

uložit do souboru;
```

Zum Quellcode extrahieren
--------------------------

Das <a href="/echo">echo</a>-Konstrukt wird verwendet, um den Inhalt aufzulisten.

Es ist sehr einfach zu bedienen:

```php
echo 'Hallo, Welt!';
```

Anschließend wird der Text "Hello world!" in den HTML-Code eingefügt. Probieren Sie die Probe aus.

> Alle anderen Demos enthalten nur das Innere des PHP-Codes. Der umgebende HTML-Code steht Ihnen zur freien Verfügung (verwenden Sie z. B. das Beispiel am Anfang dieses Artikels).

Variablen
--------------------------

Variablen sind virutelle Speicherplätze, die Daten speichern und zum Verschieben von Daten verwendet werden. Der Name einer Variablen beginnt immer mit einem "Dollar", gefolgt von dem "Namen" selbst und dem "Wert".

> Eine detaillierte Beschreibung der Funktionsweise von Variablen habe ich in <a href="/variable">einem separaten Artikel über Variablen</a> zusammengefasst.

```php
$oblibeneCislo = 1024;
$jmenoAutora = 'Jan Barášek';

echo $oblibeneCislo;
echo '<br>';
echo $jmenoAutora;
```

> Der Variablenname sollte ausdrücken, was die Variable tatsächlich enthält, damit der Code klarer wird. Beachten Sie auch die Einfügung des HTML-Tags `<br>`, um den Text einzurücken. Dieser Tag sollte Ihnen bereits aus HTML bekannt sein.

Was im `echo`-Konstrukt ausgegeben wird, nennt man eine Zeichenkette (eine Folge von Zeichen). Einzelne Zeichenfolgen können mit einem Punkt (`.`) verkettet werden, um die Ausgabe auf eine einzige Zeile zu reduzieren:

```php
$oblibeneCislo = 1024;
$jmenoAutora = 'Jan Barášek';

echo $oblibeneCislo . '<br>' . $jmenoAutora;
```

> Nach dem Verbinden der Strings mit einem Punkt wird das Ganze als ein großer String gesehen.

Variable Operationen
--------------------------

Zwischen den Variablen funktionieren alle grundlegenden <a href="/mathematics">mathematischen Operationen</a> völlig intuitiv und wie erwartet.

Definieren wir 2 Variablen und geben wir ihnen Zahlen:

```php
$x = 5; // definiert die Variable x mit dem Wert 5
$y = 3; // definiert eine Variable y mit dem Wert 3

echo $x + $y; // addiert die Variablen und druckt 8
```

> Beachten Sie, dass das Gleichheitszeichen (`=`) nicht zur Durchführung einer mathematischen Operation verwendet wird, so dass Sie z. B. keine Gleichungen schreiben können. PHP verhält sich in dieser Hinsicht einfach wie ein Taschenrechner.

Wenn wir keine Variablen verwenden wollen, können wir die Operationen direkt durchführen. Es spielt also keine Rolle, wo sich der Betrieb befindet, er wird überall bewertet.

```php
echo 5 + 3; // druckt 8
```

Alternativ dazu können wir die Variablen addieren und das Ergebnis in einer anderen Variablen speichern:

```php
$x = 5;
$y = 3;
$z = $x + $y; // Variable $z enthält 8

echo $z; // druckt 8
```

Im nächsten Teil werden wir die kompletten Grundlagen der <a href="/prinzipien-von-variablen">Variablendefinition und -verwendung</a> kennenlernen.
