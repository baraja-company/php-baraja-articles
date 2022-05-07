Forma pluriennale
=================

> id: '2abacddd-8a6b-4d25-a387-603fc7abc333'
> slug:
> 	cs: vicekrokovy-formular
> 	it: forma-pluriennale
> 
> publicationDate: '2019-09-16 09:30:19'
> mainCategoryId: a23332c0-a233-4093-abd7-85b1b00a383b
> sourceContentHash: '69cb85c856478d588b96e9db9e695d97'

A volte abbiamo bisogno di dividere il modulo in diverse parti (pagine), elaborarle separatamente e poi assemblarle in un unico risultato.

Questo articolo descrive metodi e design pattern per farlo.

> **Nota:**
>
> Il problema di dividere un modulo in più passi è molto complesso, specialmente se si vuole farlo bene. Ho incontrato molti approcci nel corso della mia vita, di cui parlerò qui. Alcuni approcci sembrano attraenti, ma sono ingenui e funzionano solo in casi specifici. Per ogni approccio, descrivo quando ha senso e quali approcci non hanno senso.

Progettazione di una soluzione teorica
-------------------------

Di solito l'obiettivo è quello di ottenere i dati di base dal primo modulo sulla prima pagina, convalidarli, poi salvarli "da qualche parte" e visualizzare la pagina successiva.

Quando l'utente arriva all'ultima pagina, l'intero modulo deve essere inviato e gli input elaborati.

Ad ogni passo, è importante convalidare sempre accuratamente tutti i dati e permettere all'utente di saltare indietro attraverso le pagine a piacimento in modo che possa correggere i dati quando incontra un errore. Inoltre, se il modulo deve essere reso condizionatamente in base ai dati già ottenuti, questo è un processo molto impegnativo.

Attuazione dei moduli stessi
--------------------------------

Possiamo implementare noi stessi i singoli moduli in puro HTML e poi gestire l'elaborazione in PHP, oppure usare soluzioni già pronte come <a href="https://doc.nette.org/cs/3.0/forms">Nette forms</a>.

> **Esempio dalla vita:**
>
> Molto spesso, i programmatori alle prime armi mi mandano un'email e fanno domande apparentemente semplici per le quali c'è una soluzione già pronta. Per esempio, specificamente sull'elaborazione dei moduli in PHP.
>
> Raccomando sempre di saltare del tutto l'elaborazione manuale e di usare invece una soluzione già pronta. In realtà, è molto complicato implementare correttamente, per esempio, la convalida dell'email inserita e che 2 password in 2 campi corrispondano, mentre vogliamo reindirizzare l'utente al modulo precompilato secondo i suoi dati e con messaggi di errore in caso di errore.
>
> Perché la gente **non sa di non sapere di non sapere** e così invece di investire 1 ora di tempo per imparare una soluzione già pronta per il 99,99% dei problemi, preferisce scegliere la propria soluzione, che passa decine di ore a fare il debug, e ci sono ancora casi in cui i moduli non funzionano, lanciano errori, hanno vulnerabilità di sicurezza e non proteggono i dati inseriti.

Quindi l'obiettivo di questo passo è quello di implementare diverse pagine a diversi URL che conterranno moduli vuoti.

Raccomando di implementare ogni modulo indipendentemente dagli altri (atomicamente) e di gestire il passaggio di stato su un diverso livello di applicazione. La ragione è che ogni modulo in pratica gestirà la validazione dei dati in modo diverso, scriverà i suoi output in modo diverso, gestirà gli errori in modo diverso, e probabilmente vorremo estenderlo o cambiarlo nel tempo, quindi non abbiamo bisogno di conoscere il contesto dell'intero processo e cambiare decine di siti per farlo.

Trasferimento di stato
---------------

Quando si elabora il primo modulo, vogliamo prima convalidare i dati ricevuti e se sono corretti, poi reindirizzare l'utente al secondo passo. È una buona idea gestire il reindirizzamento come un reindirizzamento HTTP, perché può facilmente accadere che i dati non siano validi, nel qual caso vogliamo riportare l'utente al primo modulo e non al passo successivo.

Possiamo fondamentalmente memorizzare gli stati in 4 modi:

**Non consigliato:**

- **Non memorizzare da nessuna parte** e trasmettere completamente nell'URL. Questo ha lo svantaggio che l'utente può cambiare una volta i dati già inviati nell'URL e quindi falsificare l'input. In alternativa, possiamo rivelare informazioni sensibili come le password nell'URL.
- **Aggiungi continuamente a <a href="/sessioni">sessioni</a>**, cioè inserisci in modo incrementale i nuovi dati acquisiti per chiave nel campo. Questo ha lo svantaggio che se l'applicazione fa un errore, l'utente è bloccato con le sessioni e non è in grado di risolvere l'errore in alcun modo (a parte cancellare i cookie, che è estremamente difficile per la maggior parte delle persone), inoltre con un modulo non finito c'è il rischio che i dati rimangano pre-popolati e possano essere visti da qualcun altro. Tuttavia, un caso molto peggiore si verifica se la sessione ha una validità molto breve (diciamo 5 minuti) e l'utente perde i dati del primo passo quando compila l'ultimo passo... questo può diventare abbastanza fastidioso allora.

**Raccomandato:**

- **Depositare nel database e passare l'identificatore**. La prima volta che il modulo viene inviato, memorizziamo tutti i dati raccolti in una tabella di database e generiamo un identificatore casuale (diciamo una stringa di 10 caratteri) da passare tra le pagine come parametro. Il vantaggio di questo è che quando si elabora qualsiasi modulo, possiamo scrivere i dati appena recuperati e convalidati direttamente nella tabella, e in caso di fallimento, abbiamo dei backup fisici dei moduli analizzati e possiamo agire su di essi. Per esempio, quando un ordine è incompleto, possiamo inviare un'email all'utente che non l'ha completato e aumentare la possibilità di una vendita.
- Il **Save to currently logged in account** funziona esattamente come l'inoltro via ID, tranne che invece di usare un identificatore casuale (token) usiamo una sessione all'ID dell'utente attualmente loggato (se ce n'è uno). Il vantaggio è che possiamo mostrare i dati pre-popolati all'utente arbitrariamente in futuro.

Conclusione
-----

Nessuna di queste soluzioni è perfetta o l'unica corretta. Io stesso combino più approcci quando lavoro su soluzioni a più fasi. Tipicamente, per esempio, risolvo un carrello della spesa come una tabella di database, alla quale assegno i dati che ho già raccolto e la lego o a un utente (se è loggato) o a una sessione (se non è loggato e non ci conosciamo ancora).
