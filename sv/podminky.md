Villkor och förgrening
======================

> id: '2cea5541-6879-4763-a518-cb21bf9021dd'
> slug:
> 	cs: podminky
> 	sv: villkor-och-foergrening
> 
> publicationDate: '2019-09-07 20:25:57'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: bc4140fc49413be3f2f2b2264b8ea5e6

> **Varning:** Den här artikeln skrevs för många år sedan och viss information kan vara föråldrad eller felaktig. Tänk på detta när du läser.

Inga fler linjära program! Den mest grundläggande principen för alla program är "vad som händer när....". Ett villkor kan skrivas som ett logiskt uttalande som kan vara giltigt (villkoret uppfylls) eller ogiltigt (villkoret utförs inte eller dess motsats utförs). Båda är lätta att definiera.

Allmän notation
------------

I allmänhet kan ett villkor skrivas som ett logiskt uttalande. Villkoret kan vara uppfyllt eller inte. Det är bra att räkna med båda alternativen. Om det finns flera alternativ kallas det för ett **försett villkor**.

Exempel:

```php
if (hodnota   operace   hodnota) {
	// Detta utlöses om villkoret är uppfyllt
} else {
	// Detta utlöses om villkoret inte gäller.
}
```

Vi behöver inte alltid definiera båda alternativen (ibland är det helt onödigt). I själva verket kan vi definiera situationen om bara villkoret gäller. Detta görs på följande sätt:

```php
if (hodnota   operace   hodnota) {
	// Detta utlöses om villkoret är uppfyllt
}
```

Logiska operatörer
--------------------------

| Operatör | Betydelse
|----------|---------
| `==` | Liknar
| `===` | Likvärdig och har samma datatyp (*Vad som helst kan jämföras med vad som helst, men villkoret uppfylls endast om det är ett värde av samma datatyp (t.ex. tal, text, ...)*)
| `!=` | Är inte lika med
| `<=` | Lika med eller större än
| `>=` | Lika med eller mindre än
| `<` | Större
| `>` | Mindre

Verkligt exempel
--------------------------

```php
$a = 5;
$b = 3;
if ($a === $b) {
	// block som skrivs ut om $a är lika med $b
} else {
	// block som skrivs ut om $a INTE är lika med $b
}
```

Inbäddade villkor
--------------------------

Tyvärr är resultatet endast `true` (giltigt) och `false` (ogiltigt). Så om vi vill överväga flera möjligheter måste vi bädda in flera villkor i varandra. Detta kallas ett **inbäddat villkor**. Det är inbäddat eftersom en av lösningarna på villkoret bara är ett annat villkor.

```php
$a = 5;         // vänster ficka
$b = 3;         // höger ficka
$kapsa = true;  // Har jag en ficka?

if ($kapsa === true) {

	if ($a > $b) {
		echo 'I den vänstra fickan finns mer';
	} else {
		echo 'I den högra fickan finns mer';
	}

} else {
	echo 'Du har ingen ficka';
}
```
