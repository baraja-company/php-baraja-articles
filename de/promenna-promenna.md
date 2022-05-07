Variablen Variablen
===================

> id: a0baeb3a-ab6e-4dd9-b1bc-b1872a12ee08
> slug:
> 	cs: promenna-promenna
> 	de: variablen-variablen
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '590515cc711ec27cc0f0bfe190c8fbff'

**Warnung:** Dieser Artikel wurde vor vielen Jahren geschrieben und einige Informationen sind möglicherweise veraltet oder falsch. Bitte beachten Sie dies beim Lesen.

**Variablen** sind nicht für den allgemeinen Einsatz gedacht (sie lösen Probleme, die auch auf andere Weise gelöst werden können), sie werden hauptsächlich verwendet, um Schreibvorgänge übersichtlicher und Speicherzugriffe komplexer zu gestalten.

Betrachten Sie das folgende Beispiel:

```php
$x = 25;                  // enthält 25
$nacitana_promenna = 'x'; // enthält "x"
$y = $$nacitana_promenna; // enthält 25
echo $y;                  // druckt 25
```

Beachten Sie die beiden aufeinanderfolgenden Dollar. In diesem Fall wird der Wert der Variablen $y in die Variable geladen, die den in $nacitana_variable enthaltenen Namen trägt.

Ein bisschen verwirrend, oder? Deshalb sollten Sie besser keine Variablen verwenden.
> **Hinweis:** Variable Variablen sind wegen des Dollarzeichens eine Besonderheit von PHP. In anderen Sprachen wird der Anfang des Variablennamens nicht mit einem Zeichen gekennzeichnet, so dass Sie keine variablen Variablen verwenden können, da es mehrdeutig wäre, wann es sich um eine klassische Variable handelt und wann nicht.
