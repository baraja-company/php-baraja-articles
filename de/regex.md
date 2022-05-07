Reguläre Ausdrücke in PHP
=========================

> id: '32142161-6ace-4cfd-b472-b48031219e9b'
> slug:
> 	cs: regex
> 	de: regulaere-ausdruecke-in-php
> 
> perex:
> 	- 'Regulární výrazy a jejich kompletní vysvětlení. Hledání podřetězce, pokročilé nahrazování a generování řetězců.'
> 	- 'Reguläre Ausdrücke und ihre vollständige Erklärung. Teilstringsuche, erweiterte Substitution und Stringgenerierung.'
> 
> publicationDate: '2020-03-08 13:37:38'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '392c8f14e6943dd8f345a9a134e0de92'

Reguläre Ausdrücke sind Werkzeuge, die es Ihnen ermöglichen, Zeichenketten anhand einer Maske (eines Musters) auf einfache Weise zu suchen, zu überprüfen, zu vergleichen, aufzuteilen, zu komprimieren und zu ersetzen. Es ist ein sehr leistungsfähiges und elegantes Werkzeug für fortgeschrittene Stringmanipulationen.

Maske
-----

Zu Beginn müssen wir uns zunächst den eigentlichen regulären Ausdruck ausdenken, den wir ausführen wollen. Sie wird als Textstring eingegeben, der eine Reihe von Regeln und Konfigurationsoptionen enthält (dies ist eine sehr komplexe Technik).

Für den Anfang ist es wichtig zu wissen, dass der reguläre Ausdruck sequentiell von links nach rechts ausgewertet wird, und wenn es mehrere Möglichkeiten gibt, die Zeichenkette zu interpretieren, wird immer die größtmögliche Übereinstimmung verwendet (sie verhält sich **hungrig** und versucht, so viele Zeichen wie möglich zu verarbeiten).

Das Verhalten und die Verarbeitungsstrategie eines regulären Ausdrucks kann durch Schalter beeinflusst werden, von denen es viele gibt.

Einfache Überprüfung, ob die Zeichenfolge eine gültige E-Mail ist
-----------------------------------------------

Wie kann man einfach prüfen, ob die Zeichenkette "jan@barasek.com" eine gültige E-Mail-Adresse ist, ohne sie in komplexe Teile aufzuspalten oder Zeichen für Zeichen durchzugehen?

Reguläre Ausdrücke geben die Antwort (der obige Ausdruck ist für die Zwecke des Beispiels sehr vereinfacht, und eine echte Implementierung der Überprüfung von E-Mail-Adressen sollte etwas komplizierter sein):

```php
$mail = 'jan@barasek.com';
$regex = '/^.+@.+\.(en|de|com)$/';

if (preg_match($regex, $mail)) {
   echo 'E-Mail ist gültig';
} else {
   echo 'E-Mail ist nicht gültig';
}
```

Betrachten wir den Ausdruck `/^.+@.+\.(en|en|com)$/` etwas genauer:

Zuerst müssen wir den gesamten Ausdruck in ein Paar von `/`-Zeichen (am Anfang und am Ende) einwickeln, die angeben, wo der Ausdruck beginnt und wo er endet. Auf das "/" am Ende des Ausdrucks folgen alle Modifikatoren (Einstellungen für den Verarbeitungsmodus des Ausdrucks).

Bei der Verarbeitung eines Ausdrucks gehen Sie von der linken Seite aus Zeichen für Zeichen vor. Jeder Begriff hat seine eigene Bedeutung, die in der folgenden Tabelle aufgeführt ist:

| Zeichen | Bedeutung | Beschreibung | Beispiel |
|------|-----------------|-------|-------------
| `^` | Beginn der Zeichenkette | Erzwingt, dass die Zeichenkette an dieser Stelle beginnen muss. | Erzwingt, dass die Zeichenkette mit der Sequenz `+420` beginnen muss (nützlich z. B. für die Zahlenüberprüfung): `/^+420/`. |
| `$` | Ende der Zeichenkette oder Zeile | Erzwingt, dass die Zeichenkette oder Zeile hier enden muss. Das Ende der Zeile wird dann mit "z" bestätigt. <a href="https://phpfashion.com/vite-co-znamena-v-regularnim-vyrazu">Detaillierte Erklärung</a>. | Der Dateiname muss eine Textdatei sein (mit einem Punkt und der Zeichenfolge "txt" am Ende): `/\.txt$/`. |
| `.` | Beliebiges Zeichen | Erfasst genau ein beliebiges Zeichen | Überprüft, ob die Zeichenfolge genau ein beliebiges Zeichen enthält: `/^.$/`.
| Zahl | Erkennt die Zeichen `0-9` | Erkennt eine Telefonnummer, die keine Leerzeichen enthält und 9 Ziffern hat: `/^(\+420)?\d{9}$/`.
| Erlaubt Leerzeichen zwischen den Ziffern einer dreistelligen Telefonnummer: `/^(\d{3}\s?){3}$/`. | Leerzeichen | Fängt Leerzeichen, Bindestriche und Tabulatoren.
| `+` | Mehrere Zeichen, aber mindestens eines | Wiederholt den vorherigen Unterausdruck und versucht, so viel wie möglich zu erfassen. Der Unterausdruck muss mindestens einmal wiederholt werden. | Fängt so viele Ziffern wie möglich, aber mindestens eine: `/\d+/`. |
| `*` | Mehrere Zeichen, kann keins sein | Funktioniert wie `+`, ermöglicht aber das Abfangen einer leeren Zeichenkette (der Wert muss nicht vorhanden sein) | Fängt so viele Ziffern wie möglich, wenn keine vorhanden sind, fängt es eine leere Zeichenkette: `/\d*/`. |
| `(` `)` | Klammern | Bezeichnet einen Unterausdruck. Dies kann verwendet werden, um mehrere verschiedene Tags einzuschließen und dann z. B. eine Wiederholung über sie zu verlangen oder ihren Inhalt in einer Variablen zu fangen. | Teilen wir die Postleitzahl in 2 Teile entsprechend dem Leerzeichen, das optional ist und es kann sogar mehr als eins geben: `/^(\d{3})\s*(\d{2})$/` |
| `\|` | Oder | Enthält einen Unterausdruck oder einen anderen Unterausdruck. | Zahl, die mit `+420` oder `+421` beginnt: `/^+(420\|421)\s*\d+$/`. |
| `\.` | Escaping | Wenn wir ein Zeichen in einem Ausdruck abfangen wollen, das ansonsten eine besondere Bedeutung hat, müssen wir es auf die gleiche Weise escapen wie z.B. Strings in PHP. | Fängt ein Zahlenpaar ab, das durch einen Punkt getrennt ist (wenn wir den Punkt nicht escapen würden, würde es als "beliebiges Zeichen" verstanden werden): `/\d+\.\d+/`.

Der Vollständigkeit halber gebe ich die vollständige Form der Validierungsregel für E-Mails an, wie sie von Nette implementiert wurde:

```php
/**
 * Findet heraus, ob eine Zeichenkette eine gültige E-Mail-Adresse ist.
 */
public static function isEmail(string $value): bool
{
   $atom = "[-a-z0-9!#$%&'*+/=?^_`{|}~]"; // RFC 5322 unquotierte Zeichen im Lokalteil
   $alpha = "a-z\x80-\xFF"; // Obermenge von IDN
   return (bool) preg_match("(^
      (\"([ !#-[\\]-~]*|\\\\[ -~])+\"|$atom+(\\.$atom+)*)  # quoted or unquoted
      @
      ([0-9$alpha]([-0-9$alpha]{0,61}[0-9$alpha])?\\.)+    # domain - RFC 1034
      [$alpha]([-0-9$alpha]{0,17}[$alpha])?                # top domain
   \\z)ix", $value);
}
```

preg_match()` - Validierung nach Muster
------------------------------------

Die Basisfunktion für die Formatvalidierung und das Parsing ist "preg_match()". Sie hat zwei obligatorische Parameter und der dritte kann zur Angabe des Ausgabefeldes verwendet werden.

Beispiel:

```php
$psc = '272 01'; // Kladno

if (preg_match('/^(\d{3})\s*(\d{2})$/', $psc, $parser)) {
   echo 'Die Postleitzahl ist gültig [' . $parser[1] . ','. $parser[2] . ']';
} else {
   echo 'Die Postleitzahl ist ungültig';
}
```

Der Code gibt zurück: `Code ist gültig [272, 01]`.

Beachten Sie die einzelnen Klammern, mit denen wir den Ausdruck in mehrere kleinere Teile aufgespalten haben. Dadurch ist es möglich, die einzelnen Unterausdrücke als Array-Einträge zu erhalten. Die gesamte Funktion gibt dann `true` oder `false` zurück, je nachdem, ob die Zeichenkette erfolgreich erfasst wurde.

Manchmal ist es jedoch sehr schwierig, die numerische Reihenfolge der Klammern zu finden, da sich die Zahl ändern kann oder einfach zu viele Klammern vorhanden sind. In diesem Fall ist es möglich, die Klammern einzeln zu benennen und dann über ihre Namen auf die Tasten zuzugreifen.

Zum Beispiel:

```php
$phone = '777 123 456';

preg_match('/^(?<Operator>\d{3})\s*(?<Zahl>[0-9 ]+)$/', $phone, $parser);

echo $parser['Betreiber']; // 777 zurückgegeben
```

preg_replace()` - Ersetzung durch Muster
----------------------------------------

Es ist auch möglich, Zeichenketten mit Hilfe von Regex zu ersetzen, was vor allem für verschiedene Formatkorrekturen nach der Verwendung nützlich ist.

Angenommen, wir wollen eine vom Benutzer eingegebene Telefonnummer in einer Ganzzahl speichern, da dies von einer Bibliothek eines Drittanbieters verlangt wird, aber die Benutzer können sie in einigen ziemlich wilden Formaten eingeben.

In diesem Fall halte ich mich an das Diktum:

> Seid großzügig bei dem, was ihr empfangt, und streng bei dem, was ihr sendet".

Deshalb passen wir das Format automatisch an. Zunächst zerlegen wir die Zeichenkette durch Parsing in ihre Einzelteile und falten sie dann entsprechend den Klammerzahlen zurück:

```php
function formatPhoneNumber(string $phoneNumber): int
{
   return (int) preg_replace(
      '/^(\+\d{3})\s*(\d{3})\s*(\d{3})\s*(\d{3})$/',
      '$2$3$4',
      $phoneNumber
   );
}

echo formatPhoneNumber('+420 777 123 456');
```

Konstruktion einer Zeichenkette nach einem regulären Ausdruck
----------------------------------------

Regexe sind auch sehr sinnvoll, wenn neue Zeichenketten nach einem komplexen Muster erzeugt werden sollen.

Reines PHP hat dafür keine Unterstützung, aber wir können eine <a href="https://github.com/icomefromthenet/ReverseRegex">ReverseRegex</a>-Bibliothek eines Drittanbieters herunterladen, die dies kann.

Wenn wir zum Beispiel eine Reihe von Kennwörtern auf der Grundlage der Regex `[a-z]{10}` generieren wollen, kann uns nichts aufhalten:

```txt
jmceohykoa
aclohnotga
jqegzuklcv
ixdbpbgpkl
kcyrxqqfyw
jcxsjrtrqb
kvaczmawlz
itwrowxfxh
auinmymonl
dujyzuhoag
vaygybwkfm
```

Die Verwendung ist wie folgt:

```php
use ReverseRegex\Lexer;
use ReverseRegex\Random\SimpleRandom;
use ReverseRegex\Parser;
use ReverseRegex\Generator\Scope;

require 'vendor/autoload.php';

$lexer = new  Lexer('[a-z]{10}');
$gen   = new SimpleRandom(10007);
$result = '';

$parser = new Parser($lexer, new Scope(), new Scope());
$parser->parse()->getResult()->generate($result, $gen);

echo $result;
```

Ich erstelle meine mathematischen Beispiele in Nette in Presenter zum Beispiel auf diese Weise, und es ist wirklich einfach möglich:

```php
public function actionRegex(): void
{
   $lexer = new Lexer('\d{1,3}[\+\-\*\/]');
   $parser = new Parser($lexer, new Scope(), new Scope());
   for ($i = 0; $i <= 10; $i++) {
      $result = '';
      $gen = new SimpleRandom($i);
      $parser->parse()->getResult()->generate($result, $gen);
      dump($result);
   }
   $this->terminate();
}
```

Wichtig für die Bibliothek ist, dass sie für dieselbe Eingabe immer noch dieselbe Ausgabe erzeugt (auch wenn es den Anschein hat, dass für jeden regulären Ausdruck viele mögliche Zeichenfolgen übereinstimmen). Wenn wir den erzeugten Ausdruck nach dem Zufallsprinzip ändern wollen, müssen wir auch die "Saat" ändern, mit der die Ausgabezeichenfolge erzeugt wird. Hierfür ist entweder das Durchlaufen des Saatgutintervalls oder vielleicht die Funktion "Rand(1, 1e6)" nützlich.

Fehlerbehebung und -verarbeitung
-----------------------------

In PHP ist das Abfangen von Fehlern in Regexes die Hölle, aber es gibt trotzdem eine Lösung.

Dies wird in dem Artikel <a href="https://phpfashion.com/zradne-regularni-vyrazy-v-php">Erreichbare reguläre Ausdrücke in PHP</a> von David Grudel ausführlich erklärt.
