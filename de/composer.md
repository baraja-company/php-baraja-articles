Composer - vollständiger Überblick über die erweiterten Funktionen
==================================================================

> id: a74d8d59-91ce-4602-ad52-80cf89a647bd
> slug:
> 	cs: composer
> 	de: composer---vollstaendiger-ueberblick-ueber-die-erweiterten-funktionen
> 
> perex:
> 	- Composer je pokročilý správce balíků a závislostí pro vaše PHP aplikace. Článek popisuje jeho všechny výhody a možnosti použití.
> 	- Composer ist ein fortschrittlicher Paket- und Abhängigkeitsmanager für Ihre PHP-Anwendungen. Dieser Artikel beschreibt alle Vorteile und Verwendungsmöglichkeiten.
> 
> publicationDate: '2020-03-10 20:18:19'
> mainCategoryId: '4f1d7d70-c5b0-45f1-b1d2-d03c22aa4154'
> sourceContentHash: '68340d4b4d3c8a6ed143ede176fbf04e'

Wie Sie bereits wissen, ist [Composer] (https://getcomposer.org/) ein robuster Paket- und Abhängigkeitsmanager für PHP, mit dem Sie elegant Hunderte von Projekten auf einmal verwalten und einmal geschriebenen Code gleichzeitig an alle Anwendungen verteilen können.

Dieses Tutorial dient als detaillierte und umfassende Anleitung für Entwickler. Wir behandeln alle wichtigen fortgeschrittenen Techniken für die Arbeit mit Composer und erklären die technischen Details und relevanten Abhängigkeiten.

Installation des Composers
-------------------

Unabhängig von der Plattform laden wir von [der offiziellen Composer-Website] (https://getcomposer.org/) herunter.

Intern wird die PHP-Datei `composer-setup.php` heruntergeladen, die, wenn sie im CLI-Modus ausgeführt wird, Composer installieren kann. Es ist auch wichtig zu wissen, dass Composer ohne PHP nicht funktioniert. Vergewissern Sie sich also zunächst, dass PHP auf Ihrem Computer läuft (es muss nur für Terminal verfügbar sein).

Auf Mac und Linux funktioniert Composer direkt nach der Installation und Sie müssen nur den Befehl `composer -v` aufrufen, um schnell zu überprüfen, ob Composer korrekt installiert ist.

Unter Linux kann der folgende Befehl zur Installation verwendet werden: `/usr/bin/php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"`

Unter Windows empfiehlt es sich, das Tool [Git Bash for Windows](https://gitforwindows.org/) zu installieren, mit dem Sie ein spezielles Terminal öffnen können, das sich fast wie Linux verhält und mit dem Sie in der gleichen Umgebung wie unter Linux arbeiten können.

Die Installation auf Servern erfolgt auf die gleiche Weise wie in einer lokalen Umgebung. Vergewissern Sie sich nur, dass Sie die richtige Version von PHP haben, die Composer intern verwendet.

Verfügbare Befehle
----------------

Composer implementiert eine Reihe von Befehlen.

Die Verwendung ist: `composer <Befehl>`, zum Beispiel: `composer update`.

Übersicht für `1.10.0`:

| Befehl | Beschreibung / Bedeutung |
|-----------------------|----------------|
| `Über` | Zeigt kurze Informationen über Composer an. |
| `Archiv` | Erstellt ein Archiv mit dem Inhalt des ausgewählten Composer-Pakets.
| `browse` | Öffnet die Homepage des ausgewählten Pakets, des Autors oder eine andere verwandte Seite in einem Webbrowser. Oftmals enthalten sie auch eine Dokumentation über die Verwendung der Software.
| `cc` | Löscht den internen Composer-Cache mit Versionen von Paketen, die in der Vergangenheit heruntergeladen wurden.
| `check-platform-reqs` | Prüfen, ob die Installationsanforderungen für die aktuelle Plattform erfüllt sind.
| `clear-cache` | Löschen des internen Composer-Caches. |
| `clearcache` | Internen Composer-Cache löschen. |
| | `config` | Setzt eine Konfigurationsrichtlinie.
| `create-project` | Erstellt ein neues Projekt auf der Grundlage des ausgewählten Pakets und erstellt automatisch einen Ordner, in dem das Projekt abgelegt wird.
| `Abhängig` | Zeigt an, welche Pakete die Installation des ausgewählten Pakets verursacht haben. |
| `Diagnose` | Diagnostiziert das System, um häufige Fehler zu identifizieren. Die Verarbeitung der Ausgabe ist dem Entwickler überlassen, es handelt sich lediglich um eine Auflistung.
| `dump-autoload` | Erzeugt einen neuen <a href="/autoloading-trid">Autoloader</a>. |
| `dumpautoload` | Erzeugt einen neuen <a href="/autoloading-trid">Autoloader</a>. |
| `exec` | Führt Binärdateien und Skripte von Vendor aus.
| Untersucht, wie man Abhängigkeiten herstellt/verändert.
| `global` | Ermöglicht die Ausführung globaler Composer-Befehle über die Variable `$COMPOSER_HOME`.
| `help` | Druckt die Hilfe für Befehle aus. |
| `home` | Öffnet die Startseite eines bestimmten Pakets im Browser. |
| `i` | Installiert alle Projektabhängigkeiten gemäß der Datei `composer.lock`, wenn diese existiert und gültig ist. Wenn es ein Problem gibt, werden die Informationen aus `composer.json` verwendet und `composer.lock` wird in seinen ursprünglichen Zustand zurückversetzt. |
| `info` | Zeigt Informationen über die aktuell installierten Pakete im Projekt an. Zeigt die Namen aller Pakete, ihre aktuellen Versionen und eine kurze Beschreibung an. |
| `init` | Erzeugt eine Basisfunktion `composer.json` im aktuellen Verzeichnis.
| `install` | Installiert alle Projektabhängigkeiten gemäß der Datei `composer.lock`, wenn diese existiert und gültig ist. Wenn ein Problem auftritt, werden die Informationen aus `composer.json` verwendet und `composer.lock` wird in seinen ursprünglichen Zustand zurückversetzt.
| `Lizenzen` | Zeigt eine Liste aller Pakete, ihrer Versionen und der aktuellen Lizenz an.
| "Liste" | Zeigt eine Liste der verfügbaren Befehle an.
| Zeigt eine Liste aller Pakete an, für die eine neuere Version zur Installation verfügbar ist und die die Abhängigkeiten erfüllen. Für jedes Paket wird die letzte kompatible Version angezeigt, die Composer für die Installation vorschlägt.
| Zeigt an, welche Pakete und Abhängigkeiten die Installation des angeforderten Pakets oder der angeforderten Version verhindern.
| `remove` | Entfernt das Paket aus dem `require` oder `require-dev` Konfigurationsabschnitt. |
| `require` | Fügt das angeforderte Paket zu `composer.json` hinzu und installiert es. Wenn die Abhängigkeiten nicht erfüllt werden können, wird der ursprüngliche Zustand wiederhergestellt.
| `run` | Führt die definierten Skripte in `composer.json` aus. |
| `run-script` | Führt die definierten Skripte in `composer.json` aus.
| `Suche` | Sucht Pakete nach Schlüsselwort oder Suchanfrage. |
| `self-update` | Aktualisiert die interne `composer.phar` auf die neueste Version.
| `selfupdate` | Aktualisiert die interne `composer.phar` auf die neueste Version.
| | `show` | Zeigt detaillierte Informationen über die aktuell installierten Pakete an.
| `status` | Zeigt eine Zusammenfassung der lokalen Änderungen an, die manuell an den Paketen vorgenommen wurden, basierend auf einem Vergleich mit der Quelle des Pakets, von der es ursprünglich installiert wurde. |
| `suggests` | Zeigt Vorschläge für Pakete an. Die Vorschläge können verschiedene Arten von Aktionen umfassen, wie z. B. die Installation von Sicherheitsupdates usw.
| `update` | Aktualisiert das gesamte Projekt gemäß den Abhängigkeiten, so dass sie immer alle von `composer.json` erfüllt werden. Bei Erfolg wird die Datei `composer.lock` aktualisiert, in die die aktuell installierten Versionen geschrieben werden.
| `upgrade` | Alias zu `update`. |
| `u` | Alias zu `update`. |
| `validate` | Prüft `composer.json` und `composer.lock` auf Syntaxfehler.
| `Warum` | Zeigt an, welche Pakete die Installation des aktuell ausgewählten Pakets verursacht haben, einschließlich aller Abhängigkeiten.
| `Why-not` | Zeigt an, welche Pakete und Versionen die Installation des ausgewählten Pakets oder der Version verhindern. |

Erstellen und Definieren eines Projekts
----------------------------------

Jedes von Composer verwaltete Projekt wird durch eine Datei `composer.json` in seinem Stammverzeichnis definiert, in der alle Abhängigkeiten festgelegt sind. Die Datei kann entweder manuell für ein bestehendes Projekt oder automatisch bei der Erstellung eines Projekts erstellt werden.

Da alles im Composer ein Paket ist, kann auch das Projekt selbst auf einem Paket basieren. Wenn Sie beispielsweise Dutzende oder Hunderte sehr ähnlicher Projekte erstellen, ist es sinnvoll, deren Grundkonfiguration und Struktur in ein separates Paket zu packen, auf dem die Installation basiert.

Ein Beispiel für ein solches Paket ist meine [Baraja Sandbox](https://github.com/baraja-core/sandbox), die auf reinem Nette 3.0 basiert und eine grundlegende Abhängigkeit zu meinem [Package Manager](https://github.com/baraja-core/package-manager) hinzufügt, den ich für alle Projekte und das Abhängigkeitsmanagement in der Nette-Konfiguration verwende.

Die Sandbox wird dann einfach mit dem Befehl installiert:

```shell
$ composer create-project baraja/sandbox <your-project-name>
```

Anhand des Projektnamens erstellt der Composer dann automatisch einen Ordner, in den das Projekt installiert wird (Kopieren des Paketinhalts und Installieren der Abhängigkeiten).

Im Ordner `vendor` verwaltet Composer dann alle Pakete (ihre physischen Dateien befinden sich dort) und generiert einen Autoload von Klassen, den wir idealerweise direkt als Zeile in `index.php` einfügen:

```php
require __DIR__ . '/anbieter/autoload.php';

// Der Anwendungscode selbst
```

Installieren zusätzlicher Pakete und Abhängigkeiten
-------------------------------------

Innerhalb eines funktionalen Projekts können wir sehr einfach neue Pakete installieren und Abhängigkeiten hinzufügen.

Es gibt 2 Möglichkeiten, dies zu tun:

- Mit dem Befehl `composer require ...`,
- Indem Sie eine Abhängigkeit direkt in die Datei `composer.json` im Abschnitt `require` hinzufügen und dann den Befehl `composer update` verwenden.

Versuchen Sie zum Beispiel PackageManager zu installieren: `composer require baraja-core/package-manager`, oder [Doctrine](https://github.com/baraja-core/doctrine): `composer require baraja-core/doctrine`.

Wenn das gewählte Paket nicht installiert werden kann, können spezifische Gründe abgefragt werden und Composer listet die Abhängigkeiten auf, die dies verhindern. Oft reicht es aus, eine Abhängigkeit von einer bestimmten Version zu beheben oder inkompatiblen Code zu entfernen. Ausführliche Informationen erhalten Sie mit dem Befehl: "Composer why baraja-core/doctrine".

Aktualisierung von Projekten und Paketen
-----------------------------

Ein gut durchdachtes Projekt wird so entwickelt, dass Sie im Laufe der Zeit problemlos Aktualisierungen herunterladen können und immer über die neuesten Versionen aller Pakete verfügen. Der Hauptvorteil besteht darin, dass alle Fehler behoben werden, die Leistung oft verbessert wird und viele neue Funktionen zur Verfügung stehen. Außerdem wird die schrittweise Umstellung die Aktualisierung nach langer Zeit weniger kompliziert machen, da Sie Probleme in kleinerem Rahmen beheben und Inkompatibilitäten vermeiden können.

Um alle Pakete und Abhängigkeiten zu aktualisieren, verwenden Sie den Befehl `composer update`.

Der Aktualisierungsvorgang selbst kann in einigen Fällen fehlschlagen. Der Grund dafür ist in der Regel entweder eine defekte Abhängigkeit oder ein inkompatibles Paket.

Ausführliche Informationen darüber, warum ein Paket nicht installiert werden kann, erhalten Sie mit dem Befehl: `composer why-not baraja-core/doctrine`. Wenn wir das Paket bereits haben und nur die spezifische Version nicht funktioniert (es will nicht installiert werden), können wir auch nach der spezifischen Version fragen: `composer why-not baraja-core/doctrine:v1.0.20`.

In der Datei "composer.json" können wir auch die Abhängigkeit von einer bestimmten Laufzeit auflisten. Dies ist besonders nützlich, wenn wir ein Projekt mit mehreren Personen entwickeln und überprüfen wollen, ob diese alle Erweiterungen installiert haben.

In der Regel wird die PHP-Version überprüft (sie muss "7.1.0" oder höher sein):

```json
{
   "require": {
      "php": ">=7.1.0"
   }
}
```

Möglicherweise andere Systemerweiterungen:

```json
{
   "require": {
      "php": ">=7.1.0",
      "ext-json": "*",
      "ext-session": "*",
      "ext-PDO": "*"
   }
}
```

Diese Regeln werden dann bei der Installation von Paketen oder Upgrades berücksichtigt. Dies hilft, Probleme zu vermeiden, die erst zur Laufzeit auffallen würden. Typischerweise muss z.B. ein Zahlungs-Gateway-Paket mit der API kommunizieren, also muss es Abhängigkeiten von den Erweiterungen `curl` und `json` zulassen.

Fehlerbehebung bei Abhängigkeiten
-----------------------------

Oftmals sind Abhängigkeitsverletzungen auf eine schlechte PHP-Version zurückzuführen. In diesem Fall gibt Composer eine Meldung aus, z. B. "Ihre PHP-Version (7.3.11), die von der Version "config.platform.php" (7.1) überschrieben wird, erfüllt diese Anforderung nicht".

Sehr oft wird der Fehler durch die Einstellungen direkt im Projekt `composer.json` verursacht, wo sich der folgende Abschnitt befindet:

```json
"config": {
   "platform": {
      "php": "7.2"
   }
}
```

Die Änderung **muss direkt in der Datei** vorgenommen werden. Bei einem globalen Projekt (vor der Installation oder mit einer globalen Abhängigkeit) können Sie die Composer-Version mit `composer config -g platform.php 7.2.14` erzwingen (der Schalter `-g` bedeutet `global`).

In manchen Fällen wollen wir die neuesten Paketversionen installieren und die lokalen Umgebungseinstellungen ignorieren. In diesem Fall können wir den erweiterten Befehl verwenden:

```shell
$ composer update --ignore-platform-reqs
```

**Verwenden Sie den Schalter `ignore-platform-reqs` auf eigene Gefahr, er kann das Projekt beschädigen!** Verwenden Sie ihn nicht, wenn Sie die Konsequenzen nicht verstehen.

Manuelle Composer-Aufrufe, Parameter und Speicherverwaltung
------------------------------------------------------

Composer ist eigentlich ein PHP-Skript, das in ein so genanntes PHAR verpackt ist, das eine kompilierte Version einer PHP-Anwendung ist. Die Kenntnis dieser Informationen kann sinnvoll genutzt werden, um z. B. den Aufruf selbst besser zu parametrisieren.

Bei wirklich großen Projekten kommt es manchmal vor, dass der Arbeitsspeicher knapp wird und wir viel mehr zuweisen oder die Verarbeitungszeit der Skripte erhöhen müssen.

Unter Windows können Sie dazu beispielsweise diesen Befehl verwenden:

```shell
$ php -d memory_limit=-1 C:/xampp/htdocs/composer.phar update
```

Mit dem Schalter `memory_limit=-1` wird Composer angewiesen, sich nicht an das RAM-Limit zu halten und so viel Speicher zu verbrauchen, wie er benötigt.

Benutzerdefinierte Skripte nach Composer-Aktion
--------------------------------------------

Nach der Ausführung von Composer-Aktionen können Sie die automatische Ausführung von benutzerdefinierten Skripten aufrufen, die entweder bestimmte Aktionen für das Projekt durchführen oder beispielsweise eine Konfiguration nach der Bereitstellung generieren. Ich verwende diesen Ansatz hauptsächlich auf einem lokalen Server, um ein automatisches Datenbankkonfigurationswerkzeug anzubieten, ein Datenbankschema zu generieren und so weiter.

Wir registrieren die erforderlichen Skripte im Abschnitt `scripts` der Datei `composer.json`:

```json
"scripts": {
   "post-autoload-dump": "Baraja\\PackageManager\\PackageRegistrator::composerPostAutoloadDump"
}
```

In diesem Fall wird die statische Methode `composerPostAutoloadDump` in der Klasse `Baraja\PackageManager\PackageRegistrator` automatisch aufgerufen. Composer verfügt über diese Klasse, weil er zuerst die <a href="/autoloading-trid">Autoloader-Klassen</a> erzeugt hat.

Wenn wir nur die Skripte ausführen und keine unnötigen Aktionen mit Composer durchführen wollen, ist der Befehl `composer dump` sehr nützlich, da er nur einen neuen Autoloader generiert (den ich empfehle, um ihn immer auf dem neuesten Stand zu halten) und dann die Skripte sofort ausführt. Wenn Sie Skripte verwenden möchten, habe ich ein fertiges Paket [Baraja PackageManager] (https://github.com/baraja-core/package-manager) vorbereitet, das intelligente Skripte und interaktive Schnittstellen für das Nette-Framework implementiert.

Versionierung von Vendor in Git?
------------------------

Eine Frage, die wir oft mit Entwicklern diskutieren, ist, ob man den Inhalt des Ordners `vendor` in Git versionieren oder bei jeder Installation neu erstellen soll.

Im Allgemeinen scheint die sauberste Lösung darin zu bestehen, den Inhalt **nicht** zu versionieren und alles jedes Mal neu zu installieren. Die Realität sieht jedoch so aus, dass ein Entwickler von Zeit zu Zeit die Entwicklung eines Pakets einstellt oder es sogar ganz entfernt. Das ständige Herunterladen von Paketen erschwert auch lokale Installationen und Aktualisierungen, verlangsamt die Bereitstellung und kann manchmal für kurze Ausfälle der Website verantwortlich sein, wenn neue Paketversionen nicht korrekt heruntergeladen werden.

Ich betrachte die Suspendierung des Verkäufers als eine Form der "Sicherheit". Wenn sich die Dateien physisch im Versionsverwaltungssystem befinden, haben wir zumindest eine rudimentäre Sicherheit auf dem Produktionsserver, dass die Pakete funktionieren und ihr Code genau so ist, wie er in der lokal laufenden Installation ist. Darüber hinaus benötigt Vendor oft nur einige MB, was bei den heutigen Festplattenkapazitäten sicherlich die Sicherheit einer funktionierenden Website wert ist.

**Praktische Anmerkung:** Für die durchschnittliche Website, die ich betreue, benötigt `vendor` nicht mehr als `30 MB`, was eine akzeptable Kapazität für Git ist. Wenn wir mit Junior ein Repository klonen, müssen wir ihn nicht darin schulen, wie man die Website zum Laufen bringt, und es funktioniert einfach sofort.

Benutzerdefinierte Composer-Pakete
-----------------------

Sie können Ihre eigenen Pakete innerhalb von Composer erstellen, entweder öffentlich verfügbar (registriert bei [Packagist](https://packagist.org/)) oder privat (Sie müssen Ihren eigenen Paketserver haben, zum Beispiel [Satis](https://getcomposer.org/doc/articles/handling-private-packages.md)).

Das Thema der Erstellung, Pflege, Entwicklung und Versionierung von Paketen ist sehr komplex und wird Gegenstand eines eigenen Artikels sein.

In der Zwischenzeit können Sie den Artikel [Semantic Versioning] (https://semver.org/lang/cs/) lesen, den ich für Sie übersetzt habe.
