UUID e prestazioni delle applicazioni su larga scala
====================================================

> id: '2f072ce8-13b1-41f6-b328-2bd3b416cdd2'
> slug:
> 	cs: uuid-performance
> 	it: uuid-e-prestazioni-delle-applicazioni-su-larga-scala
> 
> publicationDate: '2019-11-08 10:09:54'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: '3b6d0e37684aedc0aedd92f2654e3626'

Quando la dimensione del database cresce oltre i milioni di righe, è consigliabile iniziare a scalare l'applicazione e dividere il database in più server fisici.

Il più grande problema della divisione del database in più parti è la sua successiva sincronizzazione se l'utente richiede dati specifici.

Perché usare UUID e quali sono i suoi vantaggi rispetto all'autoincremento
--------------------------------------------------------

Supponete di avere una tabella di `articoli`, ma poiché avete un sito enorme, ci sono decine di milioni di articoli in più e dovete dividerli fisicamente su più macchine.

Se dovessimo usare un normale numero intero come `id` (chiave primaria) con l'impostazione come autoincremento, scopriremmo molto rapidamente che quando si creano record su diverse macchine in modo decentralizzato e poi li si sincronizza, ci sono collisioni di ID e dobbiamo rinumerare i record in modo complicato. Inoltre, se stiamo risolvendo molte sessioni in altre tabelle, questo può essere un overhead molto complesso in cui è facile fare errori.

Pertanto, invece di un identificatore numerico, possiamo generare un `UUUID`, che è una stringa di testo generata da un algoritmo complesso che garantisce che sarà unico anche se viene generato indipendentemente su più macchine.

Vantaggi:

- Se avete più database indipendenti che poi sincronizzate, usare un UUID significa che un ID è unico in tutti i database, non solo quello in cui vi trovate e dove è stato generato. Quando si fondono in un unico cluster, non sorgono conflitti.
- Potete conoscere la vostra "chiave primaria" prima di inserire effettivamente il record nel database. Questo riduce il numero di query SQL, semplifica la logica delle transazioni, e si può facilmente usare come chiave esterna prima che la collezione di record esista.
- L'UUID non rivela informazioni sul numero di date e sequenze ed è più sicuro da usare negli URL. Per esempio, se trovo che sono l'utente `19010018`, è facile indovinare che anche l'utente `19010017` e altri esistono. L'attacco è chiamato attacco vettoriale.

Generare un nuovo UUID
----------------------

L'UUID può essere ottenuto sia con una semplice query SQL `SELECT UUID();`, ma questo aumenta il numero di query al database e si perde la possibilità di preparare i dati prima in blocco nella logica dell'applicazione e poi scriverli in una volta sola.

Pertanto, mi piace usare il pacchetto <a href="https://github.com/ramsey/uuid">ramsey/uuid</a> ottenuto da Composer come buona soluzione. L'UUID stesso ha diverse versioni, e il pacchetto può generare giocosamente tutti i tipi secondo necessità.

Questo lo rende facile da usare:

```php
require 'venditore/autoload.php';

use Ramsey\Uuid\Uuid;

// Genera l'oggetto UUID versione 1 (basato sul tempo)
$uuid1 = Uuid::uuid1();
echo $uuid1->toString() . "\n"; // e4eaaaf2-d142-11e1-b3e4-080027620cdd

// Genera la versione 3 (basata sul nome e con hash come MD5) dell'oggetto UUID
$uuid3 = Uuid::uuid3(Uuid::NAMESPACE_DNS, 'php.net');
echo $uuid3->toString() . "\n"; // 11a38b9a-b3da-360f-9353-a5a725514269

// Genera la versione 4 (casuale) dell'oggetto UUID
$uuid4 = Uuid::uuid4();
echo $uuid4->toString() . "\n"; // 25769c6c-d34d-4bfe-ba98-e0ee856f3e7a

// Genera la versione 5 (basata sul nome e con hash come SHA1) dell'oggetto UUID
$uuid5 = Uuid::uuid5(Uuid::NAMESPACE_DNS, 'php.net');
echo $uuid5->toString() . "\n"; // c4a760a8-dbcf-5254-a0d9-6a4474bd1b62
```

Se usate Doctrine, c'è un'estensione <a href="https://github.com/ramsey/uuid-doctrine">ramsey/uuid-doctrine</a> che genera l'ID direttamente come tipo di dati.

Archiviazione fisica nel database
---------------------------

Nei miei primi tentativi ho usato `varchar(36)` come chiave primaria (ID), ma <a href="https://www.facebook.com/groups/backendisti/permalink/2465260887049808/">questa non è affatto una buona idea</a>.

> **Spiegazione della logica interna:**
>
> > I database MySql (e molti altri) non possono usare `varchar`, `char` o altri tipi di dati che esprimono una stringa come chiave primaria in modo efficiente.
> In alcuni database, c'è un tipo di dato `GUID` che è progettato per memorizzare direttamente gli UUID. Se non potete usare questo tipo, c'è un sostituto adatto nella forma `binary(16)`.

Quando si esamina fisicamente il database, l'ID è quindi rappresentato in formato HEX (poiché il formato binario non può essere visualizzato), invece del simpatico ID `726c67c4-e5eb-4a4c-8fcc-031da5d6f3c6`, si vedrà semplicemente `726C67C4E5EB4A4C8FCC031DA5D6F3C6`, che appare come `'?kYߟKg2c;'` nella query INSERT.

Convertire i dati originali da `varchar(36)` a `binary(16)`.
----------------------------------------------------

Suppongo che tu rappresenti (o pensi di rappresentare) l'ID appena impostato nel database come:

```sql
`id` binary(16) NOT NULL
```

Tuttavia, cambiare semplicemente il tipo di dati non funziona, quindi qualcosa come:

```sql
SET FOREIGN_KEY_CHECKS=0;

ALTER TABLE article CHANGE id id BINARY(16) NOT NULL

SET FOREIGN_KEY_CHECKS=1;
```

Ci sono fondamentalmente due ragioni:

- La chiave primaria e la sessione ad essa `devono avere lo stesso tipo di dati`. Pertanto, è necessario cambiare sia il tipo di dati per l'ID dell'articolo che, per esempio, nella tabella relazionale che abbina gli articoli agli autori.
- Il formato binario contiene qualcosa di leggermente diverso dalla stringa originale. È necessario utilizzare una funzione di conversione.

Quindi, l'unica soluzione corretta è quella di fare un backup dei dati (ma dovreste comunque farlo prima di ogni migrazione), preparare un database vuoto con relazioni funzionali e metterci di nuovo i dati tramite la migrazione.

Se avete generato gli UUID in modo strano prima, è meglio scegliere qualche metodo sequenziale per ottenere l'UUID e rinumerare tutti i record. La ragione è che il layout sequenziale permette di ordinare meglio i valori e di creare un `btree`, il che rende le prestazioni quasi identiche a quelle di `bigint`.

**Se conoscete un modo migliore per convertire un database esistente da UUID memorizzati come varchar al formato binario senza dover escogitare migrazioni complesse e con la conservazione delle chiavi esterne, sarei molto grato per un feedback**.
