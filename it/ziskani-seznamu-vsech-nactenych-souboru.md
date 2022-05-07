Ottenere un elenco di tutti i file caricati
===========================================

> id: '72716cbc-90a6-4870-848b-125e8430707f'
> slug:
> 	cs: ziskani-seznamu-vsech-nactenych-souboru
> 	it: ottenere-un-elenco-di-tutti-i-file-caricati
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: f611e5d3-ed7b-4fe9-84ca-9271fc2bd2e3
> sourceContentHash: '7e896aa0e501d0fe33dc6595c1bfb43e'

Quando si esegue il debug di applicazioni più complesse, a volte succede che non so quali file sono stati caricati e se manca qualcosa.

Se usate Composer o qualsiasi altro tipo di <a href="/autoloading-trid">autoloading di classe</a>, probabilmente non conoscete questo problema. Tuttavia, può essere un evento relativamente comune quando si esegue il debug di vecchie applicazioni di altri sviluppatori.

Ottenere tutti i file caricati può essere fatto con la funzione `get_included_files()`, che li restituisce come un array di stringhe di percorso assoluto.

È comune nello sviluppo avere un numero enorme di file caricati (per esempio, anche questo blog relativamente semplice usa quasi 160 file). La maggior parte delle volte, però, il grande volume non ha importanza, perché il contenuto dei file viene recuperato da OPCache, che è ottimizzato per le letture di massa.
