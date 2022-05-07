Cifra di Cesare - come funziona
===============================

> id: '662dd627-c106-406e-ad6a-7ae9a492ad92'
> slug:
> 	cs: cesarova-sifra
> 	it: cifra-di-cesare---come-funziona
> 
> publicationDate: '2019-08-23 15:43:02'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '892ad0746be469b5cdbb4de72d8e85b9'

> **Attenzione:** Questo articolo è stato scritto molti anni fa e alcune informazioni potrebbero essere obsolete o errate. Per favore, tenetelo a mente quando leggete.

Il cifrario di Cesare è una delle funzioni di hashing più semplici. Ai suoi tempi era praticamente indistruttibile, ma nell'era dei computer moderni bastano poche decine di secondi, anche qualche minuto, per romperlo. Si basa su una chiave, secondo la quale il messaggio viene criptato e poi secondo la quale può essere espanso di nuovo. La chiave è quindi segreta. Al momento della crittografia, il messaggio può essere visto e non significherà nulla (solo un'accozzaglia di caratteri). L'unico modo per rompere il cifrario è indovinare la chiave.

La chiave
--------------------------

La chiave può essere qualsiasi numero intero che abbia meno cifre del messaggio stesso. Di solito vengono date 3 cifre valide (quindi ci sono 999 combinazioni). Ogni cifra in più aumenta la sicurezza. Affinché 2 parti possano comunicare, entrambe devono conoscere questa chiave segreta (quindi devono in qualche modo passarsela l'un l'altra in modo sicuro). Se la chiave è nota a loro e non all'altro, allora il messaggio può essere diffuso anche in modo insicuro e potenzialmente non compromettere il contenuto, perché il potenziale attaccante non conosce la procedura per recuperare il messaggio.

A scopo dimostrativo, userò la chiave **123**, normalmente si usa qualche altro numero casuale che si presume non sia così facile da indovinare.

Procedura di crittografia
--------------------------

Il principio si basa sull'idea di sostituire i caratteri di un messaggio con altri utilizzando una chiave. Io lo chiamo "spostamento di carattere".

Per esempio, abbiamo questo messaggio che vogliamo criptare:

```php
TAJNA ZPRAVA
```

Ora assegna un numero ad ogni personaggio. Tipicamente per alfabeto (il suo numero di serie). Userò l'alfabeto inglese, quindi questa serie di caratteri:

```php
A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
```

Se assegniamo un numero ad ogni carattere, otteniamo qualcosa come:

```php
20-1-10-14-1   26-16-18-1-22-1
```

Ora arriva la chiave. Prendiamo ogni singolo numero e gli aggiungiamo la chiave. Sto evidenziando a colori ciò che ho aggiunto dove:

`1 2 3`

```php
21 3 13 15 3   3 17 20 4 23 3
```

Notate che quando cripto il carattere **Z**, scrivo la cifra 3. Questo perché **Z** è l'ultimo carattere dell'alfabeto, quindi quando raggiunge la fine, conta di nuovo dall'inizio della riga.

Trasmettere il messaggio
--------------------------

Ora possiamo passare il messaggio come vogliamo. A volte anche pubblicamente. Altri vedranno solo una serie illogica di numeri, quindi probabilmente non sapranno nemmeno che si tratta di una specie di cifrario. La sicurezza è anche rafforzata dalla chiave stessa, che è segreta. A volte è opportuno trasmettere il messaggio come una serie di numeri, a volte è opportuno convertire questi numeri in caratteri (di nuovo, seguendo la stessa serie alfabetica) e poi trasmettere una sequenza di caratteri. Molto dipende dalle circostanze. In generale, però, la sequenza numerica è migliore, perché poche persone sospetteranno che si tratta di un messaggio codificato.

Decrittazione presso il destinatario
--------------------------

Il destinatario decifra con la stessa procedura. Prende ogni singolo carattere e sottrae i numeri secondo la chiave e poi converte i valori risultanti in caratteri usando l'alfabeto. È solo una procedura di crittografia inversa. L'importante è conoscere la **chiave** e il **set di caratteri** - cioè, come vanno i caratteri in sequenza.

Cracking e decrittazione
--------------------------

L'unica procedura possibile per il cracking è provare ogni possibile combinazione di tutte le chiavi potenziali. Se non conosciamo la lunghezza della chiave, l'intero processo diventa ancora più complicato. Ma in generale, i computer di oggi possono provare qualcosa come 100 chiavi in 1 secondo, quindi una chiave casuale di 3 caratteri richiede qualcosa come un minuto per essere decifrata.

Tuttavia, se la chiave è lunga quanto o più del messaggio originale, non può essere rotta perché ogni singolo carattere ha la sua chiave, quindi tutte le combinazioni devono essere provate per ogni carattere.

Se io avessi un messaggio "**Prendi un messaggio**", sarebbe lungo 11 caratteri (gli spazi non contano). Se volessi una chiave che fosse anche lunga 11 caratteri, e usassi una stringa di 26 caratteri dell'alfabeto inglese, allora ci sono **11^26** = 1.191817654*10²⁷ combinazioni, il computer medio potrebbe craccare quella chiave in 1.310999419×10²⁶ secondi = 10^20 giorni :)
