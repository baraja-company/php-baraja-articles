Veralterung des Codes - wie man die Kompatibilität aufrechterhält
=================================================================

> id: '7837ee7e-a150-47bf-a338-2f4c03d5bbed'
> slug:
> 	cs: zastaravani-kodu
> 	de: veralterung-des-codes-wie-man-die-kompatibilitaet-aufrechterhaelt
> 
> publicationDate: '2022-07-29 17:10:00'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '727b60c7258fb70e21b5acb445404db0'

Bei der Entwicklung großer Systeme (z. B. Unternehmensanwendungen, gemeinsam genutzte Softwarepakete, Bibliotheken usw.), bei denen mehrere Schichten und Entwickler miteinander kommunizieren, stellt sich das Problem, wie die Freigabe neuer Codeversionen zu handhaben ist.

Betrachten wir eine Beispielsituation, in der wir ein gemeinsames Composer-Paket für eine Gemeinschaft von Entwicklern entwickeln wollen.

Semantische Versionierung
--------------------

Bevor wir das Problem der Abwärts- und Vorwärtskompatibilität lösen können, müssen wir herausfinden, wie wir die Änderungen an der Software verfolgen können. Derzeit (2022) ist die beste Möglichkeit, alle Änderungen in Git zu versionieren. Das Software-Repository kann z. B. über GitHub oder GitLab freigegeben werden. Jede Softwareänderung hat eine eindeutige Kennung, die jeden Commit identifiziert und beschreibt, was tatsächlich passiert ist.

Die folgende Strategie hat sich bei der Entwicklung von Bibliotheken für mich bewährt:

Zu Beginn der Entwicklung wird ein erster Commit im `master` (oder `main`) Zweig erstellt, in dem die zugrundeliegende Dateistruktur übertragen wird.

Für jede neue Anfrage wird ein separater Zweig vom Master erstellt, in dem gearbeitet werden kann. Wenn die Änderung fertig ist, wird ein Antrag auf Zusammenführung an den Master in Form einer "Pull-Anfrage" gesendet. Die Anfrage wird einer Codeüberprüfung unterzogen, und wenn alles in Ordnung ist, wird die Änderung in den Master übernommen.

Enthält der Zweig eine rückwärts inkompatible Änderung (BC-Break, von `Back Compatibility Break`), muss dies entsprechend gekennzeichnet werden. Die Methode zur Kennzeichnung von BC-Pausen wird in den folgenden Kapiteln erläutert.

Die Produktionsversion der Bibliothek wird dann mit Tags gekennzeichnet, die die folgende Struktur haben (basierend auf **Semantic Versioning 2.0.0**):

Wir schreiben die Versionsnummer im Format `MAJOR.MINOR.PATCH`. Die Inkrementierung der Versionsnummern wird wie folgt vorgenommen:

- MAJOR" - wenn es eine Änderung gibt, die nicht abwärtskompatibel mit anderen ist (API)
- MINOR" - wenn Funktionalität unter Beibehaltung der Abwärtskompatibilität hinzugefügt wird
- PATCH" - wenn ein Fehler behoben wurde und die Abwärtskompatibilität erhalten bleibt

Durch die Verwendung von Vorabversionen und das Hinzufügen von Metadaten lassen sich die Informationen verfeinern. Zum Beispiel: "1.0.0-alpha", "1.0.1-beta+2".

Weitere Informationen zur semantischen Versionierung finden Sie auf der offiziellen Website: https://semver.org.

Rückwärts- und Vorwärtskompatibilität
-------------------------------

Bei der Entwicklung von Software sollten Sie immer an die **Abwärtskompatibilität** (neue Funktionen und Änderungen müssen mit altem Code kompatibel sein) und in einigen Fällen an die **Vorwärtskompatibilität** (aktuelle Funktionen müssen mit zukünftigen Änderungen der Schnittstelle kompatibel sein) denken.

Beide Aufgaben richtig zu erledigen, ist eine große Herausforderung. Es ist nicht immer möglich, eine Änderung vorzunehmen, ohne die Kompatibilität zu beeinträchtigen.

Wenn Sie Änderungen vornehmen, sollten Sie immer schrittweise vorgehen und den Benutzern genügend Zeit geben, auf die Änderungen zu reagieren.

In den folgenden Abschnitten wird beschrieben, wie man darüber nachdenken kann.

Stufe 1: Kennzeichnung eines Merkmals als veraltet
--------------------------------------

Die grundlegende Art der Kompatibilitätsbedrohung ist die Entfernung oder Umbenennung einer Funktion, die in der Vergangenheit existierte. Meistens liegt das daran, dass sich die Argumente, die die Funktion akzeptiert, geändert haben oder dass es sich um eine alte Logik handelt, die auf die neue Art und Weise anders gehandhabt werden sollte.

In der ersten Phase sollten die alten Teile des Codes als veraltet gekennzeichnet, aber in keiner Weise verändert werden.

In PHP gibt es dafür die Annotation `@deprecated`, die direkt über Methoden, Funktionen, Eigenschaften, Variablen, Konstanten und generell über allen veralteten Code geschrieben werden sollte.

Es ist auch gute Praxis, eine Begründung zu schreiben, warum eine bestimmte Sache veraltet ist und wie sie in Zukunft geändert wird. Geben Sie z. B. den Namen einer neuen Funktion oder Verwendungsmethode an.

Ein praktisches Beispiel für die Kennzeichnung von veraltetem Code: Konstanten werden entfernt, es ist besser, die eingebaute Enum (BC Pause aufgrund der Migration zu einer neueren Version von PHP) zu verwenden:

```php
class OrderNotification
{
	/** @abgelehnt seit 2022-05-24, verwenden Sie enum OrderNotificationType */
	public const
		TYPE_EMAIL = 'E-Mail',
		TYPE_SMS = 'Text';
```

Die Annotation "@deprecated" führt nur zu einer stillen Warnung für die IDE (Entwicklungswerkzeug) und die Kompilierungswerkzeuge. Sie macht nichts kaputt.

Phase 2: Aufrufen einer neuen Methode/Logik
--------------------------------------

In der zweiten Phase ersetzen wir die alte Implementierung durch die neue, verwenden aber die neue Methode in der alten Implementierung. So bleibt die Schnittstelle kompatibel, ohne dass der Benutzer es merkt.

Beispiel: Die Methode ist veraltet, weil stattdessen ein neuer statischer Dienst erstellt wurde. Da sie von anderen verwendet werden kann, wird sie einfach als veraltet markiert und ruft intern die neue Implementierung auf. Der Entwickler kann in der Regel davon ausgehen, dass die Methode in Zukunft vollständig abgeschafft wird.

```php
/** @veraltet seit 2021-09-11 stattdessen Ip::get() verwenden. */
public static function userIp(): string
{
	return Ip::get();
}
```

Phase 3: Anmerkungen für die statische Analyse ändern
-------------------------------------------

Wenn Sie eine statische Analyse wie PhpStan verwenden (sehr empfehlenswert!), ist es eine gute Idee, zuerst die PHPDoc-Annotationen umzuschreiben, bevor Sie die Datentypen tatsächlich ändern. Bei der statischen Analyse wird der Benutzer darauf hingewiesen, dass etwas nicht in Ordnung ist, aber die Laufzeit bleibt davon unberührt.

Stufe 4: Wegwerfen der Kündigung
-----------------------

In der vierten Phase wird eine neue Methode aufgerufen, und gleichzeitig wird ein Fehler der Stufe "Note" ausgelöst. Die Anwendung funktioniert weiterhin, sie beginnt nur, nach und nach Informationen im Systemprotokoll zu speichern, dass eine Funktion veraltet ist und geändert oder entfernt werden wird. Wir werden nun aktiv auf diese Art von Änderungen hinweisen. Der Entwickler wird während der Entwicklung oder Kompilierung Fehler feststellen.

```php
/** @veraltet seit 2021-05-01, stattdessen UserMetaManager verwenden. */
public function getMeta(int $userId, string $key): ?string
{
	trigger_error(__METHOD__ . 'UserMetaManager: Diese Methode ist veraltet, verwenden Sie stattdessen UserMetaManager.');
	return $this->userMetaManager->get($userId, $key);
}
```

Stufe 5: Auslösen einer Ausnahme
------------------------

Ich empfehle, eine der fatalen Ausnahmen auszulösen, bevor Sie die Methode vollständig entfernen. Dies ist besonders wichtig, da die Anwendung vollständig angehalten wird und der Fehler nicht ignoriert werden kann. Anders als bei der vollständigen Entfernung des Codes wird der Benutzer darüber informiert, was tatsächlich passiert ist, und kann den Fehler leicht beheben.

Stufe 6: Vollständige Code-Entfernung
-----------------------------

In der letzten Phase wird der alte Code vollständig entfernt. Wenn ein Benutzer die Abhängigkeiten nicht behoben hat, wird seine Anwendung nicht funktionieren.

Schwerwiegende BC-Brüche in sensiblen Bereichen sollten immer in der nächsten `MAJOR'-Version erfolgen und mindestens eine `MAJOR'-Version früher durch einen Hinweis darauf aufmerksam gemacht werden. Wenn Sie dies nicht tun, wird die Aktualisierung der Bibliothek äußerst schwierig.
