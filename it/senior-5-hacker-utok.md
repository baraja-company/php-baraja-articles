Attacco hacker all'agenzia
==========================

> id: '7d5edd94-cce4-4a32-9ec5-51260dcd1653'
> slug:
> 	cs: senior-5-utok-hackeru-na-agenturu
> 	it: attacco-hacker-all-agenzia
> 
> publicationDate: '2023-02-11 14:20:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: '18511f7b3ff80124720244a93e140932'

Una storia del 2017: lavorate come sviluppatore principale in un'agenzia e gestite circa 300 progetti di varie dimensioni che l'azienda ha sviluppato in quel periodo. La maggior parte di esse sono semplici applicazioni Nette con un massimo di 10 modelli, alcuni moduli e tabelle di database. Niente di speciale. Non si sa molto dei progetti, perché ognuno di essi è stato sviluppato da un fornitore leggermente diverso, le persone entrano ed escono dall'azienda e si spera di portarli a termine in qualche modo. Poiché l'azienda si occupa molto di ottimizzazione dei costi, ospita circa 50 progetti alla volta su un server.

Improvvisamente, un project manager corre da voi dicendo che uno dei siti importanti del cliente ha smesso di funzionare. Invece del sito vero e proprio, si ottiene una schermata nera e una serie di testi che dicono che il sito è stato violato e ha accesso all'intero server.

Ancora una volta, non sapete (e non potete sapere) molto dell'architettura e del contenuto dei progetti, e molti progetti funzionano solo sul server. Il proiezionista vi spinge a credere che il sito debba funzionare, ma non conoscete la portata dell'attacco e dove sia andato l'aggressore, o se ci siano backdoor dal passato.

Come si decide?

1. Non fate nulla e cercate prima di tutto di analizzare l'incidente.
2. Il server viene messo offline. Non si conosce l'entità del danno e l'aggressore può continuare a penetrare nel server e a rubare dati. Allo stesso tempo, ciò significa che molti siti non sono disponibili.
3. Eseguite un ripristino di un sito specifico da un backup di un giorno fa e nel frattempo vi occupate di ciò che è successo. Ma l'aggressore può riottenere l'accesso e continuare a rubare i dati.
4. Un'altra soluzione...
