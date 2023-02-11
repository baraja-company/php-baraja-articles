Nevhodné používanie zberača odpadu
==================================

> id: '842e30ab-1af1-4152-ba0f-3004ca3b620f'
> slug:
> 	cs: senior-8-garbage-collector
> 	sk: nevhodne-pouzivanie-zberaca-odpadu
> 
> publicationDate: '2023-02-11 14:35:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: ae5993313436e0d7c5291b6337b988ef

Ste vývojár veľkej staršej aplikácie, do ktorej postupne zavádzate PHPStan. Začínate na úrovni 0, ktorá je pomerne náročná, ale nakoniec ju zvládnete. Prejdete na ďalšie úrovne, kde jedna časť vášho kódu začne hlásiť nepoužívanú premennú $lock, ktorú by ste mali odstrániť.

Kód vyzerá takto:

```php
public function processOrder(int $orderId): void
{
	$lock = Lock::createLock('objednávka -' . $orderId);

	// Je tu istá logika...
}
```

Poviete si, že v premennej musí byť uložený zámok, ktorý niekto zabudol neskôr uvoľniť, alebo sa to možno deje v iných metódach, ktoré sú volané neskôr. Preto sa rozhodnete odstrániť nepoužívanú premennú a ponecháte si len volanie statickej metódy, ktorá vytvára zámok.

Mohlo by toto rozhodnutie spôsobiť kritickú chybu?

Ak áno, prečo a ako mohol pôvodný mechanizmus fungovať?

Ak nie, prečo nie a ako viete, že je to vždy bezpečná operácia?
