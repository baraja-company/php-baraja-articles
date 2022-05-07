Podmienky a vetvenie
====================

> id: '2cea5541-6879-4763-a518-cb21bf9021dd'
> slug:
> 	cs: podminky
> 	sk: podmienky-a-vetvenie
> 
> publicationDate: '2019-09-07 20:25:57'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: bc4140fc49413be3f2f2b2264b8ea5e6

> **Upozornenie:** Tento článok bol napísaný pred mnohými rokmi a niektoré informácie môžu byť zastarané alebo nesprávne. Majte to na pamäti pri čítaní.

Už žiadne lineárne programy! Najzákladnejším princípom každého programu je "čo sa stane, keď....". Podmienku možno zapísať ako logický príkaz, ktorý môže byť pravdivý (podmienka je splnená) alebo nepravdivý (potom sa nevykoná alebo sa vykoná jej presný opak). Obidve sa dajú ľahko definovať.

Všeobecný zápis
------------

Vo všeobecnosti možno podmienku zapísať ako logický príkaz. Táto podmienka môže, ale nemusí byť splnená. Je dobré počítať s oboma možnosťami. Ak existuje viacero alternatív, nazýva sa to **vložená podmienka**.

Príklad:

```php
if (hodnota   operace   hodnota) {
	// Táto funkcia sa spustí, ak je splnená podmienka
} else {
	// Táto funkcia sa spustí, ak sa podmienka neuplatní
}
```

Nemusíme vždy definovať obe možnosti (niekedy je to úplne zbytočné). V skutočnosti môžeme definovať situáciu, ak platí len táto podmienka. To sa vykonáva takto:

```php
if (hodnota   operace   hodnota) {
	// Táto funkcia sa spustí, ak je splnená podmienka
}
```

Logické operátory
--------------------------

| Operátor | Význam
|----------|---------
| `==` | Rovná sa
| `===` | Rovná sa a má rovnaký dátový typ (*čokoľvek možno porovnať s čímkoľvek, ale podmienka je splnená len vtedy, ak ide o hodnotu rovnakého dátového typu (napr. číslo, text, ...)*)
| `!=` | Nerovná sa
| `<=` | Rovná sa alebo väčšia ako
| `>=` | Rovná sa alebo menšia ako
| `<` | Väčší
| `>` | Menej

Skutočný príklad
--------------------------

```php
$a = 5;
$b = 3;
if ($a === $b) {
	// blok, ktorý sa vypíše, ak sa $a rovná $b
} else {
	// blok, ktorý sa vypíše, ak sa $a nerovná $b
}
```

Vložené podmienky
--------------------------

Bohužiaľ, výstupom je len `true` (platný) a `false` (neplatný). Ak teda chceme zvážiť viacero možností, musíme do seba vložiť viacero podmienok. Toto sa nazýva **vložená podmienka**. Je vnorená, pretože jedno z riešení podmienky je len ďalšou podmienkou.

```php
$a = 5;         // ľavé vrecko
$b = 3;         // pravé vrecko
$kapsa = true;  // Mám vrecko?

if ($kapsa === true) {

	if ($a > $b) {
		echo "V ľavom vrecku je toho viac;
	} else {
		echo "V pravom vrecku je toho viac;
	}

} else {
	echo "Nemáte žiadne vrecko;
}
```
