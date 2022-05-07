PHP-Schlüsselwörter
===================

> id: d58620d7-57a0-410a-ba73-31a1f9d984fb
> slug:
> 	cs: keywordy-jazyka-php
> 	de: php-schluesselwoerter
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '38fbdb53b4455903f3300c3c8b4a8ad2'

Fast alle Programmiersprachen bestehen aus "Schlüsselwörtern", die spezielle Ausdrücke der Sprache sind, die eine besondere Bedeutung haben.

Ein Beispiel für ein Schlüsselwort ist das Wort `String`, `if` oder `Echo`.

Es ist wichtig zu beachten, dass Schlüsselwörter (manchmal auch `Befehle`) <a href="/befehle-und-funktionen">keine Funktionen</a> sind, so ist zum Beispiel `echo` auch keine Funktion.

Die Liste der Schlüsselwörter ist gut zu wissen, weil sie in PHP besondere Bedeutungen haben und nicht immer für alles verwendet werden können. Zum Beispiel darf der Name einer Klasse nicht mit einem der vorhandenen Schlüsselwörter übereinstimmen.

Beispiel für einen Parse-Fehler
-------------------

Beim Versuch, eine Klasse mit dem Namen `String` zu verarbeiten, tritt ein `Parse error` auf, weil PHP nicht weiß, ob es sich um einen Klassennamen oder einen Datentyp handelt:

Das ist falsch:

```php
class String
{
   // ...
}
```

Liste der Schlüsselwörter
-------------------

Aktualisierte Schlüsselwortliste für PHP 7.1:

`und`, `oder`, `xor`, `for`, `do`, `while`, `foreach`, `as`, `return`, `die`, `exit`, `if`, `then`, `else`, `elseif`, `new`, `delete`, `try`, `throw`, `catch`, `finally`, `class`, `function`, `string`, `array`, `object`, `resource`, `var`, `bool`, `boolean`, `int`, Integer", "Float", "Double", "Real", "String", "Array", "Global", "Const", "Static", "Public", "Private", "Protected", "Published", `erweitert`, `umschalten`, `true`, `false`, `null`, `void`, `this`, `self`, `struct`, `char`, `signed`, `unsigned`, `short`, `long`
