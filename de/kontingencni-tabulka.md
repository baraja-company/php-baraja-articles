Kontingenztabelle in PHP
========================

> id: '9bdc1004-8f06-48ec-8f56-8707fad5cef7'
> slug:
> 	cs: kontingencni-tabulka
> 	de: kontingenztabelle-in-php
> 
> publicationDate: '2019-11-13 22:00:05'
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1
> sourceContentHash: '80fdc1436cd30bc39ffe9c11c3d86c41'

Eine Kontingenztabelle wird im Allgemeinen verwendet, um die Beziehung zwischen zwei statistischen Phänomenen darzustellen. Bei der Entwicklung einer Webanwendung müssen wir oft die Beziehung eines bestimmten Phänomens in der Datenbank zu einer Zeitsequenz visualisieren, typischerweise in der Verwaltung.

Wir haben z. B. eine Auftragstabelle, in der einzelne Produkte aufgeführt sind, und sind daran interessiert, wie die Verkäufe bestimmter Produkte mit hohen Stückzahlen mit der Zeit zusammenhängen.

Dazu wäre eine Tabelle wie die folgende nützlich:

| Datteln | Äpfel | Erdbeeren | Birnen |
|---------|--------|--------|--------|
| 2019-05 | 10 | 15 | 18 |
| 2019-04 | 12 | 18 | 11 |
| 2019-03 | 13 | 9 | 21 |
| 2019-02 | 6 | 17 | 10 |
| 2019-01 | 7 | 4 | 6 |

Es gibt keine einfache Möglichkeit, die Daten in diesem Formular in PHP aufzubereiten, und sie direkt in dieses Formular in SQL zu übertragen, ist auch nicht elegant, da wir berücksichtigen müssen, dass es eine dynamische Anzahl von Spalten gibt.

Wir müssen also bei der Gestaltung der Ausgabe dieser Datenstruktur klug vorgehen.

Serialisierung von Daten mit Schlüsseln
----------------------------

Beim Aufbau einer Tabelle verwende ich oft die Funktion, alle Datensätze, die eine bestimmte Bedingung erfüllen, direkt aus der Datenbank abzurufen, z. B. Intervalldaten.

Konkret:

```sql
SELECT *
FROM `order`
WHERE `inserted_date` <= '2019-05-01'
ORDER BY `inserted_date` DESC
```

Die Abfrage ruft alle Spalten in der Tabelle `Order` ab, filtert alle Datensätze vom Beginn der Zeitalter bis zum ``2019-05-01`` und gibt sie sortiert vom neuesten zum ältesten zurück.

Mit einer einfachen SQL-Abfrage erhalten wir die Daten fast augenblicklich. Die zweite gute Eigenschaft ist, dass Datenbankindizes bei der Zusammenstellung der Ergebnisse effizient genutzt werden können. Da die Daten jedoch in einem einfachen Array vorliegen, müssen wir sie manuell in eine Datenstruktur serialisieren, die in eine Contig-Tabelle umgewandelt werden kann.

Da eine Kontingenztabelle die Beziehung zwischen zwei oder mehr Faktoren beschreibt, ist es sinnvoll, einen mehrdimensionalen Schlüssel zu verwenden. Da jedoch einige Daten möglicherweise nicht für alle Kombinationen vorhanden sind, ist es besser, den Schlüssel zu einem einzigen String zu serialisieren und die Daten als flaches Array zu speichern.

Die Daten können in einem einzigen Schleifendurchlauf zusammengestellt werden (die Variable `$selection` enthält die Ausgabe aus der Datenbank):

```php
$data = [];

foreach ($selection as $row) {
    $date = date('Y-m', $row->insertedDate); // Datum Jahr-Monat
    foreach ($row->items as $product) { // wir gehen durch die Produkte
        $key = $date . '_' . $product->id;
        if (isset($data[$key])) {
            $data[$key]++; // existiert, werden wir ein weiteres Produkt hinzufügen
        } else {
            $data[$key] = 1; // nicht vorhanden ist, beginnen wir mit dem ersten Produkt
        }
    }
}
```

Wenn wir eine einfachere Datenstruktur untersuchen würden, wäre eine innere Schleife zum Durchsuchen der Produkte nicht erforderlich. In diesem Fall könnte der gesamte Datenaufbau mit einem einzigen Zyklus gelöst werden.

Mit diesem Ansatz erhalten wir ein so genanntes flaches Array von Werten, das wie ein "Schlüssel: Wert" aussieht und zweidimensionale Informationen speichert.

Die Ausgabe ist dann z.B. (im `Mai 2019`, ein Produkt mit der ID `10` verkaufte `6` Einheiten):

```php
$data = [
    '2019-05_10' => 6,
    ...
];
```

Daten in eine Tabelle rendern - Vorlagen
-------------------------------

Wenn wir die Daten in Form eines flachen Arrays haben, können wir die gesamte Tabelle sehr einfach darstellen. Dazu müssen wir nur die Felder aller Produkte kennen, die uns interessieren, und die Felder aller Daten, für die wir die Tabelle erstellen wollen.

```php
$products = [ ... ]; // Produktfeld: id => Name
$dates = [ ... ]; // nach Datum: Datum => Etikett

echo '<table>';
foreach ($products as $productId => $productName) {
    echo '<tr>';
    foreach ($dates as $date => $dateLabel) {
        echo '<td>';
        echo htmlspecialchars(
            (string) ($data[$date . '_' . $productId] ?? '0')
        );
        echo '<td>';
    }
    echo '</tr>';
}
echo '</tabelle>';
```

Beachten Sie, dass beim Durchsuchen der Daten nach einem bestimmten Vorkommen durch Faltung des Zeichenkettenschlüssels gesucht wird. Auf diese Weise können wir die gerenderte Tabelle beliebig einschränken oder erweitern, je nachdem, welche Daten wir durchsuchen. Wenn die Daten nicht vorhanden sind, wird der ternäre Operator `??` ausgewertet und Null angezeigt.

Wir können das Array der verfügbaren Produkte und Daten als Teil des ersten Zyklus, der die Daten aufbereitet, erstellen. Dann können wir sicher sein, dass wir nur Daten aufzeichnen, die tatsächlich existieren. In diesem Fall ist es sehr wichtig, dass die Ausgabe aus der SQL-Datenbank nach dem Erstellungsdatum sortiert ist, da sonst die Zeilen beim endgültigen Rendern der Tabelle durcheinander geraten können.
