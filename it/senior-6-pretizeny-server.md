Riparazione urgente del server sovraccarico
===========================================

> id: '07c5d1ed-f854-4797-8efe-350a24594762'
> slug:
> 	cs: senior-6-pretizeny-server
> 	it: riparazione-urgente-del-server-sovraccarico
> 
> publicationDate: '2023-02-11 14:25:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: f8239dd6b0eee234efc999c0dd3e0aeb

Uno strumento di monitoraggio esterno vi segnalerà che il tempo di risposta medio dei 5 URL monitorati è raddoppiato negli ultimi 30 minuti. Il progetto è in esecuzione su un singolo server fisico che non è sotto la vostra gestione ed è in esecuzione da qualche parte in un datacenter. Ci si collega via SSH, si avvia htop e si nota che il carico della CPU è del 95% e la memoria è da tempo sovrabbondante.

Secondo git, sapete che circa una settimana fa hanno fatto una migrazione del database a una nuova struttura di tabelle, e un collega scrive in chat che ha dovuto eseguire la migrazione durante la notte, perché il ricalcolo delle colonne e degli indici ha richiesto circa 5 ore, durante le quali quasi tutto il database era bloccato, e né INSERT né SELECT funzionavano.

Quindi i problemi di prestazioni sono probabilmente dovuti a indici progettati in modo improprio, a query SQL mal ridisegnate o a un grande pooling di connessioni. Non c'è tempo per un revert, ci sono 7 mila utenti sul sito secondo Google Analytics, e un'interruzione per 5 ore significherebbe un rischio reputazionale per il cliente, e una perdita di decine o centinaia di migliaia di corone in quel periodo (è difficile fare una stima, i proiezionisti ne fanno abbastanza). Ci si rende conto che testare solo la funzionalità in un ambiente di prova non è sufficiente e che è necessario implementare anche un test di carico.

Poiché si tratta di un importante negozio di e-commerce del vostro principale cliente e prevedete che la situazione possa peggiorare, avete 30 secondi per prendere una decisione.

Come si procede?

1. Non fate nulla. Il sito sarà più lento, ma finché il server è in grado di gestirlo, credo che vada bene...
2. Cercate di preparare un piano per ripristinare la struttura del DB e di eseguirlo il prima possibile.
3. Si migra il sito su un server più potente (ma questo significa aumentare rapidamente il prezzo al cliente, e poi aspettare forse 6 ore per completare la migrazione, perché si hanno centinaia di GB di tabelle di database).
4. Si scopre quali sono le query SQL e le pagine che richiedono più tempo e le si disabilita.
5. Si esegue Cloudflare e si lascia che faccia il check-in statico di ciò che può.
6. Qualche altra magia (non credo ci sia molto da inventare)...
