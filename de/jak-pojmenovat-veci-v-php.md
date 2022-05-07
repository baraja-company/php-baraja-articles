Wie man Variablen, Funktionen, Methoden und Klassen benennt
===========================================================

> id: f70ac75d-422f-4696-88d5-9e1b843e060a
> slug:
> 	cs: jak-pojmenovat-veci-v-php
> 	de: wie-man-variablen-funktionen-methoden-und-klassen-benennt
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1'
> sourceContentHash: '10510114a0160c0826c9ee1f54c95b03'

Um den Code in Ordnung zu halten, ist es wichtig, klare Regeln für die Ableitung von Namen zu wählen. Diese Seite bietet einen Überblick über die relativ populären Ansätze, die von vielen Programmierern, darunter auch von mir und Leuten, mit denen ich zusammenarbeite, verwendet werden.

Wenn Sie in einem Entwicklungsteam arbeiten, sollten Sie auf jeden Fall deren Regeln anwenden, aber für die allgemeine Entwicklung ist es genauso vorteilhaft, einige gute Gewohnheiten zu etablieren.

Da das Konzept der gesamten PHP-Syntax sehr breit gefächert ist, habe ich den Leitfaden hier in viele Kategorien unterteilt, die sehr spezialisiert sind.

Wenn Sie nach einer schnellen Lösung suchen, empfehle ich Ihnen das Studium der <a href="https://www.php-fig.org/psr/psr-4/">Norm PSR-4</a>.

Einfügen von Skripten, Verlinkung mit HTML, Laden von Dateien
---------------------------------------------------

Jedes Skript muss mit dem `<?php`-Tag beginnen.

Wenn am Ende der Datei **kein HTML** steht, sollte die Datei nicht beendet werden (um ein weißes Zeichen am Ende der Seite zu vermeiden).

Das Laden anderer Dateien sollte nach den folgenden Regeln erfolgen:

- Ist die Datei unkritisch und kann beim Rendern der Seite entfallen (z.B. zusätzliche Informationen in der Fußzeile), sollte sie per include geladen werden: `include 'file.php';`
- Kritische Dateien nur über das require-Konstrukt: `require 'file.php';`
- Wenn wir mit Objekten arbeiten, verwenden wir das require-Konstrukt, um den Autoloader zu laden, der sich selbst um das Laden der anderen Klassen kümmert (die meisten Frameworks implementieren dieses Verhalten).


Wenn es zumindest ein wenig möglich ist, sollten wir die Datenabruflogik vom Rendering in HTML trennen, d.h. das MVC-Modell (Model - View - Controller) verwenden.

Wir bereiten die Daten also zunächst in Presenter vor:

Präsentator.php

```php
$cisla = [1, 2, 3];

$data = [];
$data['Zahlen'] = $cisla; // Übergabe der Daten an die Vorlage
include 'renderCisel.php'; // Vorlage laden
```

Und dann zeichnen Sie es in die Vorlage ein:

`renderCisel.php`

```html
<table>
<?php
    foreach ($data['cisla'] as $cislo) {
        echo '<tr><td>' . $cislo . '</td></tr>';
    }
?>
</table>
```

Dieser Ansatz wird von den meisten Frameworks verwendet und ist nützlich, um den Code übersichtlicher zu gestalten. Im späteren Verlauf des Entwicklungsprozesses sollte dieser Ansatz von jedem erfahrenen Programmierer verwendet werden, der klar strukturierte Anwendungen entwickeln will (bei großen Anwendungen ist dieser Ansatz ein Muss).

Namen der Variablen
----------------

Wenn eine Variable eine Reihe von Werten oder anderen Objekten enthält, sollte sie im Plural benannt werden:

```php
$numbers = [1, 2, 3];
```

Denn dann können wir die Werte einfach durch eine einzige Zahl iterieren:

```php
foreach ($numbers as $number) {
    // Zahlenverarbeitung
}
```

Ein Name, der aus mehreren Wörtern besteht, wird mit der cameCase-Syntax zu einem langen Wort zusammengefasst, d. h. das erste Wort beginnt mit einem Kleinbuchstaben, jedes weitere Wort mit einem Großbuchstaben:

```php
$promenna = 'Hey! Hey!';
$seznamUzivatelu = [
   'Jan Barášek',
   'Barack Obama',
   'Steve Jobs',
   'Stephen Wolfram',
];
$maxFilesInDirectory = 12;
$nameOfPhpScript = 'index.php'; // Das PHP-Kürzel wurde verkleinert
```

Funktions- und Methodennamen
--------------------

Funktionen und Methoden sollten in ihrem Namen immer deutlich machen, was sie tun. Häufig können auch die erwarteten Eingabeparameter und der Rückgabewert in den Namen aufgenommen werden.

Versuchen Sie zu erraten, was die folgenden Funktionen tun und welchen Rückgabewert sie haben:

```php
getUserById($id);
saveErrorToLog($message);
createDefaultDirectory($path);
setAuthors(['Jan Barášek', 'Chuck Norris']);
getCurrentTime();
```

Der ganze Trick liegt im ersten Wort des Namens, das deutlich macht, welche Methode die Funktion verwenden wird. In der Regel wird die folgende Konvention eingehalten:

- `get` - Daten als Array oder Objekt abrufen, Eingabeparameter spezifizieren die gesuchte Entität
- `save` - in einer Datei oder Datenbank speichern
- create" - eine Entität erstellen (z. B. eine Instanz eines Objekts erstellen)
- Set" - Speichern von Daten in einer vordefinierten Variablen (innerhalb einer Funktion)

Namen der Klassen
----------

Eine Klasse ist eine große Einheit, die eine große Anzahl von Eigenschaften und Methoden enthält, daher sollte sie auch mit einem Großbuchstaben beginnen. Eine Klasse sollte auch nur eine Entität enthalten (und deren Eigenschaften beschreiben), daher sollte sie mit einem Singularnamen benannt werden. Wenn wir mit mehreren Entitäten arbeiten müssen, können wir einfach jede Instanz in einem Array speichern.

Beispiel:

```php
class User
{
    public string $username;

    public string $password;

    public string $role;
}

class Users
{
    /** @var Benutzer[] */
    public array $users;

    public function addUser(User $user): void
    {
        $this->users = array_push($this->users, $user);
    }
}
```

Die Klasse **User** ist auf Informationen über einen bestimmten Benutzer spezialisiert. Wenn wir mit mehreren Benutzern arbeiten wollen, erstellen wir eine weitere Klasse (envelope), die eine Reihe von Instanzen einer bestimmten Entität enthält.

Auch Fabriken können in diesem Zusammenhang oft nützlich sein, da sie es uns ermöglichen, ähnliche Objekte zu erstellen und die ursprünglichen Instanzen wiederzuverwenden, was zu einem klareren Code führt und gleichzeitig Systemressourcen spart.

Namensraum - Namespaces
---------------------------

Obwohl Namespace unabhängig von dem physischen Verzeichnis ist, in dem das Skript verfügbar ist, ist es eine gute Praxis, das Layout des Projekts zumindest teilweise zu berücksichtigen (was zu einem besseren System für die Erstellung neuer Namen führt, das auf diese Weise eindeutiger ist).

Ich persönlich benenne Namespaces nach dem gemeinsamen Unterverzeichnis für Klassen dieses Typs.

Beispiele:

```php
App\Presenters; // Dies sind die Namen aller Vortragenden
App\Model;      // Dies ist der Name des allgemeinen Modells
App\Model\Math; // Dies ist der Name des Modells, das mit Mathematik arbeitet
```

Für ein korrektes automatisches Laden von Klassen ist es ratsam, den <a href="https://jakpsatphp.cz/PSR4/">PSR-4-Standard</a> zu befolgen.

Benutzerdefinierte Konventionen (zum Beispiel in einem Firmenteam)
-----------------------------------------

Und wie nennen Sie Ihre? Ich würde mich über Tipps freuen, wie ich diesen Artikel verbessern kann.

Im Allgemeinen sind jedoch benutzerdefinierte Konventionen innerhalb eines Teams nicht sehr sinnvoll, da sie die Portierung des Codes auf andere Frameworks erschweren, und wenn Sie einen neuen Kollegen einstellen, muss dieser die aktuellen Konventionen lernen. Daher ist es am besten, die "PSR-4"-Norm zu befolgen.
