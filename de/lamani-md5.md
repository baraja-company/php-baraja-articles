Wie man die md5-Funktion knackt
===============================

> id: '9e678fcc-3d5e-46a3-9c3f-c1eb3d1ad367'
> slug:
> 	cs: lamani-md5
> 	de: wie-man-die-md5-funktion-knackt
> 
> publicationDate: '2019-08-23 15:33:10'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '4e176a09e43dbf12e131103be6a9cf4f'

MD5 ist eine weit verbreitete Funktion zur Berechnung von Hashes.

Anfänger verwenden es oft für <a href="/hashovani">Passwort-Hashing</a>, was keine gute Idee ist, da es viele Möglichkeiten gibt, das ursprüngliche Passwort wiederherzustellen.

In diesem Artikel werden spezifische Methoden beschrieben, um dies zu erreichen.

Zeitliche Komplexität
----------------

Alle Sicherheitsmaßnahmen beruhen auf der Tatsache, dass es unverhältnismäßig lange dauert, alle Passwörter auszuprobieren. Nun, das sollte es auch. Das Problem mit dem `md5()`-Algorithmus ist insbesondere, dass er eine sehr schnelle Funktion ist. Auf einem normalen Computer ist es kein Problem, über eine Million Hashes pro Sekunde zu berechnen.

Wenn wir das Passwort knacken, indem wir eine Kombination nach der anderen ausprobieren, handelt es sich um einen **Brute-Force-Angriff**.

Methoden zum Knacken
----------------

Es gibt mehrere Strategien:

- Aufeinanderfolgende Versuch-und-Irrtum-Tests (Brute-Force-Angriff)
- Testen von Wörterbuchpasswörtern
- Rainbow-Tabellen (vorberechnete Hash-Datenbank)
- Google-Suchen
- Kollisionen im Algorithmus

Es gibt noch viele weitere Methoden, dieser Artikel beschreibt nur die gängigsten.

Strategien zum Brechen mit roher Gewalt
-----------------------------

Alle Kombinationen von Buchstaben, Zahlen und anderen Zeichen werden nacheinander ausprobiert.

Die erzeugten Versuche werden nacheinander gehasht und mit dem ursprünglichen Hash verglichen.

So zum Beispiel:

```php
aaaaaaabaaacaaadaaaeaaaf...
```

Das Problem bei diesem Angriff liegt im `md5()`-Algorithmus selbst. Wenn wir nur die Kleinbuchstaben des englischen Alphabets und Zahlen ausprobieren würden, würde es höchstens zehn Minuten dauern, alle Kombinationen auf einem handelsüblichen Computer auszuprobieren.

Es ist daher wichtig, lange Passwörter zu wählen, vorzugsweise zufällige mit Sonderzeichen.

Wörterbuch-Angriffsstrategie
----------------------------

Die meisten Menschen wählen schwache Passwörter, die im Wörterbuch stehen.

Wenn wir uns diese Tatsache zunutze machen, können wir unwahrscheinliche Varianten wie `6w1SCq5cs` schnell verwerfen und stattdessen vorhandene Wörter erraten.

Außerdem wissen wir aus früheren Passwort-Leaks großer Unternehmen, dass die Nutzer einen Großbuchstaben am Anfang des Passworts und eine Zahl am Ende wählen. Mal sehen - hat Ihr Passwort das auch? :)

Rainbow-Tabellen - vorberechnete Datenbank
--------------------------------------

Da ein Passwort immer demselben Hash entspricht, ist es einfach, eine riesige Datenbank neu zu berechnen, in der die Passwörter zuerst durchsucht werden.

Tatsächlich ist die Suche immer um Größenordnungen schneller als das wiederholte Durchsuchen von Hashes.

Außerdem können bei größeren Datenlecks die Passwörter auf diese Weise parallel gehasht werden, so dass z. B. 10 % aller Benutzerpasswörter schnell wiedergefunden werden können.

Eine gute Passwort-Datenbank ist zum Beispiel <a href="https://crackstation.net/">Crack Station</a>.

Google-Suche
-------------------

Viele einfache Passwörter sind Google direkt bekannt, da es Seiten indiziert, die Hashes enthalten.

Ich verwende immer Google als erste Option. :)

Auffinden von Kollisionen im Algorithmus
--------------------------

Das <a href="https://cs.wikipedia.org/wiki/Dirichlet%C5%AFv_princip">Dirichlet-Prinzip</a> besagt, dass es bei einem Satz von Hashes, die immer 32 Zeichen lang sind, mindestens 2 verschiedene Passwörter mit 33 Zeichen (eines davon länger) gibt, die denselben Hash erzeugen.

In der Praxis ist es nicht sinnvoll, nach Kollisionen zu suchen, aber manchmal erleichtert der Anwendungsautor selbst das Raten, indem er die Kollisionen neu berechnet.

Zum Beispiel:

```php
$password = 'Passwort';

for ($i = 0; $i <= 1000; $i++) {
    $password = md5($password);
}

echo $password; // 1000x gehasht mit md5()
```

In diesem Fall ist es sinnvoll, die Kollision zu erraten und nicht den ursprünglichen Hash.

Prost auf das Heften!
