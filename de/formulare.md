HTML-Formulare - Teil im Browser
================================

> id: cb0015c7-b7b6-41ac-8263-4068960e16b3
> slug:
> 	cs: formulare
> 	de: html-formulare---teil-im-browser
> 
> perex:
> 	- 'Zpracování formulářů v PHP, zejména možnosti odeslání získaných dat na server metodou GET a POST.'
> 	- 'Verarbeitung von Formularen in PHP, insbesondere die Möglichkeit, die erhaltenen Daten mit den Methoden GET und POST an den Server zu senden.'
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: a23332c0-a233-4093-abd7-85b1b00a383b
> sourceContentHash: '8d2a0df0667b3da5d6b19e6c25ad2517'

Bevor wir die Benutzerdaten auf der Serverseite über PHP verarbeiten können, müssen wir sie zunächst erhalten. Dies geschieht im Browser über HTML-Formulare, die die grundlegenden Elemente für den Empfang der Daten festlegen. In diesem Artikel sollen nicht alle Möglichkeiten von Formularen vorgestellt werden, sondern nur die grundlegenden Möglichkeiten der Datenübernahme und des Verständnisses des Prinzips.

Grundlegende HTML-Formularquelle
-----------------------------

```html
<form action="script.php" method="get">

<!-- Zde bude celý obsah formuláře -->

</form>
```

Jedes Formular beginnt mit dem HTML-Tag "<form>" und endet mit dem Tag "</form>". Alle Formularfelder, die sich zwischen diesen Tags befinden, werden übermittelt.

Als Nächstes müssen Sie mit dem Attribut `action` (Skriptname) festlegen, wohin das Formular gesendet werden soll, und mit dem Attribut `method`, welche Methode verwendet werden soll (GET oder POST). Wenn die Methode und das Ziel nicht angegeben werden, sendet sich das Formular standardmäßig mit der GET-Methode.

Grundlegende Formularfelder
-------------------------

Das am häufigsten verwendete Feld wird verwendet, um den Text (String) zu erhalten. Jedes Feld hat seinen eigenen Typ und Namen, an dem es nach der Übermittlung erkannt werden kann.

Gemeinsame Textfelder
------------------

Am wichtigsten ist, dass ich ein einfaches Textfeld benötige:

```html
<input type="text" name="food">
```

<input type="text" name="food">

Feld Passwort
---------------------

```html
<input type="password" name="heslo">
```

<input type="password" name="password">

Kontrollkästchen
--------

Es wird verwendet, um den Booleschen Wert (`TRUE` und `FALSE`) zu prüfen:

```html
<input type="checkbox" name="vop" checked="checked">
```

<label>
	<input type="checkbox" name="vop" checked="checked"> Sind Sie mit den Bedingungen einverstanden?
</label>

Optionsfeld zur Auswahl mehrerer Optionen
------------------------------------

```html
<input type="radio" name="language" value="cz" checked> Čeština
<input type="radio" name="language" value="sk"> Slovenština
<input type="radio" name="language" value="en"> Angličtina
```

Sie können aus mehreren Optionen wählen. Die ausgewählte Option sendet ihren Wert. Standardmäßig ist es gut, ein Feld mit dem Attribut `checked="checked"` auszuwählen:

<label>
	<input type="radio" name="language" value="cz" checked="checked"> Tschechisch
</label><br>
<label>
	<input type="radio" name="language" value="en"> Slowakisch
</label><br>
<label>
	<input type="radio" name="language" value="en"> Englisch
</label>

Großes Textfeld
------------------

Erstellt für die Eingabe von mehrzeiligem Text. Sie wird auch zur Eingabe verwendet:

- `cols` ~ Anzahl der Spalten
- rows" ~ Anzahl der Zeilen

```html
<textarea name="article" cols="40" rows="6">
Ahoj lidi!
</textarea>
```

<textarea name="artikel" cols="40" rows="6">
Hallo Leute!
</textarea>

Auswahlbox
---------

Bietet eine bequeme Möglichkeit, aus vielen Daten auszuwählen.

```html
<select name="gender">
	<option value="man">Muž</option>
	<option value="woman">Žena</option>
</select>
```

Nach dem Absenden des Formulars wird der Wert in "value" gesendet.

<select name="gender">
	<option value="man">Männlich</option>
	<option value="Frau">Frau</option>
</select>

Schaltfläche "Senden
---------------------

Das Formular kann eine unbegrenzte Anzahl von Übermittlungsschaltflächen enthalten. Sie sind leicht zu erfassen:

```html
<input type="submit" value="Odeslat">
```

Wenn Sie darauf klicken, werden alle Daten aus den Formularfeldern übernommen und an das eingestellte Skript gesendet:

<input type="submit" value="Submit">

Datenverarbeitung auf dem Server
-------------------------

Als Nächstes müssen Sie die Daten an den Server senden und sie dort verarbeiten. Dies wird im <a href="/processing-formula-in-php">nächsten Artikel</a> behandelt.
