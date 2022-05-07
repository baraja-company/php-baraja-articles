Algoritmo dei motori di ricerca su Internet - ordinamento e descrittore
=======================================================================

> id: ca4aaac5-0cd8-41db-b860-c32661c43d82
> slug:
> 	cs: vyhledavac-trideni-a-popisovac
> 	it: algoritmo-dei-motori-di-ricerca-su-internet---ordinamento-e-descrittore
> 
> publicationDate: '2016-09-11 13:00:00'
> mainCategoryId: '367f936c-073f-44bd-b399-30738e93137a'
> sourceContentHash: '541310dfdc96b75a320c7cad1b03e734'

In questa lezione sui principi di un motore di ricerca su Internet, capiremo come un motore di ricerca ordina, descrive e valuta i risultati.

Ordinamento dei risultati
----------------

Immaginiamo un barile finito che è attualmente pronto sul server di ricerca. La nostra prima query di ricerca arriva dall'utente e ora abbiamo bisogno di fare il primo ordinamento "grezzo", che sarà ulteriormente raffinato.

Facciamo la seguente query di input di esempio:

```txt
[O (and) pejskovi (and) a (and) kočičce]
```

Sì, questa è la forma in cui il server di ricerca riceve la query elaborata dall'utente e ora aspetta che il risultato venga restituito. In totale abbiamo meno di `300 ms` per fare questo, quindi veloce :)

Nel primo passo, otteniamo dei barili pieni di ID di risultati futuri (chiamati anche candidati) e di operazioni da eseguire. Questo barile recuperato può non essere completo in alcuni casi, ma contiene solo i valori che il server è riuscito a leggere dal disco in qualche intervallo di tempo predeterminato (chiamato anche **timeout**). Per esempio, otteniamo le seguenti serie di numeri (che sono parzialmente ordinati in anticipo):

| Barili | Documenti |
|-------|---------|
| o | 2,3,4,5,9,12,15,19,23,26,28,34,39,42,58,67,83,91,115 |
| cane | 5,19,23,42,46,58 |
| a | 2,3,4,5,9,12,15,19,23,26,28,34,39,42,58,67,83,91,115 |
| figa | 9,19,42,57,58,62,68,83 |

Ora eseguiamo un'intersezione grossolana di tutti gli insiemi e otteniamo una lista di ID dei documenti che sono gli stessi in tutti i barili. In pratica, passiamo attraverso la lista più corta e cerchiamo le altre.

Gli ID comuni a tutti i barili sono "19, 42, 58", quindi la futura pagina dei risultati contiene 3 elementi in un ordine ancora sconosciuto. Continuiamo a considerare i documenti come ID perché in questo modo si risparmia un'enorme quantità di dati. Nei risultati di ricerca, tuttavia, generalmente non vogliamo elencare i numeri dei documenti all'utente, ma piuttosto il titolo del documento (intestazione), la descrizione, l'URL e altre informazioni. Questo è fornito dal componente "descrittore".

Descrittore
---------

Dal capitolo precedente ci rimane una lista di candidati per i risultati futuri. Lo abbiamo recuperato in forma ordinata in termini di sistema, cioè, come il motore di ricerca li ha classificati in modo sequenziale nell'indice. A questo punto, non possiamo più eseguire una lettura sequenziale dei big data, ma dobbiamo attraversare sequenzialmente un qualche tipo di database relazionale per trovare informazioni aggiuntive per un altro tipo di ordinamento più comprensibile per l'utente.

Per esempio, una fetta di database potrebbe essere come questa:


| ID | Titolo | Etichetta | URL | Rank |
|----|---------|---------|-----|------|
| 19 | Josef Čapek: A Tale of a Dog and a Cat | Era quando il cane e il gatto facevano ancora i contadini insieme; avevano la loro casetta vicino al bosco e ci vivevano insieme e volevano fare tutto come fanno i grandi | www.troglodytarium.cz/webz/axf/.../Capek_J_Pejsek_a_kocicka.htm | 72 |
| "Va bene", disse il cane, e il gatto prese del sapone e una pentola d'acqua, si inginocchiò sul pavimento, prese il cane come spazzola e strofinò tutto il pavimento con il cane. Il pavimento era tutto bagnato | www.capek.narod.ru/povidani.htm | 86 |
| 58 | Come il cane e il gatto fecero una torta per la festa| Domani era la festa del cane e il compleanno del gatto. I bambini lo sapevano e volevano fare una sorpresa al cane e al gatto per il loro compleanno. Stavano pensando cosa potevano fare per il cane e | a.da.mek.sweb.cz/capek.j/dort.htm | 34 |

Calcolo della rilevanza
-----------------

L'ultimo passo è calcolare la rilevanza della risposta. Per la maggior parte dei risultati, è sufficiente rappresentare la rilevanza come un singolo numero con un raggio fisso in base al quale i risultati saranno ordinati. Per rango, i risultati sono di nuovo ordinati "approssimativamente" in diversi gruppi (il numero di gruppi dipende dalla varianza dei ranghi) e questi gruppi sono ulteriormente lavorati.

I gruppi con il rango più alto vengono presi per primi, spesso questo è sufficiente per ordinare la prima pagina dei risultati e non dobbiamo occuparci di altro. Tuttavia, se diversi documenti hanno lo stesso rango, dobbiamo fare un'analisi più dettagliata e calcolare ulteriori ranghi ausiliari per aiutare a classificare la pagina. Un rango aggiuntivo può essere per esempio la qualità dei link, l'età del documento, la lunghezza del titolo, il "look and feel" dell'URL e molti altri fattori.

Restituire i risultati all'utente
---------------------------

Urrà! Abbiamo i risultati e ora non ci resta che rendere la pagina per l'utente. Il pacchetto dei risultati viene riconsegnato al server, che inserisce i risultati in un modello già preparato, inserisce i banner pubblicitari intorno alla pagina e rimanda la pagina all'utente. Allo stesso tempo, può anche eseguire altre operazioni ausiliarie, come il caching dei risultati. Se qualcun altro cerca la stessa query in un prossimo futuro (per esempio, nell'ultima ora), i risultati non saranno cercati di nuovo, ma solo letti dalla memoria temporanea - il che spesso aiuta a migliorare le prestazioni complessive del motore di ricerca.

Conclusione e approfondimento sulla semantica
---------------------------

Questo articolo aveva lo scopo di descrivere i principi di base del funzionamento interno dei motori di ricerca e come riescono a recuperare un numero teoricamente illimitato di documenti in un tempo ancora ragionevole. Si tratta di enormi ottimizzazioni matematiche che sono state sviluppate durante molti anni dai migliori team di programmazione.

Una descrizione completa dei dettagli va oltre lo scopo di questo articolo. Il tutto è anche complicato dal fatto che la maggior parte degli altri passi non sono descritti in dettaglio da nessuno dei motori di ricerca, perché sono i loro segreti commerciali.

È anche importante notare che i motori di ricerca non capiscono ancora il contenuto delle pagine e il significato dei risultati, ed è ancora solo un calcolo statistico basato sulla potenza di calcolo grezza e la capacità delle persone di collegarsi a testi di qualità, e questo sistema è anche abbastanza vulnerabile (prendete l'esempio della "bomba di Google", dove un webmaster subdolo forza un contenuto su una query di ricerca che è irrilevante).

Questo problema dovrebbe essere in parte risolto dall'intelligenza artificiale, che tutti i motori di ricerca stanno gradualmente cercando di integrare. Tuttavia, questa è ancora una lontana musica del futuro, poiché non è un compito facile e per lo più si tratta solo di migliorare i metodi di calcolo statistico. Facciamo l'esempio di Google graph search, che cerca di rispondere direttamente alla query (anche se nella sua struttura interna cerca ancora solo in una base di conoscenza predefinita).

Quindi la prossima volta che chiedete una query al vostro motore di ricerca preferito, ricordate quanto lavoro ha dovuto fare il vostro motore di ricerca e quanti TB di dati ha dovuto leggere dal disco per trovare i primi 10 documenti per la vostra query di ricerca.
