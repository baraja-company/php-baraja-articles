Obsolescenza del codice: come mantenere la compatibilità
========================================================

> id: '7837ee7e-a150-47bf-a338-2f4c03d5bbed'
> slug:
> 	cs: zastaravani-kodu
> 	it: obsolescenza-del-codice-come-mantenere-la-compatibilita
> 
> publicationDate: '2022-07-29 17:10:00'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '727b60c7258fb70e21b5acb445404db0'

Quando si sviluppano sistemi di grandi dimensioni (ad esempio, applicazioni aziendali, pacchetti software condivisi, librerie, ...) in cui più livelli e sviluppatori comunicano tra loro, si pone il problema di come gestire il rilascio di nuove versioni di codice.

Vediamo un esempio in cui vogliamo sviluppare un pacchetto Composer condiviso per una comunità di sviluppatori.

Versioni semantiche
--------------------

Prima di risolvere il problema della compatibilità in avanti e all'indietro, dobbiamo capire come tenere traccia delle modifiche apportate al software. Attualmente (2022), il modo migliore per versionare tutte le modifiche è Git. Il repository del software può essere condiviso, ad esempio, tramite GitHub o GitLab. Ogni modifica del software ha un identificatore unico che identifica ogni commit e descrive ciò che è effettivamente accaduto.

La seguente strategia ha funzionato bene per lo sviluppo delle librerie:

All'inizio dello sviluppo, viene creato un commit iniziale nel ramo `master` (o `main`), dove viene effettuato il commit della struttura di file sottostante.

Per ogni nuova richiesta, viene creato un ramo separato da master in cui lavorare. Quando la modifica è pronta, viene inviata una richiesta di unione al master sotto forma di `Richiesta di estrazione`. Viene eseguita una revisione del codice sulla richiesta e, se tutto è ok, la modifica viene unita al master.

Se il ramo contiene una modifica incompatibile all'indietro (BC break, da `Back Compatibility Break'), questa deve essere contrassegnata di conseguenza. Il metodo di marcatura delle interruzioni del BC è discusso nei capitoli successivi.

La versione di produzione della libreria viene quindi etichettata utilizzando tag con la seguente struttura (basata su **Semantic Versioning 2.0.0**):

Scriviamo il numero di versione nel formato `MAJOR.MINOR.PATCH`. L'incremento dei numeri di versione avviene come segue:

- `MAJOR` - quando c'è una modifica che non è retrocompatibile con altre (API)
- `MINOR` - quando la funzionalità viene aggiunta mantenendo la compatibilità con il passato.
- `PATCH` - quando viene risolto un bug e viene mantenuta la retrocompatibilità

Utilizzando le pre-release e aggiungendo metadati è possibile affinare le informazioni. Ad esempio: `1.0.0-alpha`, `1.0.1-beta+2`.

Per saperne di più sul versioning semantico si può consultare il sito ufficiale: https://semver.org.

Compatibilità con il passato e con il futuro
-------------------------------

Quando si progetta un software, bisogna sempre pensare alla **compatibilità all'indietro** (le nuove funzionalità e le modifiche devono essere compatibili con il vecchio codice) e, in alcuni casi, alla **compatibilità in avanti** (le funzionalità attuali devono essere compatibili con le future modifiche dell'interfaccia).

Riuscire a svolgere bene entrambi i compiti è molto impegnativo. Non sempre è possibile apportare una modifica senza interrompere la compatibilità.

Quando si apportano modifiche, si deve sempre procedere per gradi e lasciare agli utenti il tempo necessario per reagire alle modifiche.

Le sezioni seguenti descrivono come ragionare su questo aspetto.

Fase 1: contrassegnare una caratteristica come deprecata
--------------------------------------

Il tipo fondamentale di minaccia alla compatibilità è la rimozione o la ridenominazione di una funzionalità esistente in passato. Il più delle volte ciò avviene perché gli argomenti accettati dalla funzione sono cambiati, oppure perché si tratta di una vecchia logica che dovrebbe essere gestita in modo diverso nel nuovo modo.

Nella prima fase, le vecchie parti del codice dovrebbero essere contrassegnate come deprecate ma non modificate in alcun modo.

In PHP esiste un'annotazione `@deprecated` per questo, che dovrebbe essere scritta direttamente sopra i metodi, le funzioni, le proprietà, le variabili, le costanti e in generale tutto il codice deprecato.

È anche buona norma scrivere il motivo per cui una determinata cosa è deprecata e come sarà cambiata in futuro. Ad esempio, indicare il nome di una nuova funzione o di un metodo di utilizzo.

Un esempio reale di marcatura del codice obsoleto: Le costanti saranno rimosse, è meglio usare l'Enum integrato (interruzione del BC a causa della migrazione a una nuova versione di PHP):

```php
class OrderNotification
{
	/** @deprecato dal 2022-05-24, utilizzare enum OrderNotificationType */
	public const
		TYPE_EMAIL = 'e-mail',
		TYPE_SMS = 'testo';
```

L'annotazione `@deprecated` causerà solo un avviso silenzioso per l'IDE (strumento di sviluppo) e gli strumenti di compilazione. Non rompe nulla.

Fase 2: Chiamata di un nuovo metodo/logica
--------------------------------------

Nella seconda fase, sostituiamo la vecchia implementazione con la nuova, ma utilizziamo il nuovo metodo nella vecchia implementazione. Questo aiuterà a mantenere l'interfaccia compatibile senza che l'utente se ne accorga.

Esempio: il metodo è deprecato perché al suo posto è stato creato un nuovo servizio statico. Dal momento che qualcuno può usarlo, viene semplicemente contrassegnato come deprecato e richiama internamente la nuova implementazione. Lo sviluppatore può generalmente presumere che il metodo sarà completamente rimosso in futuro.

```php
/** @deprecato dal 2021-09-11 Utilizzare invece Ip::get(). */
public static function userIp(): string
{
	return Ip::get();
}
```

Fase 3: Modifica delle annotazioni per l'analisi statica
-------------------------------------------

Se si usa un'analisi statica come PhpStan (altamente raccomandata!), è una buona idea riscrivere le annotazioni PHPDoc prima di modificare effettivamente i tipi di dati. L'analisi statica segnalerà all'utente che qualcosa non funziona, ma il runtime non verrà toccato.

Fase 4: buttare via l'avviso
-----------------------

Nella quarta fase, viene chiamato un nuovo metodo e contemporaneamente viene lanciato un errore di livello `note`. L'applicazione continua a funzionare, ma inizia a memorizzare gradualmente nel registro di sistema l'informazione che una funzione è deprecata e sarà modificata o rimossa. D'ora in poi, avviseremo attivamente su questo tipo di modifiche. Lo sviluppatore vedrà degli errori durante lo sviluppo o la compilazione.

```php
/** @deprecato dal 2021-05-01, utilizzare invece UserMetaManager. */
public function getMeta(int $userId, string $key): ?string
{
	trigger_error(__METHOD__ . 'Questo metodo è deprecato, utilizzare invece UserMetaManager.');
	return $this->userMetaManager->get($userId, $key);
}
```

Fase 5: Lancio di un'eccezione
------------------------

Si consiglia di lanciare una delle eccezioni fatali prima di rimuovere completamente il metodo. Questo è particolarmente importante perché l'applicazione verrà completamente interrotta e l'errore non potrà essere ignorato. A differenza della rimozione completa del codice, l'utente viene informato di ciò che è effettivamente accaduto e può facilmente correggere l'errore.

Fase 6: rimozione completa del codice
-----------------------------

Nell'ultima fase, il vecchio codice verrà completamente rimosso. Se un utente non ha risolto le dipendenze, la sua applicazione sarà interrotta.

Le gravi rotture del BC in aree sensibili dovrebbero sempre essere effettuate nella release `MAJOR` successiva e dovrebbero essere segnalate almeno una release `MAJOR` prima, lanciando un avviso. Se non lo fate, l'aggiornamento della libreria sarà estremamente difficile.
