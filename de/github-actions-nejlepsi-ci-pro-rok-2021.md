GitHub Actions - die beste KI für 2021
======================================

> id: c1fe3b77-a506-4f20-bf1e-90ee82817c7d
> slug:
> 	cs: github-actions-nejlepsi-ci-pro-rok-2021
> 	de: github-actions---die-beste-ki-fuer-2021
> 
> perex:
> 	- 'Protože už nějaký čas vyvíjíte webové aplikace, určitě jste si všimli, že se pro vás celá řada věcí rutinně opakuje, i když by nemusela. Velmi často jde o technickou správu projektů, verzování souborů, automatická kontrola kódu, zpracování různých dat, nebo třeba deploy na server a bezpečnostní věci kolem toho.'
> 	- 'Da Sie schon seit einiger Zeit Webanwendungen entwickeln, ist Ihnen wahrscheinlich aufgefallen, dass sich viele Dinge für Sie routinemäßig wiederholen, auch wenn sie das nicht sollten. Sehr oft geht es um technisches Projektmanagement, Dateiversionierung, automatisierte Codeüberprüfung, verschiedene Datenverarbeitungen oder vielleicht auch um die Bereitstellung auf dem Server und damit verbundene Sicherheitsfragen.'
> 
> publicationDate: '2021-02-07 15:46:39'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: d3d151927a02dde744d1202449c904dc

Da Sie schon seit einiger Zeit Webanwendungen entwickeln, ist Ihnen wahrscheinlich aufgefallen, dass sich viele Dinge für Sie routinemäßig wiederholen, auch wenn sie das nicht sollten. Sehr oft geht es um technisches Projektmanagement, Dateiversionierung, automatisierte Codeüberprüfung, verschiedene Datenverarbeitungen oder vielleicht auch um die Bereitstellung auf dem Server und damit verbundene Sicherheitsfragen.

Bei der Beratung von Unternehmen stoße ich sehr oft auf das Problem, dass die Prävention sehr unterschätzt wird. Der Grund dafür ist oft, dass die Entwickler einige Dinge als sehr anspruchsvoll empfinden und glauben, dass dies zusätzliche Arbeit für sie bedeutet. Die Wahrheit ist jedoch, dass man das ganze System nur einmal einrichten muss und dann die langfristigen Vorteile nutzen kann.

Was ist CI (Kontinuierliche Integration)
----------

CI/CD ist ein Werkzeug, mit dem Routineaufgaben automatisiert werden können, die eine ähnliche Basis haben und sich in Projekten immer wiederholen. Am häufigsten wird CI für automatisierte Tests, Code-Reviews, automatisierte Deployments (Bereitstellung einer Anwendung auf einem Webserver), Projektmanagement oder Sicherheitsaudits eingesetzt.

Betrachten Sie CI als ein virtuelles Betriebssystem, das vordefinierte Befehle auf dem Terminal ausführt oder bestimmte Programme startet. Die Ausgabe von CI ist entweder ein Erfolg oder ein Misserfolg, der direkt im Projekt angezeigt wird, aber auch an Administratoren gesendet wird, die darauf reagieren können. Eine gute Praxis ist es, CI-Aufträge nach einem bestimmten Ereignis auszuführen (z. B. nach einer Übergabe an das Repository, einer eingegangenen Merge-/Pull-Anfrage, einem Tag/Version/Release oder einem Cron-Zeitschleifenlauf).

Wie CI konkret funktioniert
------------

Die Grundlage von CI ist eine Konfigurationsdatei, in der wir das Verhalten und die Reaktion auf Ereignisse definieren. Im Falle von GitHub müssen Sie lediglich ein Hilfsverzeichnis `.github/workflows` erstellen und darin eine beliebige Datei mit der Erweiterung `.yml` erstellen. GitHub analysiert sie bei jedem Commit und führt sie bei Bedarf aus.

Nehmen wir an, wir wollen eine neue Konfigurationsdatei mit einer bestimmten Aufgabe definieren. Da es sich um eine Aufgabe handelt, die direkt von GitHub oder einer anderen Umgebung auf der virtuellen Maschine ausgeführt wird, müssen wir grundlegende Dinge über die Umgebung definieren, darunter:

- Der Name der Aufgabe (z. B. der Name des Tests)
- Trigger-Ereignisse (welches Ereignis die Aufgabe auslösen soll, z. B. jedes Mal, wenn Sie einen Push zum Repository durchführen oder eine Pull-Anfrage erhalten)
- Die Definition der Aufgaben selbst (es können viele Aufgaben parallel laufen)

Im Falle von Arbeitsplätzen definieren wir weiter:

- Name der Stelle
- Laufende Umgebung (normalerweise der Name des Betriebssystems)
- Schritte zum Bau des Containers

Der obige Container ist bereits die erste Vorbereitung für die **komplette Dockerisierung des Projekts**. Die grundlegende Verwendung von CI ist relativ einfach, und Sie müssen Docker überhaupt nicht verstehen, sondern nur die Befehle im Terminal kennen.

Als Teil der spezifischen Schritte in der Aufgabe müssen wir die einzelnen Befehle ausführen, die die Installation des soeben installierten Betriebssystems erstellen werden. Im Falle von PHP ist es also wichtig, nur die PHP-Sprache zu installieren (und die Version anzugeben), das Projekt-Repository in den Runner zu klonen, die Projektabhängigkeiten zu installieren (meistens [Composer](/composer)) und den Test selbst auszuführen.

Warum GitHub Actions (CI) verwenden?
--------

In der IT-Gemeinschaft herrscht der Aberglaube vor, dass Microsoft das böse Unternehmen ist, das minderwertige Produkte herstellt. Im Falle von GitHub Actions trifft dies jedoch überhaupt nicht zu, denn sie verfügen objektiv über die beste CI-Plattform der Welt.

Der grundlegende Vorteil von GitHub CI liegt in der Einfachheit und Eleganz der Notation. Mit nur wenigen Zeilen können Sie Ihre eigene Testumgebung konfigurieren und automatisch verwalten, auch ohne Vorkenntnisse. Gleichzeitig sehe ich in der Geschwindigkeit der Bearbeitung bestimmter Aufgaben einen großen Vorteil. So kann beispielsweise ein Test zu Codestyle (Codeformatierung) und PhpStan (Überprüfung von Datentypen, Logik und allgemeiner Korrektheit) von GitHub Actions für ein normales Projekt in weniger als 50 Sekunden ausgewertet werden, während GitLab denselben Test beispielsweise in fast 8 Minuten auswertet! Andere Lösungen wie Travis sind relativ teuer und die meisten Entwickler wissen die Vorteile ihrer speziellen Funktionen ohnehin nicht zu schätzen.

Vorbereitete Aktionen
-----

GitHub hat eine große Datenbank mit vorgefertigten Aktionen und Paketen, die Sie sofort verwenden können. Dabei handelt es sich in der Regel um Betriebssysteme, Programmiersprachen oder Bibliotheken aus der Community.

Sie finden eine Datenbank mit Aktionen direkt auf dem [Marktplatz] (https://github.com/marketplace?type=actions), meine Lieblingsaktion ist zum Beispiel [SMS bei Ausfall über Twillio senden] (https://github.com/marketplace/actions/twilio-sms).

Fertige Beispiele
------

Sehr viele Beispiele [die ich in meinen eigenen Repositories veröffentliche] (https://github.com/baraja-core), in denen Sie transparent sehen können, welche spezifische Konfiguration ich verwende.

Im Februar 2021 habe ich diese Konfiguration zum Beispiel für die Überprüfung des Codestils in einem Projekt verwendet:

```yaml
name: Coding Style

on:
  push:
    branches:
      - master
  pull_request:
    types: [ assigned, opened, synchronize, reopened ]
  schedule:
    - cron: '1 * * * *'

jobs:
  nette_cc:
    name: Nette Code Checker
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: shivammathur/setup-php@v2
        with:
          php-version: 7.4
          coverage: none

      - run: composer create-project nette/code-checker temp/code-checker ^3 --no-progress
      - run: php temp/code-checker/code-checker --strict-types --no-progress

  nette_cs:
    name: Nette Coding Standard
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: shivammathur/setup-php@v2
        with:
          php-version: 8.0
          coverage: none

      - run: composer create-project nette/coding-standard temp/coding-standard ^3 --no-progress --ignore-platform-reqs
      - run: php temp/coding-standard/ecs check src
```

Diese Konfigurationsdatei installiert das neueste Ubuntu (`runs-on: ubuntu-latest`), klont das Projekt-Repository (`uses: actions/checkout@v2`), installiert PHP (`uses: shivammathur/setup-php@v2`) in der Version 7.4 (`php-version: 7.4`), installiert das code-checker-Paket und führt dann den Job über PHP aus.

Ein paar weitere Hinweise für weniger erfahrene Benutzer:

- CI startet bei jedem Start eine virtuelle Maschine mit einem vollständigen Betriebssystem, das zum Aufrufen von Befehlen im Terminal verwendet werden kann, genau wie z.B. Ihr Server
- Zur Überprüfung der Testergebnisse werden sogenannte Return-Codes verwendet, die von Linux selbst definiert werden. Wenn ein Programm "0" (Null) zurückgibt, bedeutet dies Erfolg und jede andere Zahl einen Fehler. Shell-Skripte funktionieren beispielsweise auf die gleiche Weise
- Wenn wir einen benutzerdefinierten Test oder eine komplexere Logik schreiben wollen, können wir z.B. PHP verwenden und dies als CLI-Task ausführen (den Befehl `php file.php`), der dann ausgeführt wird und alles tun kann, wozu er Rechte hat (mit Dateien arbeiten, über das Internet kommunizieren, auf dem Bildschirm ausgeben, ...)
- Wenn CI läuft, wird die gesamte Ausgabe auf dem Bildschirm aufgezeichnet und kann direkt im Webbrowser angezeigt werden.
- CI ist nicht interaktiv, auch wenn Terminal interaktive Operationen mit dem Benutzer durchführen kann, unterstützt CI dies nicht und muss als Aufgabe konzipiert werden, die manchmal startet und manchmal beendet wird
- Für die Ausführung des CI-Containers ist eine Zeitüberschreitung von 1 Stunde festgelegt. Wenn in dieser Zeit nicht alles abgearbeitet wird (z. B. wenn das Programm stecken bleibt), wird das gesamte System zwangsweise beendet und ein Fehler gemeldet.
- GitHub kann CI-Aufträge automatisch per Cron ausführen, aber sehr oft werden sie nicht zum richtigen Zeitpunkt, sondern mit einer Verzögerung ausgeführt
- Sie können auch die gesamte CI-Logik in einen Docker-Container verpacken und ihn lokal auf allen Rechnern oder auf einem Server ausführen, zum Beispiel
- Wenn Sie auf geheime Informationen (z.B. Datenbankpasswort) zugreifen müssen, gibt es die Möglichkeit, geheime Variablen direkt in GitHub zu definieren und CI hat sie zur Laufzeit verfügbar
- Die Bereitstellung auf einem Webserver erfolgt am besten immer über SSH
- Die Gesamtlaufzeit der CI-Jobs ist begrenzt, normalerweise auf 2 Tausend Minuten pro Monat (sehr oft reicht das für Sie aus, ich habe es nie ausgeschöpft, weil ein Job maximal eine Minute läuft), oder Sie können die Zeit gegen eine Gebühr erhöhen
- Sie können auch CI-Runner auf Ihrem Server hosten und das Minutenlimit umgehen, aber höchstwahrscheinlich wird Ihr Server nicht viel schneller sein, da Microsoft stark in Azure investiert hat, wo GitHub Actions gehostet wird, und es läuft auf sehr leistungsstarken Servern
- Sehr oft ist es praktisch, bestimmte Pakete nur für CI zu installieren (typischerweise PhpStan und verschiedene Sicherheitsfunktionen), also fügen Sie sie in den Abschnitt `composer.json` der `require-dev`-Datei ein, damit sie nicht in einem bestimmten Projekt als obligatorische Abhängigkeit installiert werden

GitHub-Anwendungen und Bots
-----

Im Gegensatz zu anderen Repository-Hosts unterstützt GitHub sogenannte GitHub Apps und Automatisierungs-Bots. Dabei handelt es sich um eine große Datenbank mit vorgefertigten Bots, die automatisierte Operationen an Ihren Projekten durchführen können (auch ohne KI). Wenn sie auf etwas stoßen, werden sie zum Beispiel einen Fehler melden oder einen Pull Request mit einer Korrektur senden.

Ich persönlich benutze es und empfehle es Ihnen:

- [Dependabot](https://dependabot.com) - automatische Überprüfung von Abhängigkeiten in Composer, z.B. wenn eine neue Version der von Ihnen verwendeten Pakete herauskommt und kompatibel ist, wird automatisch ein Pull Request gesendet
- [Codecov](https://github.com/marketplace/codecov) - prüft die Codeabdeckung mit automatischen Tests und stellt grafisch dar, welche Teile des Codes noch nicht abgedeckt wurden
- [Snyk](https://github.com/marketplace/snyk) - prüft automatisch auf Sicherheitslücken
- [Imgbot](https://github.com/apps/imgbot) - automatische Optimierung der Bilddatengröße

Viele andere Anwendungen finden Sie auf dem [Marktplatz] (https://github.com/marketplace?type=apps).

Weitere praktische Tipps und Tricks
-----

Die Verwendung von Automatisierungsaufgaben in GitHub ist mir sehr ans Herz gewachsen.

Ich habe automatische Überprüfungen in all meinen Repositories eingerichtet, so dass ich (oder jeder andere in der Community) bei jedem Commit eine Echtzeit-Rückmeldung darüber erhalte, ob die Codequalität (Codestyle und PhpStan), die Sicherheit und mehr verletzt wurden.

Ich lasse einige Tests automatisch jede Stunde laufen. Wenn es zum Beispiel eine Sicherheitslücke oder ein CVE gibt, weiß ich spätestens in einer Stunde Bescheid, sogar auf der Ebene der einzelnen Projekte und was genau passiert ist.

Im Laufe der Zeit, nachdem ich verschiedene Konfigurationen getestet habe, bin ich zu dem Schluss gekommen, dass ich die folgenden Abhängigkeiten zu Composer als ideale Mindestkonfiguration betrachte:

- `phpstan/phpstan` - Überprüfung des Codes auf Richtigkeit und Logik
- `tracy/tracy` - Fehlerbericht und Protokollierung auf hoher Ebene
- `phpstan/phpstan-nette` - Phpstan Erweiterungen und Spezialitäten in Nette
- `spaze/phpstan-disallowed-calls` - eine brillante Bibliothek von Michal Spacek, die sich um die (Un-)Verwendung gefährlicher Dinge im Code kümmert (von vergessenen Variablendumps bis hin zur Möglichkeit, einen Benutzerstring als Shell-Befehl auszuführen)
- `roave/security-advisories` - prüft auf die Installation von unsicheren Versionen von Paketen (wenn eine Sicherheitslücke in einem bestimmten Paket gefunden wird oder eine CVE veröffentlicht wird, verhindert dieses Paket die Installation der beschädigten Version)

Idealerweise sollten Sie die grundlegende Sicherheitseinrichtung so einstellen, dass sie mindestens alle 8 Stunden automatisch ausgeführt wird und Fehler an Ihre E-Mail gesendet werden. Denn wenn eine Projektinstallation fehlschlägt oder eine neue Sicherheitslücke entdeckt wird, möchte ich so schnell wie möglich davon erfahren und sofort reagieren.

Denken Sie daran, dass alles automatisch funktioniert, so dass Sie immer sicher sein können, dass selbst wenn Sie komplizierte Prozesse zur Überprüfung der Sicherheit und anderer Dinge verwenden, dies keinen zusätzlichen Aufwand bedeutet und Sie nur eine gut durchgeführte Erstkonfiguration benötigen.

Denn in der Vorbeugung und Automatisierung liegt ein großes Potenzial!
