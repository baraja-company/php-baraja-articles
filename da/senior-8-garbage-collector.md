Uhensigtsmæssig brug af Garbage Collector
=========================================

> id: '842e30ab-1af1-4152-ba0f-3004ca3b620f'
> slug:
> 	cs: senior-8-garbage-collector
> 	da: uhensigtsmaessig-brug-af-garbage-collector
> 
> publicationDate: '2023-02-11 14:35:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: ae5993313436e0d7c5291b6337b988ef

Du er udvikler af en stor ældre applikation, som du gradvist indfører PHPStan i. Du starter med niveau 0, som er ret udfordrende, men til sidst får du det hele på plads. Du går videre til de næste niveauer, hvor en del af din kode begynder at rapportere en ubrugt $lock-variabel, som du bør fjerne.

Koden ser således ud:

```php
public function processOrder(int $orderId): void
{
	$lock = Lock::createLock('ordre-' . $orderId);

	// Der er en vis logik her...
}
```

Du siger til dig selv, at der må være en lås gemt i variablen, som nogen har glemt at frigive senere, eller måske sker det i andre metoder, som kaldes senere. Så du beslutter dig for at fjerne den ubrugte variabel og kun beholde opkaldet til den statiske metode, der opretter låsen.

Kan denne beslutning forårsage en kritisk fejl?

Hvis ja, hvorfor, og hvordan kunne den oprindelige mekanisme have fungeret?

Hvis ikke, hvorfor ikke, og hvordan kan du vide, at det altid er en sikker operation?
