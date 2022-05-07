Das Prinzip der Einkapselung in PSA
===================================

> id: '54968a42-b678-4385-91ac-c13ba96c9b34'
> slug:
> 	cs: zapouzdreni
> 	de: das-prinzip-der-einkapselung-in-psa
> 
> publicationDate: '2020-02-16 21:21:35'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '3fbce6cdcbf5f274312496604569d9c2'

Eines der Hauptprinzipien der OOP ist das **Kapselungsprinzip**, das besagt, dass komplexe Probleme in viele kleine Probleme zerlegt werden sollten, die wir unabhängig und gleichzeitig lösen können. Gleichzeitig ist es uns als Nutzer egal, wie das geschieht, und die Daten (der interne Zustand) bleiben isoliert.

Wenn wir zum Beispiel das Problem lösen, wie wir das Ergebnis "1,6" auf der Grundlage einer Benutzerabfrage mit dem Ausdruck "(5+3)*(2/(7+3)" zurückgeben können, kann wahrscheinlich niemand von uns eine einzige Funktion oder Methode schreiben, die dieses Problem auf einmal löst.

**TIP:** Eine fertige Lösung für diese Art von Beispiel finden Sie in dem Artikel <a href="/pokrocila-kalkulacka">Verarbeitung eines mathematischen Ausdrucks als String</a>, aber seien Sie darauf vorbereitet, dass es nicht einfach ist.

Verkapselung bringt Abstraktion über Objekte
-----------------------------------------

Mit der Kapselung können Sie Objekte "als Benutzer" verwenden, d. h. ihre Methoden aufrufen und sich keine Gedanken darüber machen, wie sie intern funktionieren.

Nehmen wir an, es geht um die Berechnung des Gehalts eines Mitarbeiters und wir wollen eine bestehende Klasse eines anderen Programmierers dafür verwenden. Wir müssen nur die obligatorischen Konstruktorparameter kennen und können die Klasse "einfach verwenden":

```php
$mzda = new MzdaZamestnance(
    25000, // Bruttogehalt
    6,     // Anzahl der Jahre im Unternehmen
    10,    // Anzahl der Jahre der Erfahrung
    true   // ist es ein Mann?
);

echo $mzda->getHruba(); // 25000

echo $mzda->getCista(); // 17800
```

Die Parameter des Objekts sind fiktiv und entsprechen nicht der realen Berechnung des Lohns. Das Prinzip wird insbesondere dadurch veranschaulicht, dass wir **nur die allgemeine öffentliche Schnittstelle kennen müssen** und uns nicht einmal mit dem **internen Zustand des Objekts**, nicht einmal mit der **internen Implementierung** und schon gar nicht damit beschäftigen müssen, **warum es so funktioniert, wie es funktioniert**. Wir rufen einfach die Methode `getCista()` auf und erhalten die Nettoauszahlung.

Verkapselung ist eine Frage des Designs
----------------------------

Es ist wichtig zu beachten, dass **Einkapselung selbst kein Merkmal oder eine Syntax der Sprache** ist. Die Tatsache, dass eine Klasse und eine Anwendung gekapselt sind, ist nur eine Frage des Programmierers, der die Anwendung entwirft und über den Code nachdenkt.

Betrachten Sie den Entwurf einer Klasse immer auf diese Weise:

- KISS (keep it simple), halten Sie die Schnittstelle einfach und zwingen Sie den Benutzer nicht, unnötig zu denken. Lösen Sie die komplexe Logik für den Benutzer und er wird es Ihnen danken.
- Der Benutzer der Klasse (ein anderer Programmierer oder Sie in der Zukunft) braucht die interne Logik überhaupt nicht zu kennen, und die Methodennamen und ihre Parameter sollten ausreichen.
- Wenn ich für die Berechnung Hilfsberechnungen benötige, die für den Benutzer uninteressant und rein technisch sind, macht es keinen Sinn, dafür überhaupt einen Getter zu erstellen und sie sollten nur intern berechnet werden.
- Die Klasse muss die grundlegenden Eigenschaften des Algorithmus erfüllen, insbesondere, dass er im Allgemeinen für beliebige Daten funktioniert.
- Öffentlich zugängliche Methoden sollten so konzipiert sein, dass sie genügend Informationen liefern, um das Objekt in der Zukunft leicht um neue Funktionen zu erweitern, so dass wir neue Daten leicht aus dem berechnen können, was wir bereits wissen.

Interne Daten nicht öffentlich machen
-------------------------------

Für Eigenschaften und Methoden, die interne Logik behandeln, ist es sinnvoll, die Sichtbarkeit auf "privat" zu setzen. Der Hauptvorteil dabei ist, dass sie nicht von außen aufgerufen werden und der Benutzer gezwungen ist, die von Ihnen entworfene Schnittstelle zu verwenden, wodurch die Daten und der interne Zustand des Objekts geschützt werden.

Nehmen wir zum Beispiel ein Objekt, das ein Bankkonto darstellt, auf dem wir Zahlungen buchen und den aktuellen Saldo bearbeiten wollen:

```php
class BankAccount
{
    private int $sum;

    public function __construct(int $startSum)
    {
        $this->sum = $startSum >= 0 ? $startSum : 0;
    }

    public function getSum(): int
    {
        return $this->sum;
    }

    public function pay(int $price): void
    {
        $newSum = $this->sum - $price;
        if ($newSum < 0) {
            throw new \Exception('So viel Geld haben Sie nicht!');
        }

        $this->sum = $newSum;
    }

    public function addMoney(int $money): void
    {
        $this->sum += $money;
    }
}
```

Beachten Sie, dass die Klasse nur eine einzige "private" Eigenschaft "$sum" enthält, die den aktuellen Saldo angibt.

Wenn wir den aktuellen Kontostand abfragen wollen, gibt es dafür eine Methode "getSum()", aber wir haben keine Möglichkeit, den neuen Kontostand zu ändern. Wir können nur mit der Methode `pay()` Geld abheben oder mit der Methode `addMoney()` Geld hinzufügen.

Dank dieses Prinzips können wir immer sicher sein, dass niemand das Objekt zerstören kann.

Wenn der Benutzer versucht, mehr Geld zu bezahlen, als tatsächlich auf dem Konto vorhanden ist, lässt die Methode "pay()" dies nicht zu, da sie eine Kontrollberechnung durchführt, bevor sie die Eigenschaft "$sum" überschreibt, und wenn der Saldo negativ sein sollte (weniger als Null), wird eine Fehlerausnahme ausgelöst und der Vorgang abgebrochen.

Schlussfolgerung
-----

Wir haben das Grundprinzip der Kapselung demonstriert, das es uns ermöglicht, besser über die Abstraktion von Objekten nachzudenken und eine völlig neue Perspektive zu eröffnen.

Wenn Sie dieses Prinzip erst einmal verstanden haben, werden Sie sehen, dass <a href="/proc-use-frameworks">Frameworks sehr viel Sinn machen</a>, denn sie kapseln intern viel Cleveres, das Sie einfach verwenden können.

Das nächste Mal befassen wir uns mit <a href="/Würdigkeit-und-Sichtbarkeit">Würdigkeit und Sichtbarkeit</a>.
