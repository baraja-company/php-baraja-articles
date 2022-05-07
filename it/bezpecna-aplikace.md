Applicazione sicura
===================

> id: cc4667a3-aea3-4e0a-b323-da31ab78e019
> slug:
> 	cs: bezpecna-aplikace
> 	it: applicazione-sicura
> 
> publicationDate: '2019-10-01 14:19:04'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '74e52382b995b303550d8e4b783bbb84'

Se state seriamente sviluppando applicazioni web e il sito sarà in seguito disponibile su Internet, è molto importante affrontare la sicurezza.

Realisticamente, le seguenti minacce attendono gli sviluppatori:

- **L'applicazione ha un errore interno**, per esempio, perché il programmatore ha fatto un errore che non ha notato quando ha scritto il codice, o si manifesta solo "a volte"
- **Il server ha una configurazione errata** o l'ambiente **è cambiato** perché l'amministratore del server ha cambiato il comportamento del server e i siti non sono stati adattati per questo. In alternativa, stiamo distribuendo su una nuova macchina e non conosciamo la configurazione esatta.
- Qualcuno sta cercando di attaccare**, o un attaccante esterno o un ex dipendente dall'interno del sistema.

Tutte le situazioni sono estremamente sgradevoli e richiedono un'azione immediata. Progettare un'applicazione in modo che non fallisca mai non è tecnicamente possibile.

Durante lo sviluppo, è molto importante testare il codice una volta che è stato scritto, e se si verifica un bug sul server, è importante informare immediatamente lo sviluppatore con una descrizione accurata del problema.

Per individuare i bug e monitorare la salute del server, raccomando lo strumento <a href="https://tracy.nette.org/">Tracy</a>, che registra tutti gli errori fatali, le eccezioni non gestite e altro su file direttamente sul disco del server. Inoltre, è possibile impostare un'email dove verranno inviate le notifiche.

Sei interessato a una consulenza sulla sicurezza per la tua applicazione?

Contattatemi.
