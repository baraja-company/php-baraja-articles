Nevhodné použití Garbage collectoru
===================================

> id: "842e30ab-1af1-4152-ba0f-3004ca3b620f"
> slug:
> 	cs: senior-8-garbage-collector
>
> publicationDate: "2023-02-11 14:35:00"
> mainCategoryId: 8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a

Jste vývojář velké legacy aplikace, do které postupně zavádíte PHPStan. Začnete levelem 0, který je poměrně náročný, ale nakonec to zvládnete. Přejdete na další levely, kde vám začne v jedné části kódu hlásit nepoužívanou proměnnou $lock, kterou byste měli odebrat.

Kód vypadá takto:

```php
public function processOrder(int $orderId): void
{
	$lock = Lock::createLock('order-' . $orderId);

	// Tady je nějaká logika...
}
```

Říkáte si, že je v proměnné určitě uložený zámek, který někdo zapomněl později uvolnit, nebo se to možná děje uvnitř dalších metod, které se provolají později. Rozhodnete se proto nepoužívanou proměnnou odstranit, a zachováte jen volání statické metody, která vytváří zámek.

Může toto rozhodnutí způsobit kritickou chybu?

Pokud ano, proč, a jak mohl původní mechanismus fungovat?

Pokud ne, proč, a jak víte, že to je vždy bezpečná operace?
