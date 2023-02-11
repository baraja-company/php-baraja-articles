Niewłaściwe użycie Garbage Collector
====================================

> id: '842e30ab-1af1-4152-ba0f-3004ca3b620f'
> slug:
> 	cs: senior-8-garbage-collector
> 	pl: niewlasciwe-uzycie-garbage-collector
> 
> publicationDate: '2023-02-11 14:35:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: ae5993313436e0d7c5291b6337b988ef

Jesteś deweloperem dużej aplikacji legacy, do której stopniowo wprowadzasz PHPStan. Zaczynasz od poziomu 0, który jest dość wymagający, ale w końcu udaje ci się go opanować. Przechodzisz do kolejnych poziomów, gdzie jedna część twojego kodu zaczyna zgłaszać nieużywaną zmienną $lock, którą powinieneś usunąć.

Kod wygląda tak:

```php
public function processOrder(int $orderId): void
{
	$lock = Lock::createLock('zamówienie -' . $orderId);

	// Jest tu jakaś logika...
}
```

Mówisz sobie, że musi istnieć blokada przechowywana w zmiennej, którą ktoś zapomniał zwolnić później, a może dzieje się to wewnątrz innych metod, które są wywoływane później. Więc decydujesz się usunąć nieużywaną zmienną, zachowując tylko wywołanie do statycznej metody, która tworzy zamek.

Czy ta decyzja może spowodować błąd krytyczny?

Jeśli tak, to dlaczego i jak mógł działać pierwotny mechanizm?

Jeśli nie, to dlaczego nie i skąd wiadomo, że jest to zawsze bezpieczne działanie?
