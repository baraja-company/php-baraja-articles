Escaping-Zeichen in einer Zeichenkette in PHP
=============================================

> id: '40f9361e-e286-4b5e-a0c0-1f6cda8106af'
> slug:
> 	cs: escapovani
> 	de: escaping-zeichen-in-einer-zeichenkette-in-php
> 
> publicationDate: '2019-11-26 11:56:52'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '026284252541bc9c918a87298b9e261c'

Escaping wird verwendet, um Zeichen zu schreiben, die in verschiedenen Kontexten unterschiedliche Bedeutungen haben.

Wir wollen zum Beispiel ein weiteres Anführungszeichen in eine von Anführungszeichen eingeschlossene Zeichenfolge einfügen. Wie kann man das tun?

Es gibt 2 Möglichkeiten:

```php
echo "Levi's-Jeans"; // Kombination von Anführungszeichenarten

echo 'Levi\s Jeans'; // Backslash-Escaping
```

Escaping ist auch wichtig, wenn Variablen in eine HTML-Vorlage geschrieben werden, wo der String-Inhalt in einem anderen Kontext stehen und etwas Besonderes bedeuten kann.

Wenn wir also zum Beispiel HTML-Code auflisten (den wir in einer Variablen haben), müssen wir die Auflistung behandeln, sonst wird der HTML-Code ausgeführt.

Zum Beispiel:

```php
$message = 'Hallo <b>Tommy!</b>';

echo $message; // Falsch!

echo htmlspecialchars($message); // Richtig :)
```

Das Thema Flucht ist sehr komplex und ich empfehle die Lektüre des Artikels <a href="https://phpfashion.com/escapovani-definitivni-prirucka">Flucht - Der endgültige Leitfaden</a> von David Grudel.
