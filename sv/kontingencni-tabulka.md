Beredskapstabell i PHP
======================

> id: '9bdc1004-8f06-48ec-8f56-8707fad5cef7'
> slug:
> 	cs: kontingencni-tabulka
> 	sv: beredskapstabell-i-php
> 
> publicationDate: '2019-11-13 22:00:05'
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1
> sourceContentHash: '80fdc1436cd30bc39ffe9c11c3d86c41'

En contingencytabell används i allmänhet för att visa förhållandet mellan två statistiska fenomen. När vi utvecklar en webbapplikation behöver vi ofta visualisera förhållandet mellan ett visst fenomen i databasen och en tidssekvens, vanligtvis i administrationen.

Vi har till exempel en ordertabell som visar enskilda produkter och vi är intresserade av hur försäljningen av vissa produkter med hög volym är relaterad till tiden.

En tabell som följande skulle vara användbar för detta:

| Datum | Äpplen | Jordgubbar | Päron | Päron
|---------|--------|--------|--------|
| 2019-05 | 10 | 15 | 18 |
| 2019-04 | 12 | 18 | 11 |
| 2019-03 | 13 | 9 | 21 |
| 2019-02 | 6 | 17 | 10 |
| 2019-01 | 7 | 4 | 6 |

Det finns inget enkelt sätt att förbereda data till det här formuläret i PHP, och det är inte heller elegant att få in dem direkt i det här formuläret direkt i SQL, eftersom vi måste ta hänsyn till att det finns ett dynamiskt antal kolumner.

Därför måste vi vara smarta när vi utformar utgången av denna datastruktur.

Serialisering av data med hjälp av nycklar
----------------------------

När jag bygger en tabell använder jag ofta alla poster som uppfyller ett visst villkor direkt från databasen, t.ex. intervalldata.

Särskilt:

```sql
SELECT *
FROM `order`
WHERE `inserted_date` <= '2019-05-01'
ORDER BY `inserted_date` DESC
```

Frågan hämtar alla kolumner i ordertabellen (`order`), filtrerar alla poster från början av åldrarna fram till ``2019-05-01``` och returnerar dem sorterade från nyast till äldst.

Med en enkel SQL-fråga får vi uppgifterna nästan omedelbart. Den andra trevliga funktionen är att databasindex kan användas effektivt för att sammanställa resultaten. Men eftersom vi har data i en vanlig array måste vi manuellt serialisera dem till en datastruktur som kan konverteras till en contig-tabell.

Eftersom en kontingenstabell beskriver förhållandet mellan två eller flera faktorer är det meningsfullt att använda en flerdimensionell nyckel. Eftersom vissa data kanske inte finns för alla kombinationer är det dock bättre att serialisera nyckeln till en enda sträng och lagra data som en platt matris.

Uppgifterna kan sammanställas i ett enda looppass (variabeln `$selection` innehåller utdata från databasen):

```php
$data = [];

foreach ($selection as $row) {
    $date = date('Y-m', $row->insertedDate); // Datum år-månad
    foreach ($row->items as $product) { // vi går igenom produkterna
        $key = $date . '_' . $product->id;
        if (isset($data[$key])) {
            $data[$key]++; // finns, vi lägger till ytterligare en produkt
        } else {
            $data[$key] = 1; // inte existerar, vi börjar med den första produkten
        }
    }
}
```

Om vi hade undersökt en enklare datastruktur skulle en inre slinga inte behövas för att bläddra bland produkterna. I det här fallet kan hela datauppbyggnaden lösas med en enda cykel.

Med detta tillvägagångssätt får vi en så kallad platt array av värden som ser ut som en "nyckel: värde", som lagrar tvådimensionell information.

Utdata blir då till exempel (i "maj 2019" sålde en produkt med ID "10" "6" enheter):

```php
$data = [
    '2019-05_10' => 6,
    ...
];
```

Rendering av data till en tabell - mallar
-------------------------------

Om vi har data i form av en platt matris kan vi enkelt återge hela tabellen. För att göra detta behöver vi bara känna till fälten för alla produkter som vi är intresserade av och fälten för alla datum som vi vill rita tabellen för.

```php
$products = [ ... ]; // produktfält: id => namn
$dates = [ ... ]; // efter datum: datum => etikett

echo '<table>';
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
echo '</table>';
```

Observera att när du bläddrar i data söker du efter en specifik förekomst genom att vikta strängnyckeln. Detta tillvägagångssätt gör det möjligt att begränsa eller utöka den återgivna tabellen godtyckligt beroende på vilka data vi tittar på. Om uppgifterna inte finns, utvärderas den ternära operatören `??` och noll visas.

Vi kan bygga upp en matris med tillgängliga produkter och datum som en del av den första cykeln som förbereder data. Då kan vi vara säkra på att vi bara plottar data som faktiskt finns. I det här fallet är det mycket viktigt att utdata från SQL-databasen är sorterade efter skapelsedatum, annars kan raderna blandas om under den slutliga tabellernas rendering.
