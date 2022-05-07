Die Caesar-Chiffre - wie sie funktioniert
=========================================

> id: '662dd627-c106-406e-ad6a-7ae9a492ad92'
> slug:
> 	cs: cesarova-sifra
> 	de: die-caesar-chiffre---wie-sie-funktioniert
> 
> publicationDate: '2019-08-23 15:43:02'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '892ad0746be469b5cdbb4de72d8e85b9'

**Warnung:** Dieser Artikel wurde vor vielen Jahren geschrieben und einige Informationen sind möglicherweise veraltet oder falsch. Bitte beachten Sie dies beim Lesen.

Die Caesar-Chiffre ist eine der einfachsten Hashing-Funktionen. Zu seiner Zeit war er praktisch unknackbar, aber im Zeitalter der modernen Computer dauert es nur noch ein paar Dutzend Sekunden oder sogar ein paar Minuten, ihn zu knacken. Es basiert auf einem Schlüssel, nach dem die Nachricht verschlüsselt wird und nach dem sie dann wieder expandiert werden kann. Der Schlüssel ist also geheim. Zum Zeitpunkt der Verschlüsselung kann die Nachricht gesehen werden und hat keine Bedeutung (nur ein Durcheinander von Zeichen). Die einzige Möglichkeit, die Chiffre zu knacken, besteht darin, den Schlüssel zu erraten.

Der Schlüssel
--------------------------

Der Schlüssel kann eine beliebige ganze Zahl sein, die weniger Ziffern hat als die Nachricht selbst. In der Regel werden 3 gültige Ziffern angegeben (es gibt also 999 Kombinationen). Jede zusätzliche Ziffer erhöht die Sicherheit. Damit zwei Parteien miteinander kommunizieren können, müssen beide Parteien diesen geheimen Schlüssel kennen (sie müssen ihn also irgendwie sicher aneinander weitergeben). Wenn der Schlüssel nur ihnen und nicht dem anderen bekannt ist, kann die Nachricht auch auf unsichere Weise verbreitet werden, ohne dass der Inhalt gefährdet wird, da der potenzielle Angreifer nicht weiß, wie er die Nachricht zurückbekommt.

Zu Demonstrationszwecken verwende ich den Schlüssel **123**, normalerweise wird eine andere Zufallszahl verwendet, von der man annimmt, dass sie nicht so leicht zu erraten ist.

Verfahren zur Verschlüsselung
--------------------------

Das Prinzip beruht auf der Idee, die Zeichen einer Nachricht mit Hilfe eines Schlüssels durch andere zu ersetzen. Ich nenne das "Charakterverschiebung".

Nehmen wir zum Beispiel die folgende Nachricht, die wir verschlüsseln wollen:

```php
TAJNA ZPRAVA
```

Ordnen Sie nun jedem Zeichen eine Nummer zu. In der Regel nach dem Alphabet (seiner Seriennummer). Ich werde das englische Alphabet verwenden, also diese Reihe von Zeichen:

```php
A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
```

Wenn wir jedem Zeichen eine Nummer zuordnen, erhalten wir etwas wie:

```php
20-1-10-14-1   26-16-18-1-22-1
```

Jetzt kommt der Schlüssel. Wir nehmen jede einzelne Nummer und fügen ihr den Schlüssel hinzu. Ich habe farblich hervorgehoben, was ich wo hinzugefügt habe:

`1 2 3`

```php
21 3 13 15 3   3 17 20 4 23 3
```

Beachten Sie, dass ich beim Verschlüsseln des Zeichens **Z** die Ziffer 3 schreibe. Das liegt daran, dass **Z** das letzte Zeichen des Alphabets ist. Wenn es das Ende erreicht, wird also wieder vom Anfang der Zeile an gezählt.

Übermittlung der Nachricht
--------------------------

Jetzt können wir die Nachricht auf jede beliebige Weise weiterleiten. Manchmal sogar öffentlich. Andere werden nur eine unlogische Zahlenreihe sehen, so dass sie wahrscheinlich nicht einmal wissen, dass es sich um eine Art von Chiffre handelt. Die Sicherheit wird auch durch den Schlüssel selbst erhöht, der geheim ist. Manchmal ist es zweckmäßig, die Nachricht als Zahlenreihe zu übermitteln, manchmal ist es zweckmäßig, diese Zahlen in Zeichen umzuwandeln (wiederum in der gleichen alphabetischen Reihe) und dann eine Zeichenfolge zu übermitteln. Vieles hängt von den Umständen ab. Im Allgemeinen ist die Ziffernfolge jedoch besser, da nur wenige Menschen vermuten werden, dass es sich um eine verschlüsselte Nachricht handelt.

Entschlüsselung beim Empfänger
--------------------------

Der Empfänger entschlüsselt nach demselben Verfahren. Es nimmt jedes einzelne Zeichen und subtrahiert die Zahlen entsprechend dem Schlüssel und konvertiert dann die resultierenden Werte zurück in Zeichen unter Verwendung des Alphabets. Es handelt sich lediglich um ein umgekehrtes Verschlüsselungsverfahren. Wichtig ist, dass Sie den **Schlüssel** und den **Zeichensatz** kennen, d. h. die Reihenfolge der Zeichen.

Knacken und Entschlüsseln
--------------------------

Das einzig mögliche Verfahren zum Knacken besteht darin, alle denkbaren Kombinationen aller möglichen Schlüssel auszuprobieren. Wenn wir die Länge des Schlüssels nicht kennen, wird der ganze Prozess noch komplizierter. Aber im Allgemeinen können die heutigen Computer etwa 100 Schlüssel in einer Sekunde ausprobieren, so dass es etwa eine Minute dauert, einen 3 Zeichen langen Zufallsschlüssel zu knacken.

Ist der Schlüssel jedoch so lang oder länger als die ursprüngliche Nachricht, kann er nicht geknackt werden, da jedes einzelne Zeichen einen eigenen Schlüssel hat, so dass alle Kombinationen für jedes Zeichen ausprobiert werden müssen.

Wenn ich eine Nachricht "**NACHRICHTEN**" hätte, wäre sie 11 Zeichen lang (Leerzeichen zählen nicht). Wenn ich einen Schlüssel bräuchte, der ebenfalls 11 Zeichen lang ist, und ich eine Zeichenkette mit 26 Zeichen des englischen Alphabets verwenden würde, dann gäbe es **11^26** = 1,191817654*10²⁷ Kombinationen, der durchschnittliche Computer würde diesen Schlüssel in 1,310999419×10²⁶ Sekunden = 10^20 Tagen knacken :)
