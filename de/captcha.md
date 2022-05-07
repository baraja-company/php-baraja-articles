Wie Captcha funktioniert (beschreibendes Bild)
==============================================

> id: '9cf191e4-a49b-407b-b16e-21de7224ac43'
> slug:
> 	cs: captcha
> 	de: wie-captcha-funktioniert-beschreibendes-bild
> 
> perex:
> 	- 'Captcha je druh obrázku, který ověří, že uživatel není robot a chrání webovou aplikaci. Možnosti implementace v PHP.'
> 	- 'Ein Captcha ist eine Art Bild, das verifiziert, dass der Benutzer kein Roboter ist und die Webanwendung schützt. PHP-Implementierungsoptionen.'
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: '80357b2e7f0047bb488922f2ab7b4336'

Captcha ist derzeit eine der gängigsten Methoden zum Schutz freier Formate. Ursprünglich wurde es nicht zum Schutz der Datensicherheit geschaffen, sondern zum Schutz vor Spam und um zu erkennen, dass es sich um einen Menschen handelt.

Da es jedoch maschinell erzeugt wird, ist es nicht immer perfekt, aber die Erfolgsquote eines Captcha-Tests liegt bei etwa 99 %, und nur 1 % der Bilder kann von einem gut gemachten Spam-Roboter entschlüsselt werden.

Wie es funktioniert
--------------------------

Es gibt noch mehr Möglichkeiten, ich verwende zum Beispiel diese Lösung:

- Der Server erhält die Information, dass der Benutzer eine Formularseite angefordert hat, und beginnt mit deren Erstellung.
- In das künftige Captcha-Testfeld wird ein Bild mit einer geheimen Adresse eingefügt, die nichts aussagt (wie eine Zufallszahl, eine Zeichenfolge, ... was auch immer).
- Das Bild wird unter dieser Adresse erzeugt und gespeichert. Gleichzeitig wird die korrekte Abschrift irgendwo geschrieben (vielleicht in eine Sitzung oder eine Datenbank).
- Wenn der Benutzer das Formular abschickt, wird geprüft, ob die Abschrift mit dem übereinstimmt, was er/sie eingegeben hat. Ist dies der Fall, wird das Formular verarbeitet. Falls nicht, wird eine erneute Beschreibung eines anderen Bildes angefordert.
- Sowohl nach erfolgreichen als auch nach erfolglosen Versuchen wird das temporäre Captcha-Bild gelöscht (und kann nie wieder verwendet werden). Wird das Formular nicht innerhalb einer bestimmten Zeit eingereicht, wird es ebenfalls gelöscht (verfällt).

Korrekte Transkription
--------------------------

Wenn der Captcha-Test gelöst werden kann, ist der Benutzer wahrscheinlich ein Mensch. Es ist jedoch gut, an Benutzer zu denken, die die Aufgabe nicht lösen können, insbesondere an blinde Benutzer. Es ist eine gute Lösung, um mehrere mögliche Tests zu kombinieren (insbesondere Voice Prefetching). Allerdings ist die maschinelle Spracherkennung derzeit wesentlich effizienter als das Ablesen von Bildern. Daher ist diese Lösung nicht immer ideal.

Benutzerdefiniertes einfaches Captcha-Bild
--------------------------

Oft reicht es aus, ein leeres Bild mit bestimmten Abmessungen zu erzeugen und einige Zeichen ohne weitere Bearbeitung lesbar einzugeben. Ernsthaft! Die meisten Spam-Bots sind dumm und können generische Formulare mit dieser Art von Schutz nicht angreifen, auch wenn es sich um perfekt lesbaren Text handelt, den eine bessere OCR perfekt transkribieren kann.

Das resultierende Bild kann wie folgt aussehen:

```txt
<img src="captcha.php" alt="ukázková captcha">
```

Versuchen Sie, die Seite ein paar Mal zu aktualisieren, und Sie werden sehen, dass sich der Code jedes Mal zufällig ändert. Zu Demonstrationszwecken wird sie nur generiert, ohne zu speichern, so dass sie sofort nach dem Laden wieder entfernt wird.

Ich habe den Quellcode mit Hilfe der PHPGD-Bibliothek gelöst, die auf praktisch jeder PHP-Installation und jedem Hosting verfügbar ist:

```php
Header("Inhaltstyp: image/png");
$obr = ImageCreate(100, 35);
$pozadi = ImageColorAllocate ($obr, 219, 28, 49); //Hintergrundfarbe definieren
$bila = ImageColorAllocate ($obr, 255, 255, 255); //Weiße Farbdefinition für Text
$styl = array ($pozadi);
ImageSetStyle ($obr, $styl);
  $nahodne_cislo = rand(11111,99999); //Ziehen einer Zufallszahl mit 5 Zeichen Länge
  imagestring($obr, 5, 25, 10, $nahodne_cislo, $bila); //Funktion zum Zeichnen von Text (in diesem Fall eine Zahl)
ImagePNG($obr); //Erzeugung des Bildes im Speicher und Rendering
ImageDestroy($obr); //Löschen des Bildes aus dem Speicher (es wird nicht mehr benötigt, da es einmal erstellt wurde)
```

Das Rendering des Bildes ist dann nur noch eine Frage von HTML:

```html
<img src="captcha.php">
```

Beachten Sie, dass dieses Skript ohne weitere Modifikationen nicht funktionsfähig ist. Es dient nur zur Demonstration, um ein einfaches Bild zu erzeugen.

Zusammenfassung
--------------------------

Captcha-Tests sind recht zuverlässig, aber lästig und zeitaufwändig. Manchmal sind sie nicht lesbar. Dann ist es gut, wenn man dem Benutzer die Möglichkeit gibt, mit Hilfe von Javascript ein anderes Bild zu laden, damit nicht die ganze Seite neu geladen und alles neu ausgefüllt werden muss.

Erinnern Sie sich: Was für einen Menschen eine triviale Aufgabe ist, kann für eine Maschine niemals eine realisierbare Lösung sein. Deshalb kann sogar dieses primitive Captcha mit perfekt lesbarem Text vor mehr als der Hälfte des Spams schützen.
