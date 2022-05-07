Autoload-Klassen in PHP
=======================

> id: f6cd5762-261f-4153-b27b-075dd8b5ed13
> slug:
> 	cs: autoloading-trid
> 	de: autoload-klassen-in-php
> 
> publicationDate: '2020-02-09 10:00:29'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '441a1d4107bb32e8cfe4dbb926c2decd'

Ich bin sicher, Sie kennen das, wenn wir PHP-Skripte programmieren, teilen wir den Code in viele Dateien auf und um alle Teile verfügbar zu haben, laden wir sie mit einer Reihe von `include`, `require` oder vorzugsweise `require_once` Aufrufen, was garantiert, dass sie nur einmal geladen werden.

Im Code sieht das so aus:

```php
require_once 'Router.php';
require_once 'Seite.php';
require_once 'Paginator.php';
```

Der größte Nachteil dieses Ansatzes besteht darin, dass der Programmierer ständig dafür sorgen muss, dass alles immer geladen ist. Aber wenn er viel lädt, verliert er unnötig an Leistung und erreicht die Festplatte viele Male. Die manuelle Lösung hat also nur Probleme.

Natives Autoloading
-------------------

Glücklicherweise gibt es in PHP native Unterstützung für das so genannte **Class Autoloading**, eine Logik im Code, die eine Klassendatei nur dann lädt, wenn sie zum ersten Mal benötigt wird (typischerweise, wenn eine Instanz zum ersten Mal erstellt wird).

Eine einfache Umsetzung könnte dann wie folgt aussehen:

```php
spl_autoload_register(function (string $className): void {
    include 'src/' . $className . '.php';
});

$obj  = new MyClass1();
$obj2 = new MyClass2();
```

Beim Erstellen einer Instanz der Klasse `MyClass1` liest die Funktion `spl_autoload_register` die Datei `MyClass1.php` aus dem Verzeichnis `src`. Bei dieser Implementierung wird davon ausgegangen, dass sich jede Klasse in einer separaten Datei befindet, die nach dem Namen der Klasse oder Schnittstelle benannt ist.

Komplexere Fälle von Autoloading
-------------------------------

In einer realen Anwendung können viele unangenehme Situationen auftreten, die z. B. das automatische Laden erschweren:

- Die Klasse oder Schnittstelle existiert überhaupt nicht
- Die Datei wurde bereits einmal geladen
- Es gibt mehrere Klassen oder Schnittstellen in der gleichen Datei
- Der Pfad zur Klasse oder Schnittstelle stimmt nicht mit dem Namen überein
- Der Standort der Klasse oder Schnittstelle ändert sich im Laufe der Zeit

Allerdings müssen wir für all dies keine eigene Lösung programmieren, sondern können die bereits vorhandene Lösung verwenden.

Wenn Sie <a href="https://getcomposer.org/doc/01-basic-usage.md">Composer</a> verwenden, nutzen Sie wahrscheinlich auch dessen automatisches Laden. Der Grund dafür ist, dass Composer bei der Installation eines Pakets automatisch eine "Class Map" erstellt, die einen Überblick über die Klassen und ihren physischen Standort gibt.

Am Anfang des Codes (typischerweise in `index.php`) verwenden Sie dann einfach:

```php
require __DIR__ . '/anbieter/autoload.php';
```

Die automatische Ladung wird jedoch nur einmal beim Aufruf des Befehls "Composer Dump" erzeugt, so dass die automatische Ladung jedes Mal neu erzeugt werden muss, wenn sich die Anwendung ändert.

RobotLoader - eine elegante Lösung für die Entwicklung
----------------------------------------

Für die lokale Entwicklung gefällt mir das Paket `nette/robot-loader` sehr gut, das automatisch die Verzeichnisstruktur durchsucht und die Klassenspeicherorte zwischenspeichert. Wenn wir also eine Klasse laden, wird zuerst im Cache nachgeschaut, und nur wenn sie nicht vorhanden ist, wird das Projekt automatisch neu indiziert. Der Programmierer muss sich also nicht mehr darum kümmern, wo sich eine Datei oder eine Klasse befindet, sondern programmiert einfach.

Installation über Composer:

```txt
composer require nette/robot-loader
```

Die grundlegende Erklärung der Funktionalität ist in der <a href="https://doc.nette.org/cs/3.0/robotloader">Dokumentation</a> selbst beschrieben:

> Ähnlich wie der Google-Roboter Webseiten crawlt und indiziert, crawlt RobotLoader alle PHP-Skripte und notiert, welche Klassen, Schnittstellen und Traits er darin gefunden hat. Anschließend werden die Ergebnisse der Recherche zwischengespeichert und bei der nächsten Anfrage verwendet. Sie müssen also nur angeben, welche Verzeichnisse durchsucht und wo sie zwischengespeichert werden sollen.

Es ist dann extrem einfach zu bedienen:

```php
$loader = new Nette\Loaders\RobotLoader;

// Verzeichnisse hinzufügen, die RobotLoader indizieren soll
$loader->addDirectory(__DIR__ . '/app');
$loader->addDirectory(__DIR__ . '/libs');

// Zwischenspeicherung auf der Festplatte im Verzeichnis "temp" einstellen
$loader->setTempDirectory(__DIR__ . '/temp');
$loader->register(); // RobotLoader starten
```

Die Einstellung `$loader->setAutoRefresh(true oder false)` bestimmt, ob RobotLoader Dateien neu indizieren soll, wenn er auf eine neue Klasse stößt. Diese Funktion sollte auf Produktionsservern deaktiviert werden.

So, jetzt müssen Sie sich nie wieder mit Autoloading beschäftigen.

Kombinierte Lösung
------------------

Ich verwende eine kombinierte Lösung, wenn ich ein echtes Projekt entwickle.

In der Praxis funktioniert es so, dass ich die installierten Pakete über Composer autoload lade (was sehr effizient ist) und dies löst das Laden aller Klassen im `vendor` Verzeichnis.

Der Code für das jeweilige Projekt wird dann im Verzeichnis "app" abgelegt, wo ich nur einige wenige Klassen über den RobotLoader automatisch lade. Wichtig ist, die konkrete Anwendung immer so klein wie möglich zu halten und so viel wie möglich vorgefertigte Pakete zu verwenden, das fördert die Wiederverwendbarkeit sehr.

Die Struktur des Projekts sieht folgendermaßen aus:

```txt
/app
    Bootstrap.php <-- konfigurace
    /model
        UserForm.php <-- projektové třídy
        RegisterFactory.php
        ...
/vendor
    ... <-- knihovny
/www
    index.php <-- inicializace
```

Autoloading-Tests
------------------------

Manchmal kann es vorkommen, dass nicht alle Dateien immer geladen werden und Sie Probleme haben.

Zur Fehlersuche empfehle ich die Funktion <a href="/get-list-of-all-loaded-files">get_included_files()</a>.
