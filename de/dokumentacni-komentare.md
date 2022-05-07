Dokumentarische Kommentare, Tschechisch oder Englisch?
======================================================

> id: e5f1bf13-ab9e-4a70-a95c-77fb50aa4878
> slug:
> 	cs: dokumentacni-komentare
> 	de: dokumentarische-kommentare-tschechisch-oder-englisch
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1'
> sourceContentHash: a33af5a14338e9895fe87087362bd476

Das Schreiben von Dokumentationen kostet Zeit, und oft liest sie niemand nach Ihnen, daher ist es eine gute Praxis, Kommentare direkt in den Quellcode zu schreiben. Viel Text verunreinigt jedoch unnötig den Code, der dann möglicherweise nicht mehr gleichzeitig auf den Bildschirm passt, was wiederum die Lesbarkeit beeinträchtigt.

Daher sollte jeder Code **selbsterklärend** sein, d.h. wenn man ihn liest, sollte sofort klar sein, was er tut, und er sollte nur eine Sache tun.

Wenn eine Funktion zum Beispiel `getUserProfileById($id)` heißt, ist klar, was eine solche Funktion tut und welchen Rückgabewert wir erwarten können. Deshalb werden in Kommentaren oft nur die Ein- und Ausgabeparameter und eine kurze textliche Erläuterung des Prinzips beschrieben (wenn es sich um einen komplexeren Vorgang handelt). Wenn der Code für mehrere Personen bestimmt ist, ist es höflich, den Autor und das Datum der Erstellung anzugeben.

```php
/**
 * Gibt TRUE zurück, wenn der übergebene Wert eine gültige IPv6-Adresse ist
 * Alternativer regulärer Ausdruck:
 * /^(((?=(?>.*?(::))(?!.+\3)))\3?|([\dA-F]{1,4}(\3|:(?!$)|$)|\2))
   (?4){5}((?4){2}|((2[0-4]|1\d|[1-9])?\d|25[0-5])(\.(?7)){3})\z/i
 * @Autor Jan Barasek
 */
function isIpV6(string $value): bool
{
   if (str_contains($value, ':')) {
      return (bool) filter_var($value, FILTER_VALIDATE_IP, FILTER_FLAG_IPV6);
   }

   return false;
}
```

Aus dem Kommentar geht unmittelbar hervor, dass die Funktion eine IPv6-Adresse als Eingabe erwartet und einen booleschen Wert "TRUE" oder "FALSE" zurückgibt, je nachdem, wie die Validierung verläuft.

Die Bootstrap-Annotation dient dazu, Werte mit standardisierten Schlüsseln anzugeben, die von vielen Editoren weiterverarbeitet werden können und z.B. Parameter beim Funktionsaufruf andeuten oder Abhängigkeiten automatisch übergeben.

Kommentare direkt innerhalb des Codes werden nur in besonderen Fällen verwendet. Wenn der Code einen internen Kommentar benötigt, ist dies in der Regel ein Hinweis darauf, dass er in mehrere kleinere Teile aufgeteilt werden sollte, die jeweils einen eigenen Teil des Programms betreffen.

Sprachen
--------------

Es ist üblich, Klassen, Methoden, Funktionen und Variablen immer in englischer Sprache zu benennen (um den Code für eine große Anzahl von Programmierern lesbar zu machen), Schlüssel sind relativ standardisiert und haben eine einheitliche Syntax, so dass die Sprache auch hier keine Rolle spielt, und bei Hilfstexten hängt es von der Verwendung im Projekt ab. Wenn es sich um ein kleines Projekt für ein festes Team von Mitarbeitern handelt, ist Englisch nicht unbedingt ein Problem, im Gegenteil, zumindest versteht jeder die Beschreibung perfekt.

Motivation für die Verwendung der Anmerkung
-------------------

Sonderzeichen (Call Signs) können von anderen Werkzeugen außerhalb des Editors wahrgenommen werden, so dass es z.B. sinnvoll ist, seine Tests (auf welche Eingaben erwarte ich welche Ausgabe) direkt über die Funktionsdefinition zu setzen, so dass man nie vergisst, wo die Tests geschrieben werden und gleichzeitig jeder Programmierer sofort weiß, was die Funktion tut.

Hier ist ein kleines Beispiel dafür, wie eine Testspezifikation aussehen könnte (es handelt sich nur um ein Notationskonzept, nicht um ein spezifisches Testwerkzeug):

```php
Dosadí hodnoty a vynutí jejich dump do prohlížeče / konzole:
@test Název I---> $limit => {1-{5-8}}, $city => {Praha|Kladno|Brno|Plzeň} O---> [DUMP]

Vygeneruje náhodný hash o délce 6 znaků a ověří, zda je hlavička odpovědi jakákoli, kromě 201:
@test Název I---> $hash => {a-z0-9|len:6} O---> [HEADER:***|!HEADER:201]

Vygeneruje dvojici čísel a na výstupu ověří jejich součet:
@test Sčítání čísel I---> $a => {1-5}, $b => {7-12} O---> ($return == $a+$b)

Načte náhodný článek s ID mezi 1 až 30, ověří jeho délku nebo neprázdnost:
@test Získání textu článku I---> $id => {1-30} O---> (strlen($return) > 64 || $return != NULL)

Vygeneruje náhodnou IP adresu a ověří, zda je z České republiky:
@test Geolokace I---> $ip => {1-255}.{1-255}.{1-255}.{1-255} O---> ($return['Land'] == 'DE')

Pokusí se načíst článek s vysokým ID a při testování mrkne na modifikátory (filtry):
@test Neexistující článek I---> $id => {1000000+} O---> [HEADER:4**:3**|NOCONTENT]

Vrátí počet rohů jednorožce:
@test Jednorožec I---> $maRohy => TRUE O---> 1

Kolik prstů ukazuji?
@test Prsty I---> $ukazuji => {0-10|NAME:prsty} O---> ($return == {*|NAME:prsty})
```

Die in der Regel z.B. nach der Regel geschrieben würden:

```php
@test <název_testu> I---> $<proměnná> => <hodnota> O---> [<modifikátor>:<hodnota>] (<výraz_platnosti>)
```

Innerhalb der Tests habe ich einen Zufallswertgenerator mit regulären Ausdrücken verwendet.
Hier sind einige Beispiele:

```php
{<hodnota><směr_nerovnosti>}           {500+}  {250-}   {-200-(-50)}
{<začátek>-<konec>}                    {0-5}   {a-z}   {a-f0-9}
{<Hodnota1>|<Hodnota2>|<Hodnota3>}     {Praha|Kladno|Brno}
{<výraz>|<modifikátor>:<hodnota>}      {a-z0-9|len:6}  {*|len:6}
{*|<modifikátor>:<hodnota>}            {*|randomWord:5}
```
