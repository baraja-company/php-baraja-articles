Come vive un programmatore in un team di sviluppo interno
=========================================================

> id: '9309b0f2-0265-43aa-a9c3-10b2d486cdfc'
> slug:
> 	cs: jak-zije-programator-v-internim-tymu
> 	it: come-vive-un-programmatore-in-un-team-di-sviluppo-interno
> 
> publicationDate: '2022-01-10 12:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: e65dd78fa9813b552f30b98f04aacb30

C'è un prezzo pesante da pagare per l'onestà.

Questo sito è sempre stato una descrizione della realtà che le persone nell'IT sperimentano, quindi vorrei esaminare la mia esperienza di lavoro nei team di sviluppo. Le seguenti sono esperienze generali che ho avuto tra le varie aziende. Nessuna esperienza è associata a una particolare azienda e non serve necessariamente come critica.

Le aziende tendono a non volere persone laboriose e proattive
----------------------------------------------

Hai molte idee? Vuoi innovare? Ti piace trovare soluzioni eleganti ai problemi complessi che il tuo team sta risolvendo e che affliggono metà dell'azienda? Ti rendi conto dell'importanza della sicurezza, del design del software e di trovare i colli di bottiglia dei progetti?

Probabilmente sarai infelice nel team di sviluppo per molto tempo.

Il lavoro di squadra è qualcosa con cui ho lottato molto ultimamente - voglio dire, ci sto nuotando specificamente e sto cercando di capire i principi completamente incomprensibili per me a cui aderire.

- Lavoro di squadra significa sviluppare la soluzione che tutta la squadra capisce. Spesso non è il migliore. Non è quasi mai elegante. Infatti, è sempre inefficiente. Ma ha il bel vantaggio che tutta la squadra lo capisce e può essere gestito a lungo termine. Questa è un'idea estremamente importante. Perché con i grandi progetti, non c'è molta pressione per essere efficienti, perché come azienda puoi scalare attraverso le persone e hai abbastanza soldi. È molto più importante consegnare le cose in tempo, anche se parzialmente rotto e con vincoli sconosciuti in anticipo, ma in tempo. Ha a che fare con l'idea che la soluzione che consegni domani (anche se imperfetta) ha molto più valore per il cliente di una soluzione debuggata al 100% che non ci sarà per un anno.
- Innovare e spingere le cose fuori è piuttosto sbagliato. Ha a che fare con il fatto che ci sono persone reali che lavorano in aziende che hanno famiglia e vogliono lavorare solo durante le ore di lavoro. Ne consegue necessariamente che le persone hanno un tempo limitato per imparare e provare cose nuove. Essenzialmente, le aziende devono stipare nelle ore di lavoro delle persone l'istruzione e lo sviluppo che possono essere utilizzati per il lavoro futuro. Una conseguenza interessante è quindi il rinvio delle decisioni e dell'apprendimento di nuove cose al tempo necessario. Da parte mia, capisco questo approccio e ha molto senso per me. Se avessi dei dipendenti, preferirei anche persone tranquille che dureranno nell'azienda per, diciamo, 15 anni, a persone che hanno una grande visione d'insieme e vogliono andare avanti, ma sarà a spese della squadra. È vero che la soluzione collettiva non sarà mai la stessa di quella individuale, ma questo non significa che sarà peggiore.

Le persone come me sono una potenziale fonte di problemi e conflitti. È bello pensarla in questo modo.

Quando fai il codereview, cerca solo gli errori oggettivi
----------------------------------------

Ogni azienda ha le sue regole di codificazione. Purtroppo. All'inizio mi infastidiva così tanto.

Quando si usa uno dei linguaggi più maturi, come .NET, le regole di stile di codice tendono ad essere più simili. È solo allora che si scopre che ci sono ancora PHP e JavaScript nel mondo. Specialmente la cosa del PHP è un po' frustrante a volte. PHP è un linguaggio molto maturo con molte grandi caratteristiche, ma nella mia esperienza, è usato nelle aziende a circa un terzo del suo potenziale totale. Le ragioni tendono a variare, più spesso l'inerzia.

A questo proposito, ho cercato a lungo una soluzione procedurale per la revisione del codice. Ho giocato a lungo con PhpStan e ho seguito le idee intorno ad esso. L'idea di base di PhpStan è che evidenzia solo gli errori oggettivi che sono certi di verificarsi ad un certo punto. Più esploro PhpStan e lo porto al livello successivo, più convalido internamente quanto sia vero.

Sarebbe bello se PhpStan fosse implementato in almeno la metà delle aziende ceche. Sarebbe ancora meglio se almeno al livello 6. In pratica, ho visto le migliori aziende avere il livello 4 o 5.

Proprio l'altro giorno mi chiedevo se può esistere un'applicazione di produzione in esecuzione reale che ha il suo team di sviluppo, e non passa nemmeno il livello 0 - che è il livello che controlla le cose che ci si aspetta in qualsiasi applicazione. Qualcosa del tipo assicurarsi che tutte le classi e le funzioni esistano, che i file non abbiano errori di parsing, e così via. E sì, ci sono queste applicazioni. Ma la situazione sta migliorando nel lungo periodo. Speriamo.

Quindi seguo un approccio simile con codereview. Io riporto solo cose che sono oggettivamente sempre sbagliate. Spesso mi imbatto in un errore di valutazione del grado - qualcosa non va, ma di nuovo, non è un problema così grande da dover essere affrontato. Molti problemi vengono risolti per convenzione, o dichiarando che il rischio è piccolo, quindi non ha senso trattarlo.

Ho sempre considerato i tipi di dati mancanti (anche quelli composti) come un bug critico. Poi ho capito che c'è il test e il dump.

Non credo di avere una conclusione concreta per questa parte. Ho solo un sacco di commenti soggettivi su di esso, che è più probabile che siano fonte di malintesi reciproci, quindi non li posterò. A lungo termine, sono incline alla conclusione che non posso fare la codereview - o sono troppo severo o troppo benevolo. Non riesco a capire a livello generale cosa sia davvero importante e cosa no. Sarei molto interessato a sapere come lo fanno gli altri. Nessuno è stato ancora in grado di darmi una risposta che possa servire anche a me.

Non ci sono soldi nello sviluppo personalizzato.
---------------------------------

Hai un'agenzia e fai sviluppo web personalizzato? Se non sei abbastanza grande e non fai grandi progetti per altre società, probabilmente un giorno non esisterai.

La ragione per cui questo accade è a causa degli scarsi finanziamenti per i progetti personalizzati. Probabilmente ha a che fare con la natura dei contratti, che sono più redditizi per gli acquirenti. Infatti, la maggior parte delle agenzie sono brutalmente sottofinanziate e fanno solo un vero profitto per i proprietari.

Quando si sviluppa internamente un progetto come azienda per se stessi, un ritardo nella consegna di un mese probabilmente non ha molta importanza. Come agenzia, se non consegni un lavoro entro un mese, hai la garanzia di essere addormentato lentamente al lavoro quel mese, rovinandoti costantemente la salute, il cliente che impreca nel telefono e nei biglietti, chiedendo sempre se può essere più veloce, e come ricompensa pagherai comunque una penale contrattuale. E se non lo fai, il cliente ti etichetterà come un appaltatore inaffidabile e potrebbe considerare di andare altrove, dove non sarà comunque meglio.

Ma queste sono le regole del gioco, che ho imparato a conoscere nella pratica, e che hanno fatto un buco nel mio bilancio.

Il contracting è probabilmente la forma più complessa di business nell'IT. Non vuoi fare contratti. La contrattazione vi distruggerà. Ti chiameranno nel fine settimana, di notte, a Natale. La metà del lavoro che fai non viene pagata. Ti scuserai per errori che non puoi commettere. Guarderai su Instagram lo storytelling degli amici che sono andati in vacanza per la terza volta quest'anno mentre tu sei seduto nel tuo ufficio alle 3 del mattino di sabato a finire un progetto di un cliente per il quale verrai comunque rimproverato lunedì perché il contratto prevedeva più di quanto tu potessi fare. La gente prenderà ferie e giorni di malattia e tu lavorerai al loro posto. O si viene licenziati. E poi sarai felice di pagare l'affitto.

Ma d'altra parte, si impara molto. Perché essere sotto pressione ti mette molta pressione per imparare ed essere efficiente in modo che la prossima volta tu possa fare un po' di più nella stessa quantità di tempo. La cosa brutta è che non si può davvero applicare quella conoscenza ed esperienza altrove perché le aziende non sono interessate (ho spiegato sopra).

Per fortuna, questa è una storia del 2019 ed è molto lontana.

Non ci sarà mai un mondo ideale
-------------------------

Se sto facendo ciò che mi piace? E tu? :D

Credo che sia tutta una questione di proporzioni. Probabilmente non farete mai un lavoro che sia appagante al 100%. Probabilmente avrai sempre qualcuno che ti spiegherà che non puoi fare la cosa che hai imparato a forza per 10 anni e che internamente sai che puoi.

D'altra parte, per una certa dose di negazione della propria personalità, si guadagna lo spazio per avere qualcosa di più. Come il tempo del fine settimana, una salute migliore, più tempo per se stessi, un ambiente stabile, ecc. Che non possa essere sostenuto a lungo termine è anche ovvio.

Mi piace poter provare cose diverse per sperimentare in prima persona come ce l'hanno gli altri. Lavorare in un team di sviluppo è una noia enorme rispetto a un lavoro personalizzato.

Non si tratta più di trovare la soluzione migliore e più veloce, ma di collaborare. Si tratta di migliorare la comunicazione, le emozioni e, soprattutto, di imparare ad essere umani. E anche questo è un valore enorme.
