Refactoring eines PHP-Altprojekts - wie man technologische Schulden aufholen kann
=================================================================================

> id: '8f37305a-e9dd-4b1e-9f53-04f0fca648f7'
> slug:
> 	cs: refaktoring-legacy-php-projektu
> 	de: refactoring-eines-php-altprojekts---wie-man-technologische-schulden-aufholen-kann
> 
> publicationDate: '2021-05-11 20:45:00'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: b13673b23a86bc82a88636e4a9464b4b

Bei der Beratung von sachkundigen und erfahrenen Projektverantwortlichen stoße ich häufig auf die Frage nach der langfristigen Nachhaltigkeit eines digitalen Projekts. Viele große Projekte, die über 3 Jahre Entwicklungszeit hinausgehen, werden intern veraltet und sind nicht mehr tragfähig - stellen Sie sich ein Team von Entwicklern mit unterschiedlichem Wissensstand, Erfahrung und vor allem Fleiß vor.

Um Ihr digitales Projekt technisch auf dem TOP-Niveau zu halten, brauchen Sie:

- Kompetente Entwickler und ein Projektmanager**, die gut miteinander kommunizieren und jeder weiß, was er tut und wie es sich auf die anderen auswirkt,
- **Funktionale Werkzeuge und Arbeitsabläufe**, die das gesamte Team kennt und seine Arbeit entsprechend an andere anpasst,
- **Automatisierte Aufgaben**, die für eine tägliche Routine sorgen, die man so leicht vergisst, sind extrem wichtig.

Wenn Sie ein großes Projekt entwickeln, haben Sie es einfach nicht leicht. Es ist wichtig, die Dinge richtig zu machen, aber noch wichtiger ist es, die richtigen Dinge zu tun.

Was ist Refactoring und warum brauchen Sie es?
---------------------------------------

Das Konzept des Refactoring umfasst eine Reihe von Aktivitäten, die das Erscheinungsbild und die interne Logik eines Programmcodes verändern, ohne die Umgebung zu beeinträchtigen. Man kann sich den Refactoring-Prozess wie einen Weihnachtsputz vorstellen - es bleibt alles beim Alten, aber es ist viel organisierter, und das unnötige Zeug wird weggeworfen. Beim Refactoring ist es auch wichtig, sich vor Augen zu halten, dass es sich nicht um ein einmaliges Ereignis handelt, sondern um einen langfristigen Prozess, den man in regelmäßigen Zyklen wiederholen muss. Wenn Sie das nicht tun, riskieren Sie allerlei seltsame Fehler, finanzielle Verluste, Probleme beim Onboarding und eine Menge Unmut.

Wenn es Ihnen gelingt, das Projekt stabil und auf dem neuesten Stand zu halten, haben Sie große Vorteile davon:

- Das Projekt wird **sicherer** sein. In den von Ihnen verwendeten Bibliotheken werden regelmäßig Patches für Bugs und Sicherheitslücken veröffentlicht. Sie müssen sich mit der Sicherheit befassen, sie ist eine der grundlegenden Lebensfunktionen eines gesunden Projekts.
- Der Code und die interne Architektur werden einfacher und besser durchdacht sein. Sie werden besser in der Lage sein, Fehler zu finden, und viele Fehler können sogar verhindert werden.
- Mit einem vereinfachten Code können Sie auch jüngere Entwickler hinzuziehen, um Ihr Gesamtbudget <a href="https://www.youtube.com/watch?v=1wZtdIb-Ppk">deutlich zu reduzieren</a>.
- Vielleicht stellen Sie fest, dass Sie einige Dinge zu kompliziert und zu kompliziert machen - das Projekt wird insgesamt vereinfacht werden.

Um bei einem großen digitalen Projekt auf Kurs zu bleiben, muss man laufen. Aber Sie wollen wachsen.

Wie man sicher refaktorisiert
-------------------------

Jedes Refactoring ist eine große Wette, die sich möglicherweise nicht auszahlt.

Ich sorge immer für eine stabile Umgebung, lange bevor ich mit dem Refactoring beginne.

Für die Stabilität brauchen Sie vor allem eine **funktionale Fehlerprotokollierung**. Für einfache Projekte benötigen Sie nur <a href="https://tracy.nette.org">Tracy</a>, das Fehler in einer HTML-Datei protokolliert. Für fortgeschrittenere Projekte kommen Werkzeuge wie <a href="https://sentry.io/welcome/">Sentry</a> oder <a href="https://rollbar.com">Rollbar</a> ins Spiel. Ich persönlich verwende Tracy bei allen Projekten, andere Werkzeuge je nach Art des Projekts.

Etwa einen Monat vor dem Refactoring beginne ich mit der Fehlerverfolgung und erstelle eine Tabelle mit bekannten Fehlern, auf die ich mich später konzentrieren kann. Es ist immer wichtig zu wissen, welche Fehler durch das Refactoring eingeführt wurden und welche das Projekt bereits in der Vergangenheit enthielt. Dies wird dazu beitragen, die Ergebnisse der Arbeit gegenüber dem Kunden besser zu verteidigen.

Sobald ich dank des Protokolls weiß, dass ich in einer relativ stabilen Umgebung arbeite, können wir uns an die eigentliche Arbeit machen. Andere Rubriken enthalten Beschreibungen von Methoden, je nachdem, wie viel Risiko sich dahinter verbirgt.

Festlegen von Kodierungsstil und Kodierungsstandard
-----------------------------------

Die meisten Projekte haben keinen einheitlichen Formatierungsstil für den Code. Das ist ein großer Fehler. Ein gut geschriebenes Projekt sieht aus wie das Projekt einer Person, bei dem alle Dinge gleich geschrieben sind.

Grundlegende Formatierungsfehler werden von dem <a href="https://doc.nette.org/en/3.1/code-checker">Nette Code Checker</a> Tool, das ich regelmäßig über das gesamte Projekt laufen lasse, gut korrigiert.

Der Kodierungsstil hingegen bezieht sich auf die Formatierung des Codes und die Einrückung der einzelnen Sprachausdrücke. Der <a href="https://www.php-fig.org/psr/psr-12/">PSR-12</a>-Standard wird in der PHP-Welt häufig verwendet, und viele andere Standards basieren auf ihm. Ich empfehle die Verwendung.

Einige Projekte kombinieren auf ungeschickte Weise <a href="/spaces-and-tabs">Tabs und Leerzeichen</a>. Um effektiv zu verhindern, dass die Whitespace-Konvention (und andere Fehler) gebrochen wird, erstellen Sie eine `.editorconfig`-Datei im Stammverzeichnis des Projekts, die Ihrem Editor mitteilt, wie er neu geschriebenen Code formatieren soll.

Ich persönlich verwende diese Konfiguration:

```txt
root = true

[*]
charset = utf-8
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true

[*.{php,phpt}]
indent_style = tab
indent_size = 4
```

Außerdem sollten alle <a href="/apostrophes-and-carriage-notes">Carriage-notes in Apostrophe umgewandelt werden</a>. Dies kann automatisch geschehen.

Sie können auch die Formatierung Ihres Codes automatisch überprüfen. Ich habe dafür eine <a href="https://github.com/baraja-core/sandbox/blob/0db1f41068b6cedf79500c6a8b0ac26eae94a8eb/.github/workflows/coding-style.yml">voll funktionsfähige Demo auf GitHub</a> vorbereitet. Wenn Sie den Code auch automatisch korrigieren müssen, können Sie dies mit demselben Werkzeug tun. PhpStorm ist auch sehr gut darin, Code direkt und automatisch zu korrigieren, sogar in großen Mengen für das gesamte Projekt.

Wie man den Kodierungsstil für große Projekte festlegt
----------------------------------------------

Bei großen Projekten empfehle ich, die automatische Codeformatierung modulweise vorzunehmen, da dies zu einer großen Anzahl von Konflikten in Git führen kann. Daher ist die beste Strategie, so viele Zeilen wie möglich mit einem einzigen großen Commit zu korrigieren, diesen Commit direkt in den Master zu verschieben und die Änderung dann auf die anderen Zweige zu verteilen.

Wenn die Konflikte zu groß sind, ist es sinnvoll, den Code zuerst auf dem Master zu formatieren und dann noch einmal auf jedem großen Zweig, an dem es viele Änderungen gibt. Sehr viele geänderte Zeilen heben sich gegenseitig auf (spulen Sie vor), so dass Sie nur sehr wenige oder manchmal gar keine Konflikte lösen werden.

Wenn Sie nichts tun, wird der Code trotzdem schlecht sein. Iterative Umschreibungen über viele Jahre hinweg funktionieren definitiv nicht, da bei jeder Zusammenführung Dutzende von Zeilen korrigiert werden müssen, in denen eigentlich keine Änderung stattgefunden hat, und dies sowohl die Überprüfung des Codes als auch die potenzielle Rücknahme von Änderungen unnötig erschwert.

PhpStan - statische Code-Analyse und Typ-Fehlerkorrektur
------------------------------------------------------

Bevor man sich an eine groß angelegte Logikkorrektur macht, ist es sehr wichtig, **grundlegende Merkmale des Projektlebenszyklus** zu überprüfen und zu korrigieren. Dazu gehören so offensichtliche Dinge, die man von jedem Projekt erwartet, die aber trotzdem manchmal nicht erfüllt werden.

Zum Beispiel:

- Alle Klassen, Methoden und Funktionen müssen existieren
- Klassenvererbung darf nicht gebrochen werden
- Die Klassen müssen die verwendete Schnittstelle oder Vorgängerschnittstelle implementieren. Gleichzeitig dürfen Sie keine "finalen" Klassen erben.
- Sie dürfen keine unsicheren Funktionen wie `eval()`, `shell_exec()`, `var_dump()` und so weiter aufrufen. Und wenn Sie sie trotzdem anrufen, muss dies ausdrücklich im Kommentar angegeben werden
- Sie müssen immer Ausnahmen abfangen und dürfen nicht die gesamte Anwendung zum Absturz bringen.

Die Lösung für dieses Problem besteht darin, <a href="https://phpstan.org">PhpStan</a> im Projekt zu installieren und es **mindestens auf Stufe 1** zu fixieren. Ja, es ist hart, und ja, es ist eine Menge Arbeit. Aber wenn man das nicht tut, wird jedes Refactoring zum russischen Roulette, und der Entwickler hofft einfach, dass der Schaden so gering wie möglich ist.

Die Basisebene von PhpStan setzt nicht voraus, dass z.B. Datentypen und andere formale Dinge überall vorhanden sind, worauf wir in Zukunft eingehen werden. Andererseits kann es vorkommen, dass eine Funktion oder Methode zwar einen bestimmten Datentyp zurückgibt, aber im typehint etwas anderes behauptet.

Rector - Sichere iterative Reparatur
-----------------------------------

Ein bekanntes Programmierdiktum besagt, dass man, bevor man dem System einen neuen Fehler hinzufügt (z. B. das Auslösen einer Ausnahme), diesen Fehler zunächst als Warnung hinzufügen und ihn erst dann in einen schwerwiegenden Fehler umwandeln sollte, wenn er sich lange Zeit nicht bemerkbar macht.

Mit Datentypen verhält es sich genauso. Beim Refactoring von unbekanntem Code lasse ich automatische Tools wie <a href="https://getrector.org">Rector</a> zuerst Kommentare in den Code einfügen, was nichts kaputt macht, aber hilft zu klären, was später wo sein wird. Diese Kommentare werden dann von PhpStan abgehört, mit dessen Hilfe sicher überprüft werden kann, dass wir nichts kaputt gemacht haben und es sich um eine sichere Änderung handelt.

In der Regel füge ich in einem Commit Kommentare zu Eigenschaften und Argumenten hinzu, warte dann vielleicht einen Monat, und wenn langfristig alles funktioniert, schreibe ich an den Stellen, an denen es langfristig keine Probleme gab, auf feste Datentypen um.

Siehe den Artikel <a href="https://tomasvotruba.com/blog/2019/07/29/how-we-completed-thousands-of-missing-var-annotations-in-a-day/">Wie wir Tausende von fehlenden @var-Anmerkungen an einem Tag vervollständigten</a>.

Mehr über <a href="https://github.com/rectorphp/rector">Rector</a> finden Sie auf GitHub</a>.

Aktualisieren von Abhängigkeiten
----------------------

Bevor man sich mit der Aktualisierung von Abhängigkeiten von Paketen oder sogar PHP-Versionen befasst, ist es wichtig zu recherchieren und zu lernen, wie man <a href="/github-actions-best-ci-for-2021">automatisierte Tests</a> verwendet.

Wenn die Tests funktionieren, können wir das Tool <a href="/composer">Composer</a> aufrufen, um die Aktualisierung durchzuführen. Wenn Sie können, sollten Sie die Hilfe eines <a href="https://dependabot.com">Dependabot</a>-Roboters in Anspruch nehmen, der die `composer.json` Ihres Projekts automatisch aktualisiert und auf Kompatibilitätsänderungen prüfen kann.

Wenn Sie eine große Anzahl von Änderungen vor sich haben, sollten Sie diese immer langsam vornehmen und Version für Version erhöhen. Aktualisieren Sie niemals mehrere Pakete auf einmal. Scannen Sie nach jeder Aktualisierung das gesamte Projekt mit PhpStan und beheben Sie Fehler. Es ist ein langwieriger Prozess, der mehrere Stunden dauert, aber es steht viel auf dem Spiel.

Wohin als nächstes?
-------

Die nächsten Schritte hängen von der Art und dem Status des Projekts ab. Im Allgemeinen wird gut durchdachter Code, der von erfahrenen Entwicklern geschrieben wurde, um Größenordnungen besser gewartet als Code, der billig von einem Junior gekauft wurde, der bei einer Medienagentur gearbeitet hat, die die Webentwicklung sozusagen als Nebenerwerb betreibt.

Drücken Sie die Daumen! Es wird hart sein, aber du wirst es schaffen.
