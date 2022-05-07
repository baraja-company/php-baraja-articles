Funzione PHP date(), data e ora
===============================

> id: '9b0ec1c7-3431-4d7d-9bcc-6093285f14f1'
> slug:
> 	cs: date
> 	it: funzione-php-date-data-e-ora
> 
> perex:
> 	- 'Zjištění data a času, aktuální datum, formátování data a času a převod tvaru.'
> 	- 'Rilevamento di data e ora, data corrente, formattazione di data e ora e conversione di forma.'
> 
> publicationDate: '2019-09-11 10:14:16'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: d0323ba88fcdba84ef45439991301f6c

La funzione `date()` è uno strumento per lavorare con la data e l'ora. Si usa in due casi:

- Trovare lo stato attuale**, cioè la data, l'ora, ... e lo produce in un formato specifico,
- **Convertire** una data specifica in un altro formato (per esempio, da numero del mese a nome, formato di notazione dell'anno, sistema a 12 e 24 ore, ...).

Campione
------

```html
Já jsem speciální stránka. Vím, že právě je
<?php
    echo date('H:i'); // Hodina:minuta
?>
```

> ATTENZIONE: PHP non stampa il tuo tempo, ma il tempo sul server. Pertanto, si può ottenere un orario diverso da quello impostato sul computer.

Sintassi di ingresso
--------------------------

La funzione viene chiamata in modo normale e le singole richieste vengono inserite come argomenti della funzione.

```php
echo date('segni di formattazione', atribut konkrétního času);
```

I segni di formattazione indicano in quale formato verrà stampata la data. I segni possono includere spazi, punti, due punti, trattini e altri caratteri che non sono essi stessi segni di formattazione (se vuoi usare un segno di formattazione, devi <a href="/carriage-notes">escluderlo</a>). Una panoramica di ogni tag è qui sotto.

Il secondo attributo (opzionale) indica la data o l'ora inserita manualmente, che sarà convertita e mostrata nel formato secondo il primo parametro. Deve essere specificato come *timestamp* (può essere ottenuto tramite il tag di formattazione "U").

Esempio:

```php
echo date('d. m. Y', 1405856605); // entro il 20. 07. 2014
```

Tabella dei segni di formattazione consentiti
--------------------------

| Carattere | Descrizione
|------|---------------------
| `Y` | Anno come quattro cifre (per esempio, 1998)
| Anno come doppia cifra (es. 98)
| `M` | Abbreviazione inglese del nome del mese (es. Jan)
| `m` | Numero del mese (01-12)
| `F` | Nome del mese inglese (ad esempio, gennaio)
| `D` | Abbreviazione inglese del giorno della settimana (es. Fri)
| `l` | nome inglese del giorno della settimana (per esempio venerdì)
| `N` | Numero del giorno della settimana (1 - lunedì, 7 - domenica)
| `w` | Numero del giorno della settimana (0 - domenica, 1 - lunedì, 6 - sabato)
| `d` | Giorno del mese (01-31)
| `j` | Numero del giorno del mese (1-31)
| `z` | Giorno dell'anno (001-365)
| `H` | Ora (00-23)
| Ora (01-12)
| `i` | Minuto (00-59)
| Secondo (00-59)
| `U` | *Timestamp:* Numero di secondi dall'inizio del tempo (dal 1° gennaio 1970)
| `S` | finale inglese del numero ordinale del giorno del mese
| A` | Indicatore AM/PM
| `a` | Indicatore mattina/pomeriggio (am/pm)
| `P` | Differenza dall'ora di Greenwich (GMT) con separatore tra ore e minuti (aggiunto in PHP 5.1.3), per esempio: `+02:00`.
| `g` | Ora in formato 12 ore (1-12)
| `G` | Ora in formato 24 ore (0-23)

Formattazione temporale per la mappa del sito
---------------------------------

Molto spesso è necessario formattare l'ora per il file `sitemap.xml`, che contiene la data e l'ora dell'ultima modifica nel tag `<lastmod>`.

A partire da PHP 5.1.3, la seguente sintassi può essere usata per fare questo:

```php
date('Y-m-d\TH:i:sP');
```

La data e l'ora saranno visualizzate al secondo più vicino e includeranno anche informazioni sul fuso orario in cui si trova il server (il fuso orario è determinato dal sistema operativo del server).

Ottenere i nomi cechi dei giorni e dei mesi
----------------------------------

Non è possibile ottenere i nomi cechi dei giorni e dei mesi in PHP nel modo normale, quindi dobbiamo scrivere noi stessi tali valori. Il modo migliore è quello di memorizzare le voci in un array e recuperarle usando una chiamata all'indice.

```php
$mesice = [
    1 => 'Gennaio', 'Febbraio', 'Marzo', 'Aprile', 'Maggio',
    'Giugno', 'Luglio', 'Agosto', 'Settembre', 'Ottobre',
    'Novembre', 'Dicembre'
];

$dny = ['Domenica', 'Lunedì', 'Martedì', 'Mercoledì', 'Giovedì', 'Venerdì', 'Sabato'];
```

Questo esempio è un esempio semplificato da <a href="https://php.vrana.cz">**Jakub Vrana**</a> dall'articolo <a href="https://php.vrana.cz/ceske-nazvy-mesicu-a-dnu-v-tydnu.php">Nomi cechi dei mesi e dei giorni della settimana</a>.

Tradurre le date dall'inglese all'inglese
--------------------------------------

Spesso abbiamo una data nel formato `Friday, 13 September` e vogliamo tradurla in inglese, ma come fare? Il modo migliore è chiamare qualche funzione, per esempio `datumcesky()`, a cui passiamo la data inglese e la traduce.

```php
function datumCesky(string $date): string
{
    $men = [
        'Gennaio', 'Febbraio', 'Marzo', 'Aprile', 'Maggio',
        'Giugno', 'Luglio', 'Agosto', 'Settembre', 'Ottobre',
        'Novembre', 'Dicembre'
    ];

    $mcz = [
        'Gennaio', 'Febbraio', 'Marzo', 'Aprile', 'Maggio',
        'Giugno', 'Luglio', 'Agosto', 'Settembre', 'Ottobre',
        'Novembre', 'Dicembre'
    ];

    $date = str_replace($men, $mcz, $date);

    $den = [
        'Lunedì', 'Martedì', 'Mercoledì', 'Giovedì',
        'Venerdì', 'Sabato', 'Domenica'
    ];

    $dcz = [
        'Lunedì', 'Martedì', 'Mercoledì', 'Giovedì',
        'Venerdì', 'Sabato', 'Domenica'
    ];

    return str_replace($den, $dcz, $date);
}
```

Esempio di utilizzo:

```php
echo datumCesky('Venerdì 13 settembre'); // Venerdì, 13 dicembre
```

Questa funzione non è sicuramente ideale per la traduzione, poiché sostituisce solo le parole inglesi con quelle ceche, ma può essere sufficiente per molte distribuzioni. Per traduzioni più avanzate, dovresti sempre garantire la sintassi esatta per tradurre in uno stile uniforme, ad esempio `Venerdì 13 dicembre`.

Trovare il primo giorno del mese
-----------------------------

Il primo giorno di aprile 2018 era una domenica, ma come scoprirlo facilmente?

A partire da PHP 5.1.0, c'è una soluzione semplice:

```php
echo date('N', strtotime('2018-04-01')); // 1 (lunedì), 7 (domenica)
```

Nelle vecchie versioni dobbiamo implementare noi stessi la funzione:

```php
/**
 * @autore Jan Barášek
 */
function getFirstDayPosition(?int $year = null, ?int $month = null): int
{
    $day = (int) date('w', strtotime($year . '-' . $month . '-1')) - 1;

    return $day < 0 ? 7 : $day + 1;
}
```

Poiché il modificatore `w` restituisce l'output in formato USA, modifico ancora il numero del giorno con un semplice calcolo. La funzione restituisce un numero intero tra 1 (lunedì) e 7 (domenica).

Offset temporali / conversione della formattazione e convalida della data
--------------------------------------------------

Spesso abbiamo bisogno di saltare per un tempo relativo (diciamo +5 giorni), o di estrarre una data dall'input di testo di un utente per renderla valida. Per fare questo, la funzione <a href="https://www.php.net/manual/en/function.strtotime.php">strtotime()</a> permette la seguente sintassi:

```php
echo strtotime('ora');
echo strtotime('10 settembre 2000');
echo strtotime('+1 giorno');
echo strtotime('+1 settimana');
echo strtotime('+1 settimana 2 giorni 4 ore 2 secondi');
echo strtotime('il prossimo giovedì');
echo strtotime('lunedì scorso');
```

L'output della funzione è **timestamp** *(tempo universale)* del tempo specificato come un intero.

> In generale, consiglio di distribuire questa funzione su tutti i form in cui lavoriamo con il tempo dell'utente in qualche modo. Non è certamente una buona idea forzare l'utente a scrivere la data in un formato particolare, ma creare sempre un tale formato automaticamente in modo che l'utente possa scrivere quello che vuole. Perché spesso il testo, per esempio, è copiato da qualche parte ed è troppo lavoro riformattarlo manualmente quando può essere fatto automaticamente.

Numero di secondi dall'inizio del tempo
--------------------------

Dal 1° gennaio 1970, ogni secondo viene aggiunto a uno per dare un enorme numero intero che indica il tempo trascorso da allora. Questo è particolarmente utile per calcolare semplicemente le differenze di tempo. È una soluzione abbastanza affidabile, ma ha i suoi rischi (il problema del 2038).

**Proprietà generali di questa notazione:**
- È un numero intero, quindi è facile lavorarci,
- È nel sistema decimale, quindi è facile da leggere per gli esseri umani (tranne che non si può dire rapidamente qual è l'ora esatta),
- Non può essere usato per rappresentare il periodo prima del 1 gennaio 1970, perché non può essere negativo *(alcune nuove versioni dell'algoritmo implementano tempi negativi, ma non si può sempre contare su questo)*,
- Nel 2038 smetterà di funzionare sui computer a 32 bit e causerà il blocco del programma (a causa dello stack overflow e della dimensione massima degli interi).

Il problema del 2038
--------------------------

<h2 id="universalTime" style="background: #222; margin-top: 32px; padding: 32px; color: white; text-align: center; border-radius: 3px;">Non sto leggendo...</h1>

<script>
	setInterval(function() {
		window.document.getElementById('universalTime').innerHTML = Math.floor(Date.now() / 1000);
	}, 250);
</script>

> Il valore attuale dell'ora viene visualizzato sopra questo testo, che viene incrementato di +1 ogni secondo. Il valore attuale è caricato da javascript, quindi potrebbe non essere sempre preciso (indica l'ora di sistema del tuo computer).

I computer memorizzano i numeri interi in forma binaria, e se è un numero intero, spesso ha 32 bit (32 uno e zero). Il numero più alto possibile a 32 bit è: `011 111 111 111 111 111 111 111 111 111 111 11`.

Tuttavia, se aggiungiamo +1 a questa costante ogni secondo, otterremo un giorno un tale numero *(sarà il 19 gennaio 2038 alle 03:14:07)*. Tuttavia, quando cerchiamo di aggiungere un altro 1, l'intervallo di bit non sarà più sufficiente (il numero sarebbe più grande di quello che può essere memorizzato in 32 bit), quindi ci sarà un **stack overflow**, cioè verrà aggiunto un 1 all'inizio del numero, che in forma binaria significa un **negativo**, (quindi questo probabilmente causerà il crash del programma), portandoci da qualche parte nell'anno 1901 (ugh!).

Ci sono due soluzioni a questo problema:

- Estendere la gamma di numeri interi da 32 bit a 64 bit, dandoci un arco di diverse migliaia di anni
- Usa il tipo di dati `\DateTime` (comunemente disponibile in PHP), che fornisce un approccio orientato agli oggetti per la gestione delle date, è ben compatibile con il database, e offre una gamma di anni da `0001` a `9999`, che è sufficiente per i bisogni della maggior parte delle applicazioni del mondo reale.

Personalmente, raccomando di usare il tipo di dati `DateTime` e di non usare affatto la memorizzazione di interi.

Fusi orari diversi
-----------------

PHP è intelligente, quindi cerca sempre di usare il miglior fuso orario attuale dove si trova il server. Tuttavia, a volte può succedere che il fuso orario sia specificato in modo errato, oppure stiamo programmando un'applicazione globale a cui accedono utenti da tutto il mondo e quindi dobbiamo cambiare manualmente il fuso orario.

Questo può essere fatto globalmente per l'intero script PHP chiamando una funzione:

```php
date_default_timezone_set('UTC');
```

Occasionalmente, si può anche incontrare questa soluzione (rilevare che il server ha un fuso orario diverso da UTC):

```php
$defaultTimeZone = 'UTC';

if (date_default_timezone_get() != $defaultTimeZone) {
    date_default_timezone_set($defaultTimeZone);
}
```

Cambia data e ora
--------------------------

Non è PHP stesso che è responsabile per ottenere la data e l'ora corrente, ma il sistema operativo su cui è in esecuzione. Quindi, se avete bisogno di cambiare manualmente l'ora, cambiatela lì.
