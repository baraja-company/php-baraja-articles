Betingelser og forgrening
=========================

> id: '2cea5541-6879-4763-a518-cb21bf9021dd'
> slug:
> 	cs: podminky
> 	da: betingelser-og-forgrening
> 
> publicationDate: '2019-09-07 20:25:57'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: bc4140fc49413be3f2f2b2264b8ea5e6

> **Varsel:** Denne artikel blev skrevet for mange år siden, og nogle oplysninger kan være forældede eller forkerte. Vær opmærksom på dette, når du læser.

Ikke flere lineære programmer! Det mest grundlæggende princip for ethvert program er "hvad der sker når....". En betingelse kan skrives som et logisk udsagn, som kan være gyldigt (betingelsen er opfyldt) eller ugyldigt (så udføres den ikke eller dens nøjagtige modsætning udføres). Begge dele er lette at definere.

Generel notation
------------

Generelt kan en betingelse skrives som en logisk erklæring. Denne betingelse kan være opfyldt eller ej. Det er en god idé at medregne begge muligheder som muligt. Hvis der er flere alternativer, kaldes det for en **indlejret betingelse**.

Eksempel:

```php
if (hodnota   operace   hodnota) {
	// Dette udløses, hvis betingelsen er opfyldt
} else {
	// Dette udløses, hvis betingelsen ikke er opfyldt
}
```

Vi behøver ikke altid at definere begge muligheder (nogle gange er det helt unødvendigt). Faktisk kan vi definere situationen, hvis kun betingelsen er opfyldt. Dette gøres på følgende måde:

```php
if (hodnota   operace   hodnota) {
	// Dette udløses, hvis betingelsen er opfyldt
}
```

Logiske operatører
--------------------------

| Operatør | Betydning
|----------|---------
| `==` | Lig med
| `===` | Er lig med og har samme datatype (*Alt kan sammenlignes med alt, men betingelsen er kun opfyldt, hvis det er en værdi af samme datatype (f.eks. tal, tekst, ...)*)
| `!=` | Er ikke lig med
| `<=` | Lig med eller større end
| `>=` | Lig med eller mindre end
| `<` | Større
| `>` | Mindre

Et reelt eksempel
--------------------------

```php
$a = 5;
$b = 3;
if ($a === $b) {
	// blok, der udskrives, hvis $a er lig med $b
} else {
	// blok, der udskrives, hvis $a IKKE er lig med $b
}
```

Inlejrede betingelser
--------------------------

Desværre er resultatet kun `true` (gyldig) og `false` (ugyldig). Så hvis vi ønsker at overveje flere muligheder, skal vi indlejre flere betingelser i hinanden. Dette kaldes en **indlejret betingelse**. Den er indlejret, fordi en af løsningerne på betingelsen blot er en anden betingelse.

```php
$a = 5;         // venstre lomme
$b = 3;         // højre lomme
$kapsa = true;  // Har jeg en lomme?

if ($kapsa === true) {

	if ($a > $b) {
		echo 'I den venstre lomme er der mere';
	} else {
		echo 'I den højre lomme er der mere';
	}

} else {
	echo 'Du har ikke en lomme';
}
```
