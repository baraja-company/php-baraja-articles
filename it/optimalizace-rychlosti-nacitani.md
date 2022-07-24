Come ottimizzare efficacemente la velocità di caricamento delle pagine
======================================================================

> id: '72f1d564-cb70-431c-a3dd-a3fdcaf14292'
> slug:
> 	- null
> 	it: come-ottimizzare-efficacemente-la-velocita-di-caricamento-delle-pagine
> 
> cs: optimalizace-rychlosti-nacitani
> publicationDate: '2021-10-15 15:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '013c8f29c213d3f3f1df3fbe4be8d572'

Quando viaggio, mi capita spesso di incontrare connessioni Internet scadenti e questo, in quanto web designer, mi fa pensare ai principi di progettazione per risolvere il problema della velocità del web anche con una connessione scadente.

La pratica mi ha insegnato alcuni trucchi utili:

Le landing page importanti sono spesso semplici e l'utilizzo di uno stile CSS personalizzato è vantaggioso.
-----------------------------------------------------------------------------------

Ad esempio, la pagina di accesso al CMS è spesso molto semplice. Un semplice modulo ha davvero bisogno di scaricare l'intero Bootstrap e altri framework CSS/JS? Vale la pena ottimizzare le pagine importanti e scrivere codice nativo.

Una volta che la pagina è stata completamente renderizzata, abbiamo alcuni secondi in cui l'utente compila i suoi dati e possiamo scaricare e memorizzare nella cache gli stili rimanenti nel browser in background. Quando l'utente accede, probabilmente avrà già scaricato Bootstrap (e se è su Edge, non lo farà...).

Al posto delle icone, spesso si possono usare le emoji
-----------------------------------

Davvero! Le emoji hanno molti vantaggi pratici:

- Occupa solo 4 byte, quindi ha la meglio su qualsiasi immagine.
- Non manca mai il download, quindi l'utente vede sempre almeno un'icona anziché uno spazio vuoto.
- Visualizzazione nello stile visivo dell'utente
- Attualmente dispone già di un ampio supporto per i dispositivi
- Si risparmia la richiesta al server e non ci si deve occupare di dove reperire l'immagine (questo può essere ottimizzato tramite CDN, ma perché, no).
- Molte icone con cui probabilmente non volete avere a che fare. Ad esempio, dove trovare le icone delle bandiere di tutti i Paesi che si desidera visualizzare nell'amministrazione? Utilizzare l'emoji: https://github.com/bara.../country/blob/main/flag-emoji.json

Utilizzare gli stili critici direttamente nell'HTML durante il caricamento del sito.
------------------------------------------------------

A volte inserisco gli stili CSS importanti che definiscono il layout della pagina e il layout di base direttamente nell'HTML. Ho posto un limite di 1 KB, che cerco di inserire il più possibile. Lo svantaggio di questo approccio è che gli stili devono essere trasferiti in ogni richiesta e non possono essere memorizzati nella cache; d'altra parte, si scaricano in modo incomparabilmente più veloce di un'immagine.

Non sono un esperto di velocità di caricamento, [Martin Michalek](https://www.programia.cz/rozhovor-martin-michalek-rychlost-webu/) saprebbe dirvi di più.

Usate ajax!
--------------

Uso sempre ajax per recuperare contenuti poco importanti o lenti. Aggiunge un po' ai requisiti tecnologici dell'applicazione, ma posso permettermelo.

Un esempio di buon utilizzo di ajax è quasi tutto ciò che riguarda l'amministrazione. Molto spesso ci sono molti dati da recuperare, ma l'utente non è interessato a tutto. Quando l'utente ha solo un client javascript scaricato e tutti i dati vengono recuperati tramite Vue e json, viene scaricata solo la quantità minima di dati e le risposte sono istantanee.

Come fare in Vue: https://vue.baraja.cz/api-a-ajax-ve-vue-js

Nel backend, utilizzo la libreria per Nette: https://github.com/baraja-core/structured-api

Utilizzare un CDN adeguato
---------------------

Per la distribuzione di contenuti statici, consiglio di utilizzare un CDN per tutti i tipi di siti. Se non sapete come impostare un CDN, utilizzate almeno Cloudflare in modalità proxy. Il contenuto statico viene memorizzato automaticamente nella cache in base alle intestazioni HTTP. Non tutti i provider di hosting hanno impostato bene Pops e per i percorsi lunghi questo vi farà risparmiare centinaia di millisecondi per ogni richiesta.

Formato immagine adatto
---------------------

Uno dei miei junior ha recentemente inserito un'immagine PNG nella pagina principale del sito, che conteneva una foto e occupava 3 MB. Ha detto che era tutto a posto perché la pagina si caricava velocemente sulla sua connessione (lo fa anche con l'ottica di casa sua, non è vero...), inoltre ha detto che oggigiorno trasferiamo molti dati, come i video.

Sono all'antica e ottimizzo quello che posso.

Foto in JPG o meglio in WEBP. Ma salvare un'immagine in JPG non significa nulla, è necessario passarla attraverso un servizio di ottimizzazione: https://tinyjpg.com

Se avete immagini in Git, questo strumento può comprimerle automaticamente e inviare una richiesta di pull: https://imgbot.net. Quando viene aggiunta una nuova immagine, viene inviata nuovamente la PR.

Se è necessario comprimere migliaia di immagini (come un'intera galleria di prodotti su un sito), utilizzo un'applicazione desktop per Mac: https://imageoptim.com/mac.

La compressione delle immagini in modo appropriato di solito consente di risparmiare la maggior parte dei dati. A volte anche il 50%.
