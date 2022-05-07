PHP 8 ist da - vollständiger Überblick
======================================

> id: '8b6ce751-195f-41d2-82c6-1af4be3e86b5'
> slug:
> 	cs: php-8-kompletni-prehled-novinek
> 	de: php-8-ist-da---vollstaendiger-ueberblick
> 
> publicationDate: '2020-11-26 11:53:54'
> mainCategoryId: '17545205-215b-4962-b910-0d67ad1e933a'
> sourceContentHash: f657145ac1d2a1109fcc2a1ff3b6e6cf

Heute, am 26. November 2020, wurde nach mehreren Jahren die neue Hauptversion von PHP 8 veröffentlicht, die eine ganze Reihe von neuen Funktionen enthält. Dies ist eine der größten Aktualisierungen seit langem und verdient einen eigenen Artikel.

In diesem Artikel fassen wir alle wichtigen neuen Funktionen und die Unterschiede in der Syntax und den Optionen im Vergleich zur älteren Version zusammen. Die meisten der neuen Funktionen sind abwärtskompatibel und bringen Verhaltensverbesserungen mit sich, die Sie genießen werden.

> **Wichtige Informationen:** PHP 8 befindet sich jetzt in einer "Feature Freeze"-Phase, was bedeutet, dass keine neuen Verhaltensweisen mehr hinzugefügt werden können und nur noch Bugs behoben werden. So können Sie sich auf Kompatibilität verlassen und Ihre Anwendungen vollständig debuggen.

Unionsarten
----------

PHP im Allgemeinen hat sich in den letzten Jahren von einer rein dynamischen Sprache, in der jede Variable alles enthalten kann, zu einer strikten Form entwickelt, in der wir im Voraus wissen, welcher Datentyp in welcher Variable, welchem Parameter, welchem Argument oder welcher Eigenschaft enthalten sein wird. Die Verwendung von [data-types](/datove-types) ist immer noch optional, aber ich empfehle die Verwendung von starker Typisierung und verwende sie selbst bei allen Projekten.


Union-Typen drücken eine Sammlung mehrerer Typen aus, die jedes Argument oder jede Eigenschaft in ihnen akzeptieren.

Zum Beispiel:

```php
function validatePsc(string|int $psc): bool
{
	// Durchführung
}
```

Die Funktion `validatePsc()` in der Variablen `$psc` akzeptiert die Datentypen `string` (String) und `int` (Integer).

In der vorherigen Version von PHP 7.4 war diese Notation nicht möglich und wurde üblicherweise durch [comment](/document-comment) umgangen:

```php
/**
 * @param string|int $psc
 */
function validatePsc($psc): bool
{
	// Durchführung
}
```

Dieser Kommentar wird jedoch von PHP ignoriert (schließlich ist es ein Kommentar), und wir mussten eine zusätzliche Prüfung mit einem externen Tool wie PhpStan durchführen, was viele Entwickler ignorierten. Jetzt wird die Prüfung direkt zur Laufzeit (wenn die Anwendung läuft) durchgeführt und kann nicht umgangen werden.

PHP kennt jedoch eine bestimmte Art von Union-Typ seit Version 7, als es möglich war zu sagen, dass der Haupttyp auch `nullable` sein kann, d.h. er akzeptiert den Hauptdatentyp plus den Wert `null`.

Dies wurde auf zwei Arten geschrieben, die jeweils eine andere Bedeutung haben:

```php
function setPhone(?string $phone): void
{
	// Durchführung
}

// oder

function setPhone(string $phone = null): void
{
	// Durchführung
}

// oder Kombination

function setPhone(?string $phone = null): void
{
	// Durchführung
}
```

Alle Einträge besagen, dass das Telefon `int` (Ganzzahl) entweder ein `String` oder `null` ist.

- Die erste Notation erfordert immer die Übergabe des Wertes
- Bei der zweiten Schreibweise muss kein Wert übergeben werden; wird nichts übergeben, ist der Standardwert "null" (dies ist ein optionales Argument)
- Der dritte Eintrag ist eine Kombination der Optionen und verhält sich wie der zweite Eintrag

Bei der Verwendung von Vereinigungstypen können wir keine Notation mit einem Fragezeichen mehr verwenden und müssen z. B. den Datentyp `null` streng definieren:

```php
function setPhone(string|int|null $phone = null): void
{
	// Durchführung
}
```

Die Telefonnummer muss nun `string`, `int` oder `null` sein.

Union-Typen haben immer noch eine Reihe von Verwendungsmöglichkeiten, über die fortgeschrittene Entwickler in der Dokumentation oder der Implementierung spezifischer Bibliotheken nachlesen können.


JIT - schnellere Skriptverarbeitung
----------------------------------

Der JIT-Compiler (Just-in-Time-Compiler) sorgt für eine erhebliche Verbesserung der Leistung bei der Skriptverarbeitung (Parsen und Verstehen). Dieses Verhalten kann jedoch im Zusammenhang mit Webanfragen variieren.

Sie können nun in der Tracy-Leiste innerhalb des Nette-Frameworks sehen, ob Sie JIT aktiviert haben. Weitere Einzelheiten finden Sie in [separater Artikel] (https://stitcher.io/blog/php-jit).

Was man über die Kompilierung im Allgemeinen sagen kann, ist, dass PHP versucht, den Code im Voraus zu verarbeiten, so dass es bei der Verarbeitung einer bestimmten Anfrage nicht erst eine physische Skriptdatei durchgehen, parsen und interpretieren muss. In der Vergangenheit wurde dies über die OPCache-Erweiterung gehandhabt (die bei Servern und Hosts standardmäßig verfügbar ist), was die Verarbeitungsgeschwindigkeit um etwa die Hälfte verbesserte.

Als Faustregel gilt, dass es bei einer langsamen Anwendung immer besser ist, einen geeigneten Algorithmus für eine bestimmte Aufgabe zu wählen, als Mikrooptimierungen im Code vorzunehmen. Normalerweise werden die großen Verzögerungen durch das Warten auf die Datenbank und ihre langsamen Abfragen, das Speichern von Sitzungen, das Warten auf freien Festplattenspeicher und andere Hardwareoperationen verursacht.

Nullsicherer Operator (optionale Verkettung)
-------------------------------------

In einer realen Anwendung ist es sehr oft notwendig, das Vorhandensein eines Rückgabewerts zu überprüfen (dass er nicht `null` ist) und dann bedingt eine andere Methode aufzurufen. Die [ternären Operatoren](/ternary-operator) eignen sich hervorragend dafür, aber sie funktionieren nur mit einer Bedingung und können nicht verschachtelt werden. Der Nullsafe-Operator ermöglicht von Haus aus eine Verschachtelung.

> **TIP:** Praktisch das gleiche Verhalten wird bereits vom Latte-Templating-System unterstützt, aber es überschreibt diese Art von Syntax im nativen PHP-Code, so dass Sie den nullsafe-Operator in älteren PHP-Versionen (ab PHP 7) verwenden können. Herzlichen Glückwunsch an David für diese Änderung!

Das macht die Nutzung einfach:

```php
$orderId = $order?->getId();
```

Die Variable `$orderId` enthält entweder den von der Methode `getId()` zurückgegebenen Wert oder `null`, wenn die Variable `$order` den Wert `null` enthält und die Methode `getId()` nicht aufgerufen werden konnte.

Diese Art von Problem wurde in PHP 7 durch die folgende Syntax über den ternären Operator umgangen:

```php
$orderId = isset($order) ? $order->getId() : null;
```

Möglicherweise eine Bedingung:

```php
if (isset($order)) {
	$orderId = $order->getId();
} else {
	$orderId = null;
}
```

Der Eintrag kann weiter in den Aufruf geschrieben werden. Ich habe das Beispiel aus [Latte documentation] (https://latte.nette.org/cs/syntax#toc-volitelne-retezeni-s-nullsafe-operatorem) entnommen, das es perfekt beschreibt:

```php
$orderName = $order->item?->name;

// dasselbe wie:

$orderName = isset($order->item) ? $order->item->name : null;
```

Typische Anwendung ist die Auflistung komplexerer Strukturen in einer Vorlage, die in Latte zum Beispiel so aussieht (Beispiel aus der Dokumentation):

```php
{$user?->address?->street}
// bedeutet ca. ($user !== null) && ($user->address !== null) ? $user->address->street : null

{$items[2]?->count}
// ersetzen ca ($items[2] !== null) ? $items[2]->Zahl : null

{$user->getIdentity()?->name}
// ersetzen ca. $user->getIdentity() !== null ? $user->getIdentity()->name : null
```

In echtem Code kann es zum Beispiel so aussehen, dass wir das Land des Kunden herausfinden wollen, indem wir sein Profil lesen (und Sie haben die Daten in der Datenbank schön über Sessions gespeichert, so wie es gemacht werden sollte), dann sah es im alten PHP so aus:

```php
$country =  null;
if ($session !== null) {
    $user = $session->user;
    if ($user !== null) {
        $address = $user->getAddress();
        if ($address !== null) {
            $country = $address->country;
        }
    }
}
```

Jetzt kann sie auf eine einzige Zeile verkürzt werden:

```php
$country = $session?->user?->getAddress()?->country;
```

Die Verwendung des nullsafe-Operators verhindert auch verschiedene Fehler, die von einem unerfahrenen Entwickler in PHP 7 nicht leicht entdeckt werden können.

Dieser Eintrag führt zum Beispiel zu einem fatalen Fehler:

```php
var_dump($invoice->getDate()->format('Y-m-d') ?? null);

// return: fatal error: uncaught Error: call to a member function format() on null
```

Die korrekte Syntax lautet wie folgt:

```php
var_dump($invoice->getDate()?->format('Y-m-d'));

// liefert: null
```

Benannte Argumente
---------------------

Im guten alten PHP mussten Funktionsaufrufe mit Argumenten geschrieben werden, indem die Argumente in der exakten, von der Zielfunktion definierten Reihenfolge übergeben wurden. Dagegen ist nichts einzuwenden, aber wenn mehrere Parameter mit ähnlichen Werten verwendet werden, kann dies die Lesbarkeit beeinträchtigen. Oder wenn wir bis zum n-ten Parameter in der Reihenfolge übergeben wollten, mussten alle optionalen Parameter vorher übergeben werden, was sich negativ auf die Lesbarkeit und Vorwärtskompatibilität auswirken konnte.

Stellen Sie sich zum Beispiel die Funktion `setCookie()` in Nette vor, die eine Menge Argumente hat:

```php
public function setCookie(
	string $name,
	string $value,
	$time,
	string $path = null,
	string $domain = null,
	bool $secure = null,
	bool $httpOnly = null,
	string $sameSite = null
)
```

Die ersten drei Argumente (`$name`, `$value` und `$time`) sind obligatorisch, aber wenn wir das Argument `$httpOnly` übergeben wollten, mussten wir alle vorherigen Argumente übergeben und die Reihenfolge korrekt berechnen:

```php
$http->setCookie(
	'myCookie',
	'David mag Pferde',
	'jetzt',
	null, // Pfad
	null, // Bereich
	null, // sicher
	true
);
```

Und das wollen Sie einfach nicht, wenn Sie nicht müssen.

Elegantes Schreiben sieht dann so aus:

```php
$http->setCookie(
    name: 'myCookie',
    value: 'David mag Pferde',
    time: 'jetzt',
    httpOnly: true
);
```

Diese Art der Syntax erfordert, dass sich die Namen der Argumente in der Zielfunktion niemals ändern, da sie beim Aufruf weiterhin geschrieben werden. Zumindest werden die Entwickler sie besser benennen können.

Wenn nur eines der Argumente verwendet werden soll, kann die Syntax kombiniert und auf eine einzige Zeile verkürzt werden:

```php
$http->setCookie('myCookie', 'David mag Pferde', 'jetzt', httpOnly: true);
```

Die ersten 3 Argumente werden auf die ursprüngliche Weise übergeben, dann wird das optionale Argument `httpOnly` übergeben (weil es benannt ist).

Attribute
---------

Die meisten großen Sprachen wie Java oder C# enthalten bereits von Haus aus so genannte Annotationen, d. h. eine sprachinterne Syntax, mit der Sie anderen Sprachkonstrukten Metainformationen hinzufügen können.

In PHP hat diese Art von Syntax lange gefehlt und wurde durch die Verwendung von DOC-Kommentaren umgangen, was ein klassischer Kommentar über einer Methode ist, außer dass er zwei `/**`-Sternchen hat.

Diese Kommentare werden bei der Skriptverarbeitung ignoriert, und es muss eine spezielle Benutzerlogik hinzugefügt werden, um sie zur Laufzeit über Reflexion zu analysieren und zu interpretieren. Sie können sich wahrscheinlich vorstellen, welche Auswirkungen dies auf die Leistung haben kann. Außerdem kann die Syntax der Kommentare nicht verlangt werden und ist zur Compiletime (wenn das Skript verarbeitet wird, bevor es ausgeführt wird) sehr schwer zu überprüfen, und auch hier müssen Sie zusätzliche Tools außerhalb des normalen PHP-Toolkits verwenden, um dies zu tun.

Um die Abwärtskompatibilität zu wahren, bietet PHP Attribute mit einer Syntax, die der alternativen Kommentarschreibweise ähnelt, was die Ausführung des Skripts auf älteren PHP-Systemen nicht beeinträchtigt.

Die ursprüngliche Notation (z. B. für Inject-Abhängigkeiten in Nette Presenter verwendet):

```php
final class HomepagePresenter extends BasePresenter
{
	/** @inject */
	public EntityManager $entityManager;
}
```

Jetzt können Sie den Kommentar entfernen und das native Attribut verwenden:

```php
use App\Attributes\Inject;

final class HomepagePresenter extends BasePresenter
{
	#[Inject]
	public EntityManager $entityManager;
}
```

Es ist sehr wichtig, dass das Attribut nicht mehr nur ein Stück Zeichenkette in einem Kommentar ist, sondern eine physische Klasse, die gültigen PHP-Code darstellt.

Das ist großartig, denn Sie können nun die Eingaben in ein Attribut sicher validieren, und die Verwendung eines Attributs wird tatsächlich zu einem Aufruf seines Konstruktors, in dem andere Logik verwendet werden kann. Ich freue mich darauf, dass dies von Doctrine, das für alles Anmerkungen verwendet, nativ unterstützt wird.

Die Implementierung des Attributs selbst könnte dann etwa so aussehen:

```php
#[Attribute]
class Inject
{
    public string $value;

    public function __construct(string $value)
    {
        $this->value = $value;
    }
}
```

Strenge Logik kann wiederum innerhalb von Attributen verwendet werden, z. B. zur Überprüfung von Argumentdatentypen, Vereinigungstypen und anderen Sprachmerkmalen.

Ausdruck abgleichen
----------------

Das neue Sprachkonstrukt `match()` ist eine modernisierte Verbesserung des guten alten `switch()` (das ich versuche, nicht zu benutzen), und bringt eine Reihe von coolen Funktionen (die mich dazu bringen werden, es wieder zu benutzen).

Wir wollen zum Beispiel den Wert einer Variablen auf der Grundlage der Eingabe ändern:

```php
$pozdrav = match(bool $formal) {
    true => 'Hallo',
    false => 'Hallo',
};
```

Ein wichtiges neues Merkmal der Syntax ist, dass wir `break` nicht mehr verwenden müssen (wie das alte `switch`) und die Syntax im Allgemeinen viel sparsamer ist.

Gleichzeitig können wir innerhalb einer Bedingung (durch ein Komma getrennt) mehrere Eingaben auf einmal überprüfen und möglicherweise einen Standardwert zurückgeben (wenn keine der Bedingungen erfüllt ist).

Dies ist z. B. beim Umschreiben von HTTP-Bedingungscode in eine Fehlermeldung sehr nützlich (Sie werden es bei der Behandlung von Ausnahmecodes sehr zu schätzen wissen):

```php
$message = match ($statusCode) {
    200, 300 => null,
    400 => 'nicht gefunden',
    500 => 'Serverfehler',
    default => 'unbekannter Statuscode',
};
```

Der Vergleich von Werten wird strikt mit dem Operator `===` durchgeführt (switch verwendet nur `==`), was wiederum zeigt, dass PHP dem strikten Designpfad folgt. Daher wird die Eingabe `'200'` (eine Zeichenkette, die eine Zahl enthält) im vorherigen Fall nicht akzeptiert.

Wenn Sie keinen Wert für `default` angeben und es keine Übereinstimmung gibt, wird ein `UnhandledMatchError` ausgelöst.

Die neue Syntax erlaubt es auch, einen Ausdruck oder einen Funktionsaufruf für den Abgleich zu verwenden (er verhält sich wie eine Bedingung). Im Falle eines Fehlers können wir dann eine Ausnahme auslösen (da das `throw`-Token jetzt ein Ausdruck ist und auf diese Weise verwendet werden kann):

```php
$message = match ($statusCode) {
    200 => null,
    $this->checkServerError($statusCode) => throw new ServerError(),
    default => 'unbekannter Statuscode',
};
```

Weitergabe von Eigenschaften an den Konstruktor
-------------------------------------------------

Dies ist nur ein syntaktischer Zucker, der für die schnelle und einfache Definition einer Entität und ihrer Eigenschaften direkt im Konstruktor nützlich sein wird.


Zum Beispiel die ursprüngliche Entität:

```php
final class User
{
    public string $name;

    public function __construct(
        string $name,
    ) {
        $this->name = $name;
    }
}
```

Sie kann nur abgekürzt werden zu:

```php
final class User
{
    public function __construct(
        public string $name
    ) {}
}
```

Die Eigenschaft "$name" wird anhand des Datentyps "String" überprüft und ihr Wert kann direkt aus der Instanz gelesen werden, da es sich um eine öffentliche Eigenschaft handelt. Wenn Sie ein zusätzliches SmartObject in Nette verwenden (was ich für PHP 8 eher nicht empfehle), können Sie auch auf private Eigenschaften zugreifen, indem Sie zuerst deren Getter-Methode aufrufen, und diese Syntax vereinfacht die Dinge nochmals.

Rückgabetyp statisch
--------

In der Vergangenheit konnten wir den Datentyp "self" als Rückgabewert einer Methode verwenden, aber er gibt eine Instanz der Klasse zurück, in der er definiert ist. Der Datentyp `static` kann dies im Allgemeinen auch im Fall von Vererbung tun und gibt den Datentyp der Klasse zurück, von der die Instanz ausgeführt wurde, nicht den ihres Vorgängers.

Zum Beispiel:

```php
class BaseEndpoint
{
    public function getInstance(): static
    {
        return new static();
    }
}
```

Datentyp gemischt
------------------

Der Typ "gemischt" kann jetzt als Funktions- oder Methodenargument verwendet werden. Dies bedeutet, dass die Methode immer eine Eingabe akzeptieren muss (und daher ein obligatorisches Argument ist).

Wenn Sie es wenigstens ein bisschen können, verwenden Sie immer einen direkten Datentyp oder zumindest eine Vereinigung. Mixed ist nur sinnvoll, wenn die Funktion wirklich alles akzeptiert. In der Praxis ist die Verwendung z. B. für verschiedene Dump-Funktionen nützlich, die beliebige Eingaben akzeptieren und diese anzeigen können müssen.

Der Typ `mixed` akzeptiert die folgenden Typen: `string`, `int`, `float`, `null`, `bool`, `array`, `callable`, `object`, `resource`.

David wird dann den gemischten Typ für seine Funktion verwenden:

```php
function bdump(mixed $var): mixed
{
	Tracy\Debugger::barDump($var);
	return $var;
}
```

Tokenwurf als Ausdruck
------------------------

Das `throw`-Token ist jetzt ein Ausdruck, was in der Praxis bedeutet, dass eine Ausnahme ausgelöst werden kann, wenn die `fn()`-Lambda-Funktion abgeschnitten wird oder wenn ein ternärer Operator überprüft wird:

```php
$error = fn () => throw new \InvalidArgumentException('Dies führt immer zu einem Fehler.');

$userName = $user['Name'] ?? throw new \LogicException('Der Benutzer muss einen Namen haben.');
```

Die Funktion str_contains()
-----------------------

PHP enthält endlich eine native Funktion, um zu prüfen, ob der Standardstring eine Teilzeichenkette enthält.

Zum Beispiel:

```php
if (str_contains('Honzik mag Katzen.', 'Katzen')) {
	echo 'Das Feature behandelt Katzen.';
}
```

In der Vergangenheit wurde das Auftreten einer Teilzeichenkette mit der Funktion [strpos](/strpos-function) überprüft:

```php
if (strpos('Honzik mag Katzen.', 'Katzen') !== false) {
	echo 'Das Feature behandelt Katzen.';
}
```

Funktionen str_starts_with() und str_ends_with()
----------------------------------------------

Ein Paar neuer Funktionen, um zu prüfen, ob eine Zeichenkette mit einer Teilzeichenkette beginnt oder endet:

```php
str_starts_with('Honzik mag Katzen.', 'Honzik'); // wahr

str_ends_with('Honzik mag Katzen.', 'Katzen.'); // wahr
```

Funktion get_debug_type()
-------------------------

Verbessert die Ausgabe der bestehenden Funktion [gettype](/function-gettype), die nur den generischen Typ der übergebenen Variablen zurückgibt. Die Funktion wird z. B. verwendet, wenn eine Ausnahme ausgelöst wird, wenn wir eine ungültige Eingabe erhalten und dem Benutzer mitteilen wollen, was er tatsächlich übergeben hat.

Wenn wir die Funktion `gettype()` mit einer Variablen aufrufen, die eine Instanz der Klasse `App\User` enthält, gibt die Funktion `object` zurück, so dass wir nicht wissen, um welche Klasse es sich handelt. Die neue Funktion `get_debug_type()` gibt den Namen der Klasse zurück.

Die Funktion get_resource_id()
--------------------------

Diese Funktion gibt die Kennung einer externen Ressource aus einer Variablen zurück.

Zum Beispiel wird die Verbindung zu einer MySql-Datenbank von PHP mit Hilfe eines speziellen `Resource`-Datentyps gehandhabt; jetzt ist es möglich, herauszufinden, welche ID ihr zugewiesen wurde.

> **Historische Anmerkung:**
>
> Der Typ "Ressource" in PHP wurde zu einer Zeit entwickelt, als es noch nicht wusste, wie man Objekte verwendet, und irgendwie herausfinden musste, wie man Referenzen auf etwas wie einen "Datentyp" übergibt. Für die Zukunft ist zu erwarten, dass `resource` ganz aus der Sprache entfernt wird, daher ist es am besten, diese Funktion nicht zu verwenden.

Die Erweiterung ext-json ist immer verfügbar
-------------------------------------

In der Vergangenheit konnte PHP ohne Unterstützung für json kompiliert werden. Jetzt wird json immer verfügbar sein, so dass Sie die `ext-json`-Abhängigkeit aus Ihren `composer.json`-Dateien entfernen können und immer wissen, dass json verwendet werden kann.

Vorrang der Verkettung
-------------------------------------------------------

Stellen Sie sich etwas vor wie:

```php
echo 'Die Gesamtsumme:' . $a + $b;
```

Wird die Zahlenaddition zuerst durchgeführt, oder wird die Variable "$a" zuerst an die Zeichenkette angehängt und dann die gesamte neue Zeichenkette zu "$b" hinzugefügt?

Man würde erwarten, dass die Addition zuerst erfolgt, aber das ist eine schöne Annahme. PHP funktioniert in etwa so:

```php
echo ('Die Gesamtsumme:' . $a) + $b;
```

PHP 8 wird sich nun vorhersehbar verhalten:

```php
echo 'Die Gesamtsumme:' . ($a + $b);
```

Im Allgemeinen ist es jedoch immer besser, einen Ausdruck durch Klammern abzugrenzen.

Stabile Ordnung
---------------

Vor PHP 8 wurde die Stringsortierung mit dem so genannten instabilen Algorithmus durchgeführt, was bedeutet, dass PHP nicht garantiert, dass Elemente mit demselben (oder einem anderen gleichwertigen) Wert nicht vertauscht werden. Die neue Version ändert das Verhalten aller Sortierfunktionen auf stabil, so dass die Sortierung immer deterministisch durchgeführt wird und Sie immer die gleiche Ausgabe erhalten.

Dies löst zum Beispiel Fälle, in denen wir Nutzerbewertungen nach Relevanz einstuften, aber einige Bewertungen die gleiche Punktzahl hatten. Jetzt erscheinen sie bei jeder Sortierung in der gleichen Reihenfolge und werden nicht ständig übersprungen.

Andere neue Funktionen
---------------

PHP hat viele weitere kleinere neue Funktionen und Verbesserungen. Beispielsweise werden Fehler anders ausgelöst (aber das stört uns, die wir fehlerfreien Code schreiben, nicht, oder?).

Die vollständige Liste der Änderungen können Sie jederzeit in der offiziellen Dokumentation und im RFC-Post einsehen.

Was ich im neuen PHP vermisse
-----------------------

Ich fände es gut, wenn PHP endlich zusammengesetzte Array-Typen unterstützen würde. Wenn zum Beispiel eine Methode ein Array von Bezeichnern zurückgibt, müssen wir immer noch nur `getIds(): array` angeben und etwas wie `getIds(): int[]` wäre viel besser. Vielleicht sehen wir das bald, und die strenge Typenprüfung ist dann abgeschlossen.

Weitere Ressourcen
------------

David Grudl hielt einen netten Vortrag über die neuen Sachen bei Posobot. Ich empfehle Ihnen, sich die Aufzeichnung anzusehen:

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/ZYkwmcCUTCg" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Hiermit möchte ich mich bei David für seinen Vortrag bedanken, denn ich habe einige Informationen daraus für diesen Artikel übernommen. Insbesondere geht es um Nette's Umstellung auf PHP 8 und andere Tipps hinter den Kulissen von PHP.
