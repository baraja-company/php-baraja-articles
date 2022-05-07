Tabella di contingenza in PHP
=============================

> id: '9bdc1004-8f06-48ec-8f56-8707fad5cef7'
> slug:
> 	cs: kontingencni-tabulka
> 	it: tabella-di-contingenza-in-php
> 
> publicationDate: '2019-11-13 22:00:05'
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1
> sourceContentHash: '80fdc1436cd30bc39ffe9c11c3d86c41'

Una tabella di contingenza è generalmente utilizzata per mostrare la relazione tra due fenomeni statistici. Quando si sviluppa un'applicazione web, avremo spesso bisogno di visualizzare la relazione di un certo fenomeno nel database con una sequenza temporale, tipicamente nell'amministrazione.

Per esempio, abbiamo una tabella di ordini che mostra i singoli prodotti e siamo interessati a come le vendite di certi prodotti ad alto volume sono legate al tempo.

Una tabella come la seguente sarebbe utile per questo:

| Datteri, mele, fragole, pere.
|---------|--------|--------|--------|
| 2019-05 | 10 | 15 | 18 |
| 2019-04 | 12 | 18 | 11 |
| 2019-03 | 13 | 9 | 21 |
| 2019-02 | 6 | 17 | 10 |
| 2019-01 | 7 | 4 | 6 |

Non c'è un modo facile per preparare i dati in questo modulo in PHP, e ottenerli direttamente in questo modulo direttamente in SQL non è nemmeno elegante, perché dobbiamo tenere conto che c'è un numero dinamico di colonne.

Quindi dobbiamo essere intelligenti quando progettiamo l'output di questa struttura di dati.

Serializzare i dati usando le chiavi
----------------------------

Quando costruisco una tabella, uso spesso recuperare tutti i record che soddisfano una data condizione direttamente dal database, per esempio i dati di intervallo.

In particolare:

```sql
SELECT *
FROM `order`
WHERE `inserted_date` <= '2019-05-01'
ORDER BY `inserted_date` DESC
```

La query recupera tutte le colonne nella tabella dell'ordine (`ordine`), filtrando tutti i record dall'inizio delle età fino al ``2019-05-01``, restituendoli ordinati dal più recente al più vecchio.

Con una semplice query SQL, otteniamo i dati quasi istantaneamente. La seconda bella caratteristica è che gli indici del database possono essere usati in modo efficiente nella compilazione dei risultati. Tuttavia, poiché abbiamo i dati in un semplice array, dobbiamo serializzarli manualmente in una struttura di dati che può essere convertita in una tabella contig.

Poiché una tabella di contingenza descrive la relazione di due o più fattori, ha senso usare una chiave multidimensionale. Tuttavia, poiché alcuni dati potrebbero non esistere per tutte le combinazioni, è meglio serializzare la chiave in una singola stringa e memorizzare i dati come un array piatto.

I dati possono essere assemblati in un singolo passaggio del ciclo (la variabile `$selection` contiene l'output del database):

```php
$data = [];

foreach ($selection as $row) {
    $date = date('Y-m', $row->insertedDate); // Data anno-mese
    foreach ($row->items as $product) { // passiamo attraverso i prodotti
        $key = $date . '_' . $product->id;
        if (isset($data[$key])) {
            $data[$key]++; // esiste, aggiungeremo un altro prodotto
        } else {
            $data[$key] = 1; // non esiste, inizieremo il primo prodotto
        }
    }
}
```

Se stessimo esplorando una struttura di dati più semplice, un ciclo interno non sarebbe necessario per scorrere i prodotti. In questo caso, l'intera costruzione dei dati potrebbe essere risolta con un solo ciclo.

Con questo approccio, otteniamo un cosiddetto array piatto di valori che assomiglia a una `chiave: valore', mentre immagazzina informazioni bidimensionali.

L'output è quindi, per esempio (nel `maggio 2019`, un prodotto con ID `10` ha venduto `6` unità):

```php
$data = [
    '2019-05_10' => 6,
    ...
];
```

Rendering dei dati in una tabella - modelli
-------------------------------

Se abbiamo i dati sotto forma di un array piatto, possiamo rendere l'intera tabella molto facilmente. Per fare questo, abbiamo solo bisogno di conoscere i campi di tutti i prodotti che ci interessano e i campi di tutte le date per le quali vogliamo tracciare la tabella.

```php
$products = [ ... ]; // campo prodotto: id => nome
$dates = [ ... ]; // per data: data => etichetta

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
echo '</tabella>';
```

Si noti che quando si sfogliano i dati, si cerca un'occorrenza specifica per piegatura della chiave della stringa. Questo approccio ci permette di vincolare o estendere arbitrariamente la tabella renderizzata a seconda dei dati che stiamo consultando. Se i dati non esistono, viene valutato l'operatore ternario `??` e viene visualizzato zero.

Possiamo costruire l'array di prodotti e date disponibili come parte del primo ciclo che prepara i dati. A quel punto, saremo sicuri che stiamo tracciando solo i dati che esistono realmente. In questo caso, è molto importante che l'output dal database SQL sia ordinato per data di creazione, altrimenti le righe potrebbero essere mescolate durante il rendering finale della tabella.
