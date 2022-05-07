Perché e come usare framework e librerie
========================================

> id: '5ea6cdde-f67c-4f3f-8871-37b27ee8e23a'
> slug:
> 	cs: proc-pouzivat-frameworky
> 	it: perche-e-come-usare-framework-e-librerie
> 
> publicationDate: '2020-02-16 22:13:46'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '0926b7ec4f59904c008388431ff22eb5'

C'è una famosa barzelletta secondo cui i programmatori iniziano a usare i framework solo quando scrivono il proprio e scoprono che non va da nessuna parte. La cosa divertente è che è vero. L'ho sperimentato io stesso. Anche due volte.

Tuttavia, la pagina principale di <a href="https://nette.org">Nette</a> dice:

> I veri programmatori **non usano frameworks**. Scrivono applicazioni web tramite la linea di comando direttamente sul server. Questo è un omaggio a loro. Per il resto di noi, Nette renderà il nostro lavoro molto più facile e piacevole.

Il ruolo dei framework nello sviluppo di applicazioni
-----------------------------------

Sono sicuro che lo sai da solo:

Ti viene un'idea per un grande progetto, così inizi a scrivere codice greenfield direttamente in PHP puro... in poche ore scopri che gran parte del lavoro è ancora ripetitivo e stai risolvendo problemi di sistema di base. Come connettersi a un database, come rendere un modulo, come convalidare i dati, come inviare un'email, e molto altro.

Probabilmente scriverete le vostre funzioni da chiamare per questi compiti. Se state scrivendo diversi progetti contemporaneamente, inizierete a condividere queste funzioni tra i progetti in un unico grande file disordinato, e le modificherete continuamente quando ne avrete bisogno.

E quando le funzioni sono ad un certo stadio di sviluppo che si inizia a costruire un nuovo progetto sopra di esse ogni volta e sviluppare subito, allora si può parlare di aver scritto il proprio framework, o almeno una libreria.

Cos'è e cosa fa un quadro
-------------------------

Come abbiamo già mostrato con un esempio pratico dalla vita, un framework è una collezione di molte funzioni (ma idealmente classi e oggetti) che salvano il lavoro del programmatore. Quando sviluppa, non deve pensare a come connettersi a un database, ma sa che è già programmato da qualche parte e "funziona e basta".

I framework finiti sono quindi l'intelligenza raccolta da centinaia di persone che hanno sviluppato migliaia di applicazioni in un lungo periodo di tempo per debuggare una soluzione e un ecosistema per affrontare nuovi progetti.

Con i framework, si ottiene anche un insieme di **best practices**, che sono modi per risolvere i problemi senza doverci pensare. Se vi imbattete in un problema, qualcuno lo ha probabilmente risolto prima di voi, ed è sempre meglio trovare una soluzione nella documentazione che doverla risolvere da soli in modo complicato.

Programmazione astratta e principio di incapsulamento
---------------------------------------------

Framework ben progettati come Nette permettono di programmare ad un **livello molto alto di astrazione**.

In questo modo, il programmatore (l'utente del framework) non deve capire cosa sta succedendo internamente e esattamente come funzionano internamente i componenti, il che fa risparmiare molto tempo e fatica. Può concentrarsi sulla soluzione dei problemi reali della sua applicazione e ottenere risultati molto rapidamente.

David Grudl stesso (l'autore del framework Nette) una volta ha detto che ha progettato Nette per essere in grado di programmare un sito web per un cliente in una notte, quando torna a casa tardi dal pub e deve lanciare il sito web al mattino. Questo è dovuto principalmente al fatto che lo sviluppatore è completamente rimosso dalle cose tecniche.

Affinché questo funzioni e perché lo sviluppatore possa semplicemente usare il framework finito nel suo insieme, è molto importante usare correttamente il <a href="/encapsulation">principio di incapsulamento</a>.
