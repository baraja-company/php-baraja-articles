Sicherheit in PHP und Dateianhang einbeziehen
=============================================

> id: '820f8de6-ff1e-406c-8fe5-95080642656f'
> slug:
> 	cs: bezpecnost-include
> 	de: sicherheit-in-php-und-dateianhang-einbeziehen
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '1207d637c20fcb5e8f609be2eb449135'

Oft möchten wir eine Datei an eine Seite anhängen, die wir irgendwo anders auf der Festplatte gespeichert haben. Wenn wir den genauen Namen direkt in die Attach-Funktion eingeben, gibt es keinen Grund zur Sorge.

Sicheres Anhängen einer Datei
--------------------------

```php
include 'menü.html';
```

Das vorherige Schreiben ist absolut sicher, da wir immer dieselbe Datei mounten. Ein Sicherheitsfehler kann in diesem Fall nicht auftreten. Das einzige Problem, das auftreten kann, ist das Fehlen der Datei **menu.html**, was eine Warnmeldung auslöst (die wahrscheinlich ohnehin nicht erscheint), aber diese Situation ist selten, da wir normalerweise eine Datei anhängen, deren Existenz praktisch sicher ist.

Anhängen einer Datei nach dem Muster
--------------------------

Was aber, wenn wir zum Beispiel Artikel an eine einfache Inhaltsseite wie diese anhängen wollen? Auf dieser Website habe ich einen physischen Ordner, in dem die Artikel im HTML-Format gespeichert sind, und ich hänge sie direkt an den Quellcode an.

Es reicht jedoch nicht aus, einfach nur eine Verbindung herzustellen! Ein Anfänger kann einzelne Artikel wie diesen aufrufen:

```php
include 'Artikel/' . $_GET['Artikel'] . '.html';
```

> Aber das ist extrem gefährlich. Ein Angreifer kann einen Link zu einem anderen Verzeichnis übergeben, indem er `../` oder etwas Ähnliches als Artikelnamen verwendet, und manchmal ist es möglich, die Endung loszuwerden, indem ein Null-Byte am Ende übergeben wird. Sie sollten zumindest die Funktion `basename()` verwenden, aber besser ist es, nur Werte aus der Whitelist zuzulassen.

Warum nicht auch nicht relevante Dateien laden?
--------------------------

Oft stört es uns nicht, wenn eine falsche (unerwartete) Datei geladen wird - es ist der Fehler des Benutzers, wenn er eine Seite anfordert, die er eigentlich nicht haben will, aber es kann Situationen geben, in denen es wichtig ist. Im Besonderen:

- Der Benutzer lädt eine Datei, auf die die Öffentlichkeit keinen Zugriff hat und auf die nur der Server zugreifen kann.
- Das Laden eines anderen PHP-Skripts kann eine unerwartete Aktion oder Fehlermeldung auslösen, die einen Hinweis auf die Funktionsweise der Website geben und zu weiteren Angriffen führen kann.
- Das Laden einer anderen Datei kann nicht nur dazu führen, dass sie dem Dokument hinzugefügt wird, sondern auch, dass sie ausgeführt wird.

Whitelist und Eingabevalidierung
--------------------------

Wenn wir nicht in der Lage sind, Eingaben auf sichere Weise zu validieren (z. B. anhand einer Whitelist), sollten wir uns nicht auf die Ehrlichkeit des Benutzers verlassen und Skripte zumindest auf PHP-Ebene verteidigen.

Zunächst ist es wichtig, alle geladenen Dateien in denselben Ordner (Verzeichnis) zu legen und einige gefährliche Zeichen zu deaktivieren, insbesondere Schrägstrich und Punkt. Dadurch wird es unmöglich, auf andere Ordner zuzugreifen, die potenziell gefährdete Dateien enthalten. Gefährliche Zeichen können auch durch einfaches Löschen aus der Eingabezeichenkette ausgeschaltet werden.

```php
$load = '../index'; // diese Eingabe könnte potentiell gefährlich sein
$load = strtr($load, './', ''); // löscht alle Punkte und Schrägstriche aus der Zeichenkette
include $load .'.html';
```

Laufende Dateien?
--------------------------

Es ist wichtig zu beachten, dass das **include**-Konstrukt die Dateien beim Einbinden genauso ausführt, als ob es sich um PHP-Code handeln würde, daher ist es eine gute Idee, diese Möglichkeit zu berücksichtigen.

Oftmals werden jedoch Dateien angehängt, die anschließend nicht ausgeführt werden müssen, und wir sind nur an dem gespeicherten Text (Inhalt) in Form einer Zeichenkette interessiert. Daher können wir die Datei in eine Variable laden und mit ihr als String arbeiten, was ziemlich sicher ist.

```php
$load = '../index'; // diese Eingabe könnte potentiell gefährlich sein
$load = strtr($load, './', ''); // löscht alle Punkte und Schrägstriche aus der Zeichenkette
$file = file_get_contents($load . '.html'); // Laden des Inhalts in die Variable
echo $file; // Auslesen des Inhalts der Datei
```

Diese Lösung sieht auf den ersten Blick interessant und sicher aus - und sie ist sicher. Selbst wenn es dem Benutzer gelingt, eine PHP-Datei aufzurufen, wird sie nicht ausgeführt. Allerdings kann er ihn (d. h. seinen Quellcode) anzeigen, und da müssen wir vorsichtig sein.

Erkennen der gewünschten Datei im Skript
--------------------------

Hierfür gibt es keinen endgültigen Leitfaden, das muss jeder selbst tun, je nach den Erfordernissen des Drehbuchs. Ich erkenne zum Beispiel einen Artikel von anderen Dateien daran, dass sie eine Überschrift der Größe H1 enthalten. Wenn also jemand eine Datei lädt, in der es keine Überschrift gibt, zeige ich nichts an und die Seite endet mit einer Fehlermeldung. Es ist immer wichtig, ein einzigartiges Merkmal zu finden, das nur die gewünschten Dateien haben und das die anderen Dateien nicht haben.

Schlussfolgerung
--------------------------

Obwohl das Validieren und Laden von Dateien relativ einfach ist, machen viele Anfänger immer noch Fehler - und werden es auch weiterhin tun. Das Wichtigste ist, dass wir die Bedeutung dessen, was wir laden, richtig einschätzen und den gewünschten Inhalt vom Rest unterscheiden können. Und das Wichtigste: Arbeiten Sie mit dem Inhalt als String und laden Sie ihn nie direkt in die Seite.
