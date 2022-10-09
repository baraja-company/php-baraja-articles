Kontingenstabel i PHP
=====================

> id: '9bdc1004-8f06-48ec-8f56-8707fad5cef7'
> slug:
> 	cs: kontingencni-tabulka
> 	da: kontingenstabel-i-php
> 
> publicationDate: '2019-11-13 22:00:05'
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1
> sourceContentHash: '80fdc1436cd30bc39ffe9c11c3d86c41'

En kontingenstabel bruges generelt til at vise forholdet mellem to statistiske fænomener. Når vi udvikler en webapplikation, har vi ofte brug for at visualisere forholdet mellem et bestemt fænomen i databasen og et tidsforløb, typisk i administrationen.

Vi har f.eks. en tabel med ordrer, der viser individuelle produkter, og vi er interesseret i, hvordan salget af visse produkter med stor volumen er relateret til tid.

En tabel som den følgende ville være nyttig til dette formål:

| Dato | Æbler | Jordbær | Pærer | Pærer
|---------|--------|--------|--------|
| 2019-05 | 10 | 15 | 18 |
| 2019-04 | 12 | 18 | 11 |
| 2019-03 | 13 | 9 | 21 |
| 2019-02 | 6 | 17 | 10 |
| 2019-01 | 7 | 4 | 6 |

Der er ingen nem måde at forberede dataene til denne formular i PHP, og det er heller ikke elegant at få dem direkte ind i denne formular direkte i SQL, fordi vi skal tage hensyn til, at der er et dynamisk antal kolonner.

Så vi skal være smarte, når vi udformer output af denne datastruktur.

Serialisering af data ved hjælp af nøgler
----------------------------

Når jeg opbygger en tabel, bruger jeg ofte at hente alle poster, der opfylder en given betingelse, direkte fra databasen, f.eks. intervaldata.

Mere specifikt:

```sql
SELECT *
FROM `order`
WHERE `inserted_date` <= '2019-05-01'
ORDER BY `inserted_date` DESC
```

Forespørgslen henter alle kolonner i ordretabellen (`order`), filtrerer alle poster fra begyndelsen af alderen til ``2019-05-01``` og returnerer dem sorteret fra nyeste til ældste.

Med en simpel SQL-forespørgsel får vi dataene næsten øjeblikkeligt. Den anden gode egenskab er, at databaseindekser kan bruges effektivt til at kompilere resultaterne. Men da vi har dataene i et almindeligt array, skal vi yderligere manuelt serialisere dem til en datastruktur, der kan konverteres til en contig-tabel.

Da en kontingenstabel beskriver forholdet mellem to eller flere faktorer, er det fornuftigt at bruge en flerdimensional nøgle. Men da nogle data måske ikke findes for alle kombinationer, er det bedre at serialisere nøglen til en enkelt streng og gemme dataene som et fladt array.

Dataene kan samles i et enkelt loop-pass (variablen `$selection` indeholder output fra databasen):

```php
$data = [];

foreach ($selection as $row) {
    $date = date('Y-m', $row->insertedDate); // Dato år-måned
    foreach ($row->items as $product) { // vi gennemgår produkterne
        $key = $date . '_' . $product->id;
        if (isset($data[$key])) {
            $data[$key]++; // findes, vil vi tilføje et andet produkt
        } else {
            $data[$key] = 1; // ikke findes, starter vi det første produkt
        }
    }
}
```

Hvis vi udforskede en enklere datastruktur, ville det ikke være nødvendigt med en indre løkke for at gennemse produkterne. I dette tilfælde kan hele dataopbygningen løses med en enkelt cyklus.

Med denne fremgangsmåde får vi et såkaldt fladt array af værdier, der ligner en "nøgle: værdi", men som lagrer todimensionale oplysninger.

Output er så f.eks. (i `maj 2019`, et produkt med ID `10` solgte `6` enheder):

```php
$data = [
    '2019-05_10' => 6,
    ...
];
```

Rendering af data i en tabel - skabeloner
-------------------------------

Hvis vi har dataene i form af et fladt array, kan vi meget let gengive hele tabellen. For at gøre dette skal vi blot kende felterne for alle de produkter, vi er interesseret i, og felterne for alle de datoer, som vi ønsker at tegne tabellen for.

```php
$products = [ ... ]; // produktfelt: id => navn
$dates = [ ... ]; // efter dato: date => label

echo '<tabel>';
foreach ($products as $productId => $productName) {
    echo '<tr>';
    foreach ($dates as $date => $dateLabel) {
        echo '<td>';
        echo htmlspecialchars(
            (string) ($data[$date . '_' . $productId] ?? '0')
        );
        echo '<td>';
    }
    echo '</tr>';
}
echo '</tabel>';
```

Bemærk, at når du gennemser dataene, søger den efter en specifik forekomst ved at folde en strengnøgle. Denne fremgangsmåde giver os mulighed for at begrænse eller udvide den gengivne tabel vilkårligt afhængigt af, hvilke data vi gennemser. Hvis dataene ikke findes, evalueres den ternære operatør `??`, og der vises nul.

Vi kan opbygge arrayet af tilgængelige produkter og datoer som en del af den første cyklus, der forbereder dataene. På det tidspunkt er vi sikre på, at vi kun plotter de data, der rent faktisk findes. I dette tilfælde er det meget vigtigt, at output fra SQL-databasen er sorteret efter oprettelsesdato, da det ellers kan ske, at rækkerne bliver blandet om under den endelige tabelgengivelse.
