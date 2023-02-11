Olämplig användning av Garbage Collector
========================================

> id: '842e30ab-1af1-4152-ba0f-3004ca3b620f'
> slug:
> 	cs: senior-8-garbage-collector
> 	sv: olaemplig-anvaendning-av-garbage-collector
> 
> publicationDate: '2023-02-11 14:35:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: ae5993313436e0d7c5291b6337b988ef

Du är en utvecklare av en stor äldre applikation som du gradvis inför PHPStan i. Du börjar med nivå 0, som är ganska utmanande, men så småningom får du rätt. Du går vidare till nästa nivå, där en del av din kod börjar rapportera en oanvänd $lock-variabel som du bör ta bort.

Koden ser ut så här:

```php
public function processOrder(int $orderId): void
{
	$lock = Lock::createLock('beställning-' . $orderId);

	// Det finns en viss logik här...
}
```

Du säger till dig själv att det måste finnas en låsning i variabeln som någon har glömt att frigöra senare, eller att det kanske händer i andra metoder som anropas senare. Du bestämmer dig därför för att ta bort den oanvända variabeln och behålla endast anropet till den statiska metoden som skapar låset.

Kan detta beslut orsaka ett kritiskt fel?

Om så är fallet, varför, och hur skulle den ursprungliga mekanismen ha fungerat?

Om inte, varför inte, och hur vet du att detta alltid är en säker operation?
