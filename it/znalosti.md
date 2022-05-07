Panoramica della conoscenza dello sviluppo web
==============================================

> id: e5e9613c-66fe-4d5e-a686-a182aab8061a
> slug:
> 	cs: znalosti
> 	it: panoramica-della-conoscenza-dello-sviluppo-web
> 
> publicationDate: '2019-09-11 10:07:07'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: b7284b1fc11b40ddb7628995070198a1

Sapere come creare un sito web e poi prendersene cura in modo completo non è solo una questione di realizzazione. Ci sono molti ostacoli lungo la strada ed è bene avere almeno un'idea di base di ogni cosa. Quando ho iniziato, non sapevo bene cosa imparare. Questa pagina serve da guida attraverso gli argomenti che ho dovuto studiare gradualmente per essere in grado di capire lo sviluppo web e affrontare le situazioni più comuni.

Amministrazione del server
--------------

> Un server web è un computer che gestisce il web. Quando un utente visualizza una pagina, il server web invia la pagina all'utente.
>
> Al momento (2021), non ha più senso ottenere un hosting gratuito se si è seri sul web. Un server può essere **affittato a partire da 50 corone al mese**.

- Installazione del server (differisce su Windows e Linux)
- Configurazione del server
	- Impostazioni Apache
	- Impostazione PHP
	- <a href="/info">ottenere la configurazione PHP corrente</a> (funzione `phpinfo()`)
	- Ngnix routing (migliora le prestazioni del server)

Internet e browser web
--------------------------------

- Browser web
- Principio di richiesta e risposta
	- Indirizzo URL
	- Ajax e Ajaj
	- Generazione di codice HTML (sistemi di template)

Corde
-----------------

- Lettura, scrittura e concatenazione di stringhe, in particolare lavoro di base sulle stringhe
- Elaborazione delle stringhe
	- Navigare carattere per carattere
	- <a href="/if">confrontare le stringhe</a>
	- Somiglianza delle stringhe (funzioni `levenshtein()`, `similar_text()` e `soundex()`)
	- <a href="/explode">Explode</a>, dividendo per separatore
	- Le **espressioni regolari** dividono le stringhe secondo una maschera universalmente configurabile
	- **Tokenizer** rompe le stringhe in piccole parti (token)
- Serializzazione dei dati in una stringa
	- <a href="/json">Json</a>, un oggetto javascript memorizzato in una stringa
