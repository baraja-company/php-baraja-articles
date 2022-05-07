PHP-Funktion date(), Datum und Uhrzeit
======================================

> id: '9b0ec1c7-3431-4d7d-9bcc-6093285f14f1'
> slug:
> 	cs: date
> 	de: php-funktion-date-datum-und-uhrzeit
> 
> perex:
> 	- 'Zjištění data a času, aktuální datum, formátování data a času a převod tvaru.'
> 	- 'Datums- und Uhrzeiterkennung, aktuelles Datum, Datums- und Uhrzeitformatierung und Formatumwandlung.'
> 
> publicationDate: '2019-09-11 10:14:16'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: d0323ba88fcdba84ef45439991301f6c

Die Funktion `date()` ist ein Werkzeug für die Arbeit mit Datum und Uhrzeit. Sie wird in zwei Fällen verwendet:

- **Ermittlung des aktuellen Zustands**, d.h. des aktuellen Datums, der Uhrzeit, ... und die Ausgabe in einem bestimmten Format,
- **Umwandlung** eines bestimmten Datums in ein anderes Format (z. B. Monatszahl in Namen, Jahresnotation, 12- und 24-Stunden-System, ...).

Muster
------

```html
Já jsem speciální stránka. Vím, že právě je
<?php
    echo date('H:i'); // Hodina:minuta
?>
```

> WARNUNG: PHP druckt nicht Ihre Zeit, sondern die Zeit des Servers. Daher kann es sein, dass Sie eine andere Zeit als die auf Ihrem Computer eingestellte Zeit erhalten.

Eingabe-Syntax
--------------------------

Die Funktion wird wie üblich aufgerufen, und die einzelnen Anforderungen werden als Funktionsargumente eingegeben.

```php
echo date('Formatierungszeichen', atribut konkrétního času);
```

Die Formatierungszeichen geben an, in welchem Format das Datum gedruckt wird. Zu den Zeichen können Leerzeichen, Punkte, Doppelpunkte, Bindestriche und andere Zeichen gehören, die selbst keine Formatierungszeichen sind (wenn Sie ein Formatierungszeichen verwenden wollen, müssen Sie es <a href="/carriage-notes">ausblenden</a>). Nachstehend finden Sie eine Übersicht über die einzelnen Tags.

Das zweite (optionale) Attribut gibt das manuell eingegebene Datum oder die Uhrzeit an, das/die umgewandelt und im Format gemäß dem ersten Parameter ausgegeben wird. Sie muss als *Zeitstempel* angegeben werden (kann über das Formatierungskennzeichen "U" ermittelt werden).

Beispiel:

```php
echo date('d. m. Y', 1405856605); // bis 20. 07. 2014
```

Tabelle der zulässigen Formatierungszeichen
--------------------------

| Zeichen | Beschreibung
|------|---------------------
| "Y" | Jahr als vier Ziffern (z.B. 1998)
| "y" | Jahr als Doppelziffer (z.B. 98)
| "M" | Englische Abkürzung des Monatsnamens (z. B. Jan)
| "m" | Monatsnummer (01-12)
| "F" | Englischer Monatsname (z. B. Januar)
| "D" | Englische Abkürzung des Wochentags (z.B. Fri)
| "l" | englische Bezeichnung des Wochentags (z. B. Freitag)
| "N" | Nummer des Wochentags (1 - Montag, 7 - Sonntag)
| "w" | Nummer des Wochentags (0 - Sonntag, 1 - Montag, 6 - Samstag)
| "d" | Tag des Monats (01-31)
| "j" | Nummer des Tages des Monats (1-31)
| "z" | Tag des Jahres (001-365)
| "H" | Stunde (00-23)
| "h" | Stunde (01-12)
| "i" | Minute (00-59)
| "s" | Sekunde (00-59)
| "U" | *Zeitstempel:* Anzahl der Sekunden seit Beginn der Zeit (seit 1. Januar 1970)
| "S" | Englische Endung der Ordnungszahl des Monatstages
| "A" | AM/PM-Anzeige
| Vormittags-/Nachmittagsindikator (am/pm)
| "P" | Differenz zur Greenwich-Zeit (GMT) mit Trennzeichen zwischen Stunden und Minuten (hinzugefügt in PHP 5.1.3), zum Beispiel: "+02:00".
| "g" | Stunde im 12-Stunden-Format (1-12)
| "G" | Stunde im 24-Stunden-Format (0-23)

Zeitformatierung für die Sitemap
---------------------------------

Sehr oft müssen Sie die Uhrzeit für die Datei `sitemap.xml` formatieren, die das Datum und die Uhrzeit der letzten Änderung im Tag `<lastmod>` enthält.

Ab PHP 5.1.3 kann dazu folgende Syntax verwendet werden:

```php
date('Y-m-d\TH:i:sP');
```

Datum und Uhrzeit werden auf die Sekunde genau angezeigt und enthalten auch Informationen über die Zeitzone, in der sich der Server befindet (die Zeitzone wird vom Betriebssystem des Servers bestimmt).

Tschechische Namen von Tagen und Monaten
----------------------------------

Es ist nicht möglich, die tschechischen Namen der Tage und Monate in PHP auf normale Weise zu erhalten, so dass wir diese Werte selbst schreiben müssen. Am besten ist es, die Einträge in einem Array zu speichern und sie mit einem Indexaufruf abzurufen.

```php
$mesice = [
    1 => 'Januar', 'Februar', 'März', 'April', 'Mai',
    'Juni', 'Juli', 'August', 'September', 'Oktober',
    'November', 'Dezember'
];

$dny = ['Sonntag', 'Montag', 'Dienstag', 'Mittwoch', 'Donnerstag', 'Freitag', 'Samstag'];
```

Dieses Beispiel ist ein vereinfachtes Beispiel von <a href="https://php.vrana.cz">**Jakub Vrana**</a> aus dem Artikel <a href="https://php.vrana.cz/ceske-nazvy-mesicu-a-dnu-v-tydnu.php">Tschechische Namen von Monaten und Wochentagen</a>.

Übersetzen von Daten von Englisch nach Englisch
--------------------------------------

Oft haben wir ein Datum im Format `Freitag, 13. September` und wollen es ins Englische übersetzen, aber wie macht man das? Am besten ist es, eine Funktion aufzurufen, zum Beispiel `datumcesky()`, der wir das englische Datum übergeben und die es übersetzt.

```php
function datumCesky(string $date): string
{
    $men = [
        'Januar', 'Februar', 'März', 'April', 'Mai',
        'Juni', 'Juli', 'August', 'September', 'Oktober',
        'November', 'Dezember'
    ];

    $mcz = [
        'Januar', 'Februar', 'März', 'April', 'Mai',
        'Juni', 'Juli', 'August', 'September', 'Oktober',
        'November', 'Dezember'
    ];

    $date = str_replace($men, $mcz, $date);

    $den = [
        'Montag', 'Dienstag', 'Mittwoch', 'Donnerstag',
        'Freitag', 'Samstag', 'Sonntag'
    ];

    $dcz = [
        'Montag', 'Dienstag', 'Mittwoch', 'Donnerstag',
        'Freitag', 'Samstag', 'Sonntag'
    ];

    return str_replace($den, $dcz, $date);
}
```

Beispiel für die Verwendung:

```php
echo datumCesky('Freitag, 13. September'); // Freitag, 13. Dezember
```

Diese Funktion ist sicherlich nicht ideal für die Übersetzung, da sie nur englische Wörter durch tschechische ersetzt, aber für viele Einsätze dürfte sie ausreichend sein. Bei anspruchsvolleren Übersetzungen sollten Sie immer die genaue Syntax gewährleisten, um einen einheitlichen Stil zu gewährleisten, z. B. `Freitag, 13. Dezember`.

Suche nach dem ersten Tag des Monats
-----------------------------

Der erste Tag im April 2018 war ein Sonntag, aber wie kann man das leicht herausfinden?

Seit PHP 5.1.0 gibt es eine einfache Lösung:

```php
echo date('N', strtotime('2018-04-01')); // 1 (Montag), 7 (Sonntag)
```

In älteren Versionen müssen wir die Funktion selbst implementieren:

```php
/**
 * @Autor Jan Barášek
 */
function getFirstDayPosition(?int $year = null, ?int $month = null): int
{
    $day = (int) date('w', strtotime($year . '-' . $month . '-1')) - 1;

    return $day < 0 ? 7 : $day + 1;
}
```

Da der Modifikator "w" die Ausgabe im US-Format liefert, passe ich die Tageszahl noch durch eine einfache Berechnung an. Die Funktion gibt eine ganze Zahl zwischen 1 (Montag) und 7 (Sonntag) zurück.

Zeitverschiebungen / Formatierungsumwandlung und Datumsvalidierung
--------------------------------------------------

Oft müssen wir einen relativen Zeitsprung machen (z. B. +5 Tage) oder ein Datum aus einer Texteingabe des Benutzers ziehen, damit es gültig ist. Zu diesem Zweck erlaubt die Funktion <a href="https://www.php.net/manual/en/function.strtotime.php">strtotime()</a> die folgende Syntax:

```php
echo strtotime('jetzt');
echo strtotime('10. September 2000');
echo strtotime('+1 Tag');
echo strtotime('+1 Woche');
echo strtotime('+1 Woche 2 Tage 4 Stunden 2 Sekunden');
echo strtotime('nächsten Donnerstag');
echo strtotime('letzten Montag');
```

Die Ausgabe der Funktion ist **Zeitstempel** *(Weltzeit)* der angegebenen Zeit als Ganzzahl.

> Im Allgemeinen empfehle ich, diese Funktion in allen Formularen einzusetzen, in denen wir irgendwie mit der Zeit des Benutzers arbeiten. Es ist sicherlich keine gute Idee, den Benutzer zu zwingen, das Datum in einem bestimmten Format zu schreiben, sondern immer ein solches Format automatisch zu erstellen, damit der Benutzer schreiben kann, was er will. Denn oft wird der Text zum Beispiel von irgendwoher kopiert und es ist zu viel Arbeit, ihn manuell umzuformatieren, wenn dies automatisch geschehen kann.

Anzahl der Sekunden ab dem Beginn der Zeit
--------------------------

Seit dem 1. Januar 1970 wird jede Sekunde zu einer Sekunde addiert, um eine große ganze Zahl zu erhalten, die die seither verstrichene Zeit angibt. Dies ist besonders nützlich für die einfache Berechnung von Zeitunterschieden. Diese Lösung ist recht zuverlässig, birgt aber auch Risiken (das Problem des Jahres 2038).

**Allgemeine Eigenschaften dieser Notation:**
- Es handelt sich um eine ganze Zahl, mit der man leicht arbeiten kann,
- Sie ist im Dezimalsystem angegeben, so dass sie für Menschen leicht zu lesen ist (außer dass man die genaue Uhrzeit nicht schnell erkennen kann),
- Sie kann nicht verwendet werden, um den Zeitraum vor dem 1. Januar 1970 darzustellen, da sie nicht negativ sein kann *(einige neue Versionen des Algorithmus implementieren negative Zeiten, aber darauf kann man sich nicht immer verlassen)*,
- Im Jahr 2038 wird es auf 32-Bit-Computern nicht mehr funktionieren und das Programm zum Absturz bringen (wegen Stack-Überlauf und maximaler Integer-Größe).

Das Problem 2038
--------------------------

<h2 id="universalTime" style="background: #222; margin-top: 32px; padding: 32px; color: white; text-align: center; border-radius: 3px;">Ich lese nicht...</h1>

<script>
	setInterval(function() {
		window.document.getElementById('universalTime').innerHTML = Math.floor(Date.now() / 1000);
	}, 250);
</script>

> Über diesem Text wird der aktuelle Zeitwert angezeigt, der jede Sekunde um +1 erhöht wird. Der aktuelle Wert wird von Javascript geladen und ist daher möglicherweise nicht immer genau (er zeigt die Systemzeit Ihres Computers an).

Computer speichern ganze Zahlen in binärer Form, und wenn es sich um eine ganze Zahl handelt, hat sie oft 32 Bits (32 Einsen und Nullen). Die höchstmögliche 32-Bit-Zahl ist: `011 111 111 111 111 111 111 111 111 11`.

Wenn wir jedoch jede Sekunde +1 zu dieser Konstante addieren, werden wir eines Tages eine solche Zahl erhalten *(es wird der 19. Januar 2038 um 03:14:07 sein)*. Wenn wir jedoch versuchen, eine weitere 1 hinzuzufügen, reicht der Bitbereich nicht mehr aus (die Zahl wäre größer, als in 32 Bits gespeichert werden kann), so dass es zu einem **Stapelüberlauf** kommt, d.h. eine 1 wird an den Anfang der Zahl angehängt, was in binärer Form eine **negative Zahl** bedeutet (was wahrscheinlich zu einem Programmabsturz führt) und uns irgendwo im Jahr 1901 landen lässt (igitt!).

Für dieses Problem gibt es zwei Lösungen:

- Erweiterung des Ganzzahlbereichs von 32 Bit auf 64 Bit, was eine Spanne von mehreren tausend Jahren ergibt
- Verwenden Sie den Datentyp "DateTime" (allgemein in PHP verfügbar), der einen objektorientierten Ansatz für die Datumsverwaltung bietet, gut mit der Datenbank kompatibel ist und eine Reihe von Jahren von "0001" bis "9999" bietet, was für die Bedürfnisse der meisten realen Anwendungen ausreichend ist.

Ich persönlich plädiere dafür, den Datentyp `DateTime` zu verwenden und auf die Speicherung ganzer Zahlen zu verzichten.

Unterschiedliche Zeitzonen
-----------------

PHP ist intelligent, d.h. es versucht immer, die aktuell beste Zeitzone zu verwenden, in der sich der Server befindet. Manchmal kann es jedoch vorkommen, dass die Zeitzone nicht korrekt angegeben ist, oder wir programmieren eine globale Anwendung, auf die Benutzer aus der ganzen Welt zugreifen, und müssen daher die Zeitzone manuell ändern.

Dies kann global für das gesamte PHP-Skript durch den Aufruf einer Funktion geschehen:

```php
date_default_timezone_set('UTC');
```

Gelegentlich gibt es auch diese Lösung (Feststellung, dass der Server eine andere Zeitzone als UTC hat):

```php
$defaultTimeZone = 'UTC';

if (date_default_timezone_get() != $defaultTimeZone) {
    date_default_timezone_set($defaultTimeZone);
}
```

Datum und Uhrzeit ändern
--------------------------

Es ist nicht PHP selbst, das für die Ermittlung des aktuellen Datums und der Uhrzeit verantwortlich ist, sondern das Betriebssystem, auf dem es läuft. Wenn Sie also die Uhrzeit manuell ändern müssen, ändern Sie sie dort.
