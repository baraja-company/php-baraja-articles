Uso inappropriato del Garbage Collector
=======================================

> id: '842e30ab-1af1-4152-ba0f-3004ca3b620f'
> slug:
> 	cs: senior-8-garbage-collector
> 	it: uso-inappropriato-del-garbage-collector
> 
> publicationDate: '2023-02-11 14:35:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: ae5993313436e0d7c5291b6337b988ef

Siete lo sviluppatore di una grande applicazione legacy, nella quale state gradualmente introducendo PHPStan. Si inizia con il livello 0, che è piuttosto impegnativo, ma alla fine si riesce a risolvere il problema. Si passa ai livelli successivi, dove una parte del codice inizia a segnalare una variabile $lock inutilizzata che dovrebbe essere rimossa.

Il codice si presenta come segue:

```php
public function processOrder(int $orderId): void
{
	$lock = Lock::createLock('ordine-' . $orderId);

	// C'è una certa logica qui...
}
```

Ci si dice che ci deve essere un blocco memorizzato nella variabile che qualcuno ha dimenticato di rilasciare in seguito, o forse sta accadendo all'interno di altri metodi che vengono chiamati in seguito. Si decide quindi di rimuovere la variabile inutilizzata, mantenendo solo la chiamata al metodo statico che crea il blocco.

Questa decisione può causare un errore critico?

Se sì, perché e come avrebbe potuto funzionare il meccanismo originale?

Se non è così, perché non lo è e come si fa a sapere che si tratta sempre di un'operazione sicura?
