Mehrjähriges Formular
=====================

> id: '2abacddd-8a6b-4d25-a387-603fc7abc333'
> slug:
> 	cs: vicekrokovy-formular
> 	de: mehrjaehriges-formular
> 
> publicationDate: '2019-09-16 09:30:19'
> mainCategoryId: a23332c0-a233-4093-abd7-85b1b00a383b
> sourceContentHash: '69cb85c856478d588b96e9db9e695d97'

Manchmal müssen wir das Formular in mehrere Teile (Seiten) aufteilen, diese getrennt verarbeiten und dann zu einem Ergebnis zusammenfügen.

Dieser Artikel beschreibt Methoden und Entwurfsmuster, um dies zu erreichen.

> **Anmerkung:**
>
> Die Aufteilung eines Formulars in mehrere Schritte ist sehr komplex, besonders wenn man es gut machen will. Ich habe in meinem Leben viele Ansätze kennengelernt, die ich hier erörtern werde. Einige Ansätze sehen verlockend aus, sind aber naiv und funktionieren nur in bestimmten Fällen. Ich beschreibe für jeden Ansatz, wann er sinnvoll ist und welche Ansätze nicht sinnvoll sind.

Entwurf einer theoretischen Lösung
-------------------------

Normalerweise besteht das Ziel darin, die grundlegenden Daten aus dem ersten Formular auf der ersten Seite zu erhalten, sie zu validieren, sie dann "irgendwo" zu speichern und die nächste Seite anzuzeigen.

Wenn der Benutzer zur letzten Seite gelangt, muss das gesamte Formular abgeschickt und die Eingaben verarbeitet werden.

Bei jedem Schritt ist es wichtig, alle Daten sorgfältig zu überprüfen und dem Benutzer die Möglichkeit zu geben, die Seiten nach Belieben zurückzuspringen, damit er die Daten korrigieren kann, wenn er auf einen Fehler stößt. Wenn das Formular außerdem auf der Grundlage der bereits erhaltenen Daten bedingt gerendert werden soll, ist dies ein sehr anspruchsvoller Prozess.

Umsetzung der Formulare selbst
--------------------------------

Wir können die einzelnen Formulare entweder selbst in reinem HTML implementieren und dann die Verarbeitung in PHP übernehmen oder fertige Lösungen wie <a href="https://doc.nette.org/cs/3.0/forms">Nette forms</a> verwenden.

> **Beispiel aus dem Leben:**
>
> Sehr oft schicken mir unerfahrene Programmierer eine E-Mail und stellen scheinbar einfache Fragen, für die es eine fertige Lösung gibt. Zum Beispiel speziell zur Formularverarbeitung in PHP.
>
> Ich empfehle immer, auf eine manuelle Bearbeitung zu verzichten und stattdessen eine vorgefertigte Lösung zu verwenden. In der Realität ist es sehr kompliziert, z. B. die Validierung der eingegebenen E-Mail und die Übereinstimmung von zwei Passwörtern in zwei Feldern korrekt zu implementieren, während wir den Benutzer entsprechend seinen Daten und mit Fehlermeldungen im Falle eines Fehlers zum vorausgefüllten Formular zurückleiten wollen.
>
> Weil die Leute **nicht wissen, dass sie nicht wissen, dass sie nicht wissen**, und statt eine Stunde Zeit zu investieren, um eine fertige Lösung für 99,99 % der Probleme zu lernen, ziehen sie es vor, ihre eigene Lösung zu wählen, für die sie Dutzende von Stunden mit der Fehlersuche verbringen, und es gibt immer noch Fälle, in denen Formulare nicht funktionieren, Fehler auslösen, Sicherheitslücken haben und die eingegebenen Daten nicht schützen.

Das Ziel dieses Schrittes ist es also, mehrere Seiten unter verschiedenen URLs zu implementieren, die leere Formulare enthalten werden.

Ich empfehle, jedes Formular unabhängig von den anderen (atomar) zu implementieren und die Zustandsübergabe in einer anderen Anwendungsschicht zu behandeln. Der Grund dafür ist, dass jedes Formular in der Praxis die Datenvalidierung anders handhabt, seine Ausgaben anders schreibt, Fehler anders behandelt und wir es wahrscheinlich im Laufe der Zeit erweitern oder ändern wollen, so dass wir den Kontext des gesamten Prozesses nicht kennen und dafür Dutzende von Seiten ändern müssen.

Staatliche Übertragung
---------------

Bei der Verarbeitung des ersten Formulars wollen wir zunächst die empfangenen Daten überprüfen und, wenn sie korrekt sind, den Benutzer zum zweiten Schritt weiterleiten. Es ist eine gute Idee, die Weiterleitung als HTTP-Weiterleitung zu handhaben, da es leicht passieren kann, dass die Daten nicht gültig sind. In diesem Fall wollen wir den Benutzer zum ersten Formular zurückbringen und nicht zum nächsten Schritt.

Wir können Zustände grundsätzlich auf 4 Arten speichern:

**Nicht empfohlen:**

- Dies hat den Nachteil, dass der Benutzer die bereits in der URL gesendeten Daten einmalig verändern und so die Eingabe fälschen kann. Alternativ können wir sensible Informationen wie Passwörter in der URL offenlegen.
- **Kontinuierlich zu <a href="/sessions">sessions</a>** hinzufügen, d.h. schrittweise neu erfasste Daten nach Schlüssel in das Feld einfügen. Dies hat den Nachteil, dass der Benutzer im Falle eines Fehlers in der Anwendung mit den Sitzungen festhängt und den Fehler nicht mehr beheben kann (außer durch Löschen der Cookies, was für die meisten Menschen äußerst schwierig ist), und dass bei einem unvollständigen Formular die Gefahr besteht, dass die Daten ausgefüllt bleiben und von jemand anderem eingesehen werden können. Viel schlimmer ist es jedoch, wenn die Sitzung nur eine sehr kurze Gültigkeitsdauer hat (z. B. 5 Minuten) und der Benutzer beim Ausfüllen des letzten Schritts die Daten aus dem ersten Schritt verliert... das kann dann ziemlich ärgerlich werden.

**Empfohlen:**

- **Eintragung in die Datenbank und Übergabe der Kennung**. Beim ersten Absenden des Formulars werden alle gesammelten Daten in einer Datenbanktabelle gespeichert und ein zufälliger Bezeichner (z. B. eine 10 Zeichen lange Zeichenkette) generiert, der zwischen den Seiten als Parameter übergeben wird. Dies hat den Vorteil, dass wir bei der Verarbeitung eines beliebigen Formulars die neu abgerufenen und validierten Daten direkt in die Tabelle schreiben können, und dass wir im Falle eines Fehlers physische Sicherungen der geparsten Formulare haben und mit ihnen arbeiten können. Wenn zum Beispiel eine Bestellung unvollständig ist, können wir eine E-Mail an den Benutzer senden, dass er sie nicht abgeschlossen hat, und so die Chance auf einen Verkauf erhöhen.
- Die Funktion **Speichern unter dem aktuell angemeldeten Konto** funktioniert genauso wie die Weiterleitung über die ID, nur dass wir anstelle eines zufälligen Bezeichners (Token) eine Sitzung unter der ID des aktuell angemeldeten Benutzers (sofern vorhanden) verwenden. Der Vorteil ist, dass wir die vorausgefüllten Daten dem Benutzer in Zukunft beliebig anzeigen können.

Schlussfolgerung
-----

Keine dieser Lösungen ist perfekt oder die einzig richtige. Ich selbst kombiniere mehrere Ansätze, wenn ich an mehrstufigen Lösungen arbeite. Typischerweise löse ich z.B. einen Warenkorb als Datenbanktabelle, der ich die bereits gesammelten Daten zuordne und sie entweder an einen Benutzer (wenn er eingeloggt ist) oder an eine Sitzung (wenn er nicht eingeloggt ist und wir uns noch nicht kennen) binde.
