Podmínky a větvení
==================

> id: "2cea5541-6879-4763-a518-cb21bf9021dd"
> slugCS: podminky
> publicationDate: "2019-09-07 20:25:57"
> mainCategoryId: "6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070"

> **Upozornění:** Tento článek byl napsán před mnoha lety a některé informace mohou být zastaralé, nebo nepravdivé. Při čtení na to, prosím, berte ohled.

Konec lineárním programům! Nejzákladnější princip každého programu je „co se stane když....“. Podmínka se dá zapsat jako logický výrok, který může platit (podmínka se splní) nebo neplatí (pak se neprovede nebo se provede její přesný opak). Oboje se dá snadno definovat.

Obecný zápis
------------

Obecně se dá podmínka zapsat jako logický výrok. Podmínka může být splněna, nebo nemusí. Je dobré počítat obě varianty jako možné. Pokud je více alternativních řešení, tak se tomu říká **vnořená podmínka**.

Příklad:

```php
if (hodnota   operace   hodnota) {
	// Toto se spustí, pokud podmínka platí
} else {
	// Toto se spustí, pokud podmínka neplatí
}
```

Nemusíme vždy definovat obě varianty (někdy to je naprosto zbytečné). Můžeme totiž definovat situaci v případě, že jen podmínka platí. To se dělá takto:

```php
if (hodnota   operace   hodnota) {
	// Toto se spustí, pokud podmínka platí
}
```

Logické operátory
--------------------------

| Operátor | Význam
|----------|---------
| `==`     | Rovná se
| `===`    | Rovná se a má stejný datový typ (*lze porovnávat cokoli s čímkoli, ale podmínka je splněna jen když se jedná o hodnotu stejného datového typu (na př. číslo, text, …)*)
| `!=`     | Nerovná se
| `<=`     | Rovná se nebo je větší
| `>=`     | Rovná se nebo je menší
| `<`      | Větší
| `>`      | Menší

Reálná ukázka
--------------------------

```php
$a = 5;
$b = 3;
if ($a === $b) {
	// blok, který se vypíše, pokud se $a rovná $b
} else {
	// blok, který se vypíše, pokud se $a NErovná $b
}
```

Vnořené podmínky
--------------------------

Bohužel je výstupem pouze hodnota `true` (platí) a `false` (neplatí). Pokud tedy chceme brát v potaz více možností, tak musíme vložit více podmínek do sebe. Tomu se říká **vnořená podmínka**. Vnořená je z toho důvodu, že jedním z řešení podmínky je právě další podmínka.

```php
$a = 5;         // levá kapsa
$b = 3;         // pravá kapsa
$kapsa = true;  // mám kapsu?

if ($kapsa === true) {

	if ($a > $b) {
		echo 'V levé kapse je více';
	} else {
		echo 'V pravé kapse je více';
	}

} else {
	echo 'Nemáš kapsu';
}
```
