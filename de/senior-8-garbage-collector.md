Ungeeignete Verwendung des Garbage Collectors
=============================================

> id: '842e30ab-1af1-4152-ba0f-3004ca3b620f'
> slug:
> 	cs: senior-8-garbage-collector
> 	de: ungeeignete-verwendung-des-garbage-collectors
> 
> publicationDate: '2023-02-11 14:35:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: ae5993313436e0d7c5291b6337b988ef

Sie sind Entwickler einer großen Legacy-Anwendung, in die Sie schrittweise PHPStan einführen wollen. Man fängt mit Level 0 an, was eine ziemliche Herausforderung ist, aber irgendwann hat man es geschafft. Sie gehen weiter zu den nächsten Ebenen, wo ein Teil Ihres Codes beginnt, eine unbenutzte $lock-Variable zu melden, die Sie entfernen sollten.

Der Code sieht wie folgt aus:

```php
public function processOrder(int $orderId): void
{
	$lock = Lock::createLock('Bestellung -' . $orderId);

	// Es gibt hier eine gewisse Logik...
}
```

Sie sagen sich, dass in der Variablen eine Sperre gespeichert sein muss, die jemand vergessen hat, später freizugeben, oder dass es vielleicht in anderen Methoden passiert, die später aufgerufen werden. Sie beschließen also, die unbenutzte Variable zu entfernen und nur den Aufruf der statischen Methode, die die Sperre erzeugt, beizubehalten.

Könnte diese Entscheidung einen kritischen Fehler verursachen?

Wenn ja, warum, und wie könnte der ursprüngliche Mechanismus funktioniert haben?

Wenn nicht, warum nicht, und woher wissen Sie, dass dies immer ein sicherer Vorgang ist?
