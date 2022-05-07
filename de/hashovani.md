Hashing von Zeichenketten und Passwörtern
=========================================

> id: '7978bee8-62cc-4770-b15b-a8d08d1dcf34'
> slug:
> 	cs: hashovani
> 	de: hashing-von-zeichenketten-und-passwoertern
> 
> perex:
> 	- 'Hash není šifra! Metody hashování dat a hesel. MD5, SHA1, Bcrypt. Ověření hesla.'
> 	- 'Hash ist keine Chiffre! Methoden zum Hashing von Daten und Passwörtern. MD5, SHA1, Bcrypt. Passwort-Überprüfung.'
> 
> publicationDate: '2019-09-11 10:13:30'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '5d1e289fd93e18ad73eb23ee1bbba8ee'

Das Hashing-Verfahren (im Gegensatz zur Verschlüsselung) erzeugt aus der Eingabe eine Ausgabe, aus der die ursprüngliche Zeichenfolge nicht mehr abgeleitet werden kann.

Es eignet sich daher gut für den Schutz sensibler Zeichenfolgen, Passwörter und Prüfsummen.

Eine weitere nette Eigenschaft von Hash-Funktionen ist, dass sie immer gleich lange Ausgaben erzeugen und dass eine kleine Änderung der Eingabe immer die gesamte Ausgabe verändert.

Hashing-Funktionen
----------------

Es gibt viele Hash-Funktionen in PHP, die wichtigsten sind:

- **Bcrypt: password_hash()** - Sicherstes Passwort-Hashing, rechenzeitintensiv, verwendet internes Salz und hasst iterativ.
- **md5()** - Sehr schnelle Funktion, geeignet für das Hashing von Dateien. Die Ausgabe erfolgt immer mit 32 Zeichen.
- **sha1()** - Schnelle Hash-Funktion für Datei-Hashing, intern von Git für Commit-Hashing verwendet. Die Ausgabe erfolgt immer mit 40 Zeichen.

Hashing
-----------

```php
$password = 'Geheim-Passwort';

echo password_hash($password); // Bcrypt
echo md5($password);
echo sha1($password);
```

> **Warnung:** Weder `md5()` noch `sha1()` sind für das Hashing von Passwörtern geeignet, da es rechnerisch einfach ist, das ursprüngliche Passwort zu ermitteln oder zumindest die Passwörter vorzuberechnen. Es ist viel besser, `bcrypt` zu verwenden, das für das Hashing von Passwörtern entwickelt wurde.
>
> Die Website <a href="https://www.md5cracker.com/">md5cracker.com</a> verfügt über eine Datenbank mit Prüfsummen (Hashes). Versuchen Sie, nach dem Hash "79c2b46ce2594ecbcb5b73e928345492" zu suchen, wie Sie sehen können.

Die einzig richtige Lösung: "Bcrypt + salt".
--------------------------------------

In dem Vortrag <a href="https://www.youtube.com/watch?v=F58_A5TM-Sc">Wie man es in der Zielebene nicht vermasselt</a> sprach David Grudl über Möglichkeiten, Passwörter richtig zu hacken und zu speichern.

Die einzig richtige Lösung ist: "Bcrypt + Salt".

Konkret:

```php
$password = 'Hash';

// Erzeugt einen sicheren Hashwert
echo password_hash($password, PASSWORD_BCRYPT);

// Alternativ mit höherer Komplexität (Standard ist 10):
echo password_hash($password, PASSWORD_BCRYPT, ['Kosten' => 12]);
```

Der Vorteil von Bcryp liegt vor allem in seiner Schnelligkeit und der automatischen Besalzung.

Die Tatsache, dass die Generierung **lang** dauert, sagen wir 100 ms, macht es für einen Angreifer sehr teuer, viele Passwörter zu testen.

Außerdem wird der ausgegebene Hash-Wert automatisch mit einem **Zufallssalz** behandelt, d. h., wenn dasselbe Kennwort wiederholt gehasht wird, wird immer ein anderer Hash-Wert ausgegeben. Daher ist ein Angreifer nicht in der Lage, eine vorberechnete Hash-Tabelle zu verwenden.

Daher können wir die Korrektheit des Kennworts nicht durch wiederholtes Hashing überprüfen, sondern müssen eine spezielle Funktion aufrufen:

```php
if (password_verify($password, $hash)) {
    // Passwort ist korrekt
} else {
    // Passwort ist falsch
}
```

Passwort-Saling
------------

Um das Knacken von Hashes zu erschweren, ist es eine gute Idee, eine zusätzliche Zeichenfolge in die ursprüngliche Eingabe einzufügen. Idealerweise eine zufällige. Dieser Vorgang wird **Passwortsalzen** genannt.

Die Sicherheit basiert auf der Idee, dass ein Angreifer nicht in der Lage sein wird, eine vorberechnete Tabelle von Passwörtern und Hashes zu verwenden, da er das Salt nicht kennt und die Passwörter einzeln knacken muss.

Zum Beispiel:

```php
$password = 'geheimer_Reisepass';
$salt = 'fghjgtzjhg';

$hash = md5($password . $salt);

echo $password; // druckt das ursprüngliche Passwort
echo $hash;     // druckt Passwort-Hash inklusive Salt
```

Zusammengesetzte Hash-Funktionen
------------------------

Sie denken vielleicht, dass es eine gute Idee wäre, die Hash-Funktion wiederholt auszuführen und damit die Komplexität des Knackens zu erhöhen, da das ursprüngliche Kennwort wiederholt gehasht werden muss.

Zum Beispiel:

```php
$password = 'Passwort';

for ($i = 0; $i <= 1000; $i++) {
    $password = md5($password);
}

echo $password; // 1000x gehasht mit md5()
```

Paradoxerweise ist die Schwierigkeit des Durchbruchs geringer oder bleibt fast gleich.

Der Grund dafür ist, dass die Funktion `md5()` extrem schnell ist und auf einem normalen Computer mehr als eine Million Hashes pro Sekunde berechnen kann, so dass das Ausprobieren eines Kennworts nach dem anderen nicht viel langsamer ist.

Der zweite Grund ist eher eine Theorie, nämlich die Möglichkeit einer so genannten Kollision. Wenn wir ein Kennwort wiederholt hashen, kann es passieren, dass wir auf einen Hash stoßen, den der Angreifer bereits kennt, und der es ihm ermöglicht, das Kennwort mithilfe der Datenbank zu hashen.

Daher ist es besser, eine langsame, sichere Hashing-Funktion zu verwenden und das Hashing nur einmal durchzuführen, während die endgültige Ausgabe weiterhin mit Salting behandelt wird.
