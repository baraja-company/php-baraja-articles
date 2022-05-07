Un giorno gli hacker attaccheranno il tuo sito web
==================================================

> id: a11ca0a6-77e1-4ba4-8eca-83449c9cbf48
> slug:
> 	cs: jednou-vam-hackeri-napadnou-web
> 	it: un-giorno-gli-hacker-attaccheranno-il-tuo-sito-web
> 
> publicationDate: '2020-08-04 12:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: a74c964823fb0c4231b711d113055a24

Non ti succederà? :DDDDDDDDD

Mentre gestisco più di 300 siti web, ho varie emergenze di tanto in tanto. A volte sono piuttosto accesi, ma spesso si tratta di una completa banalità. Se, come me, siete stati tentati dalla programmazione in passato, e sapete anche che [la programmazione è una rottura] (http://borisovo.cz/programming-sucks-cz.html), sarete sicuramente d'accordo con me.

Nella morsa degli hacker malvagi
----------------------

Quando un'applicazione web diventa popolare, diventa un obiettivo allettante per gli hacker. La loro motivazione di solito non è quella di far crollare l'intero sito web, al contrario. Infatti, **gli hacker vogliono che voi, come amministratore del server, siate completamente all'oscuro di loro**. Un buon hacker aspetta per mesi, osservando il server web e prendendo la cosa più preziosa - i vostri dati.

Per proteggere i vostri utenti, è imperativo criptare tutti i dati e avere più livelli di protezione. Per le password, usate una delle funzioni lente come [Bcrypt + salt](https://php.baraja.cz/hashovani), Argon2, PBKDF2 o Scrypt. Michal Špaček ha già scritto su [security rating](https://pulse.michalspacek.cz/passwords/storages/rating#slow-hashes), e lo ringrazio per aver aggiunto all'articolo. Come proprietario di un sito, devi insistere su un corretto hashing delle password quando discuti con un programmatore. Altrimenti, ti metterai a piangere, come molti prima di te che si sono anche illusi che la cosa non li riguardasse.

Riesci a capire quando sei sotto attacco?
---------------------------------

Ho notato che quando un server web raggiunge una certa soglia di traffico (nella Repubblica Ceca, è di circa 50+ visitatori al giorno), inizia a ricevere una serie di attacchi diretti ad esso dal nulla. Per rendere la cosa non facile, l'attaccante ha sempre un vantaggio, perché può scegliere quale parte dell'infrastruttura iniziare ad attaccare e tu - come proprietario del server web - devi riconoscerlo in tempo, difenderti ed essere meglio preparato per la prossima volta.

Quanti attacchi le sono stati rivolti nell'ultimo mese, per esempio? Lo sai esattamente? Quanti ne hai difesi? Qualcun altro ha accesso al tuo server?

E se qualcuno ti sta attaccando in questo momento? E ora... e ora anche...?

Sfortunatamente, non c'è un modo unico per riconoscere un attacco. Ma ci sono strumenti che possono darvi un vantaggio significativo.

Quello che ha funzionato meglio per me ultimamente:

- Quando un aggressore non sa dove sono fisicamente i vostri server**, ha una posizione molto più difficile. Le informazioni sull'architettura fisica possono essere offuscate molto bene con [Clouflare] (https://www.cloudflare.com/), che proxy (e crittografa bidirezionalmente) tutte le comunicazioni di rete. Ha anche una serie di altri vantaggi, come il recupero più veloce dall'estero, la protezione automatica contro gli attacchi DDOS, la compressione delle risorse e, ultimo ma non meno importante, il blocco automatico dei visitatori disonesti. Uso Cloudflare per tutti i miei siti web (e quasi ogni progetto usa diverse tecnologie).
- **Registrazione degli errori**. Sempre e tutto. È probabile che la vostra applicazione contenga molti bug commessi dal vostro programmatore. Che si tratti di [eccezioni non catturate] (https://php.baraja.cz/vyjimky), errori di applicazione, SQL injection, e così via. Se state programmando in PHP e non capite il logging, ci sono abbastanza problemi che [Tracy](https://tracy.nette.org/) (ed eventualmente altri strumenti come [Sentry](https://sentry.io/)) e i log di accesso sul server stesso cattureranno. La registrazione degli errori non è una cosa bella da avere, è un must. Registro i bug su tutti i siti che mantengo attivamente, e ho informazioni sui nuovi bug inviate alla mia e-mail in modo da saperlo immediatamente.
- **Piattaforma sicura e aggiornata**. Hai WordPress sul tuo sito? Sapete come aggiornarlo correttamente? Conoscete tutti i rischi che affrontate? Quando qualcosa è gratis, è quasi sempre a spese di qualcos'altro. Personalmente, uso [Nette framework](https://nette.org/cs/) versione 3 per lo sviluppo di applicazioni, che aggiorno settimanalmente e cerco attivamente le vulnerabilità di sicurezza da patchare. Proprio come fai la manutenzione della tua auto regolarmente, devi fare la manutenzione del tuo sito web regolarmente e almeno una volta al mese. La manutenzione del sito include l'aggiornamento di tutte le librerie esterne e il monitoraggio costante dei log. Se state usando una vecchia versione di una libreria, è molto probabile che un attaccante conosca la vulnerabilità e cerchi di sfruttarla.
La domanda è quando
Un gran numero di persone nella mia zona sottovaluta la sicurezza in modo incredibile ed espone su Internet contenuti molto facili da violare e sfruttare.

Infatti, anche le grandi aziende fanno errori, anche su cose banali come l'attacco [XSS](https://www.zive.cz/clanky/vyvojar-objevil-zranitelnost-v-seznamu-dokazal-mezi-vysledky-propasovat-zakazany-kod/sc-3-a-200023/default.aspx) (cross-site scripting).

La questione non è se qualcuno romperà mai il tuo sito web. La domanda è quando accadrà e se sarete abbastanza preparati per affrontarlo. Forse un hacker ha avuto accesso al tuo server per mesi e tu non lo sai (ancora).

Cosa fare quando ci sono problemi
-------------------------------

Quando si scopre il lavoro dell'hacker, di solito è troppo tardi e l'hacker ha fatto tutto quello che doveva fare. Molto probabilmente ha piazzato mallware su tutto il server e ha un controllo almeno parziale della macchina.

Questo è un **tipo di problema caotico e bisogna agire in fretta**.

Poiché questa è l'ultima fase dell'attacco, personalmente consiglio di scollegare completamente il server web da internet e iniziare a capire gradualmente cosa è successo. Inoltre, non sapremo mai esattamente se il server sta nascondendo qualcos'altro. L'attaccante ha sempre un vantaggio significativo.

Se decidiamo di rimettere l'ultimo backup funzionante sul server, aiuteremo molto l'attaccante. Sa già come introdursi nel vostro server e non c'è niente che gli impedisca di farlo di nuovo tra qualche ora. Inoltre, il backup probabilmente contiene mallware o qualche altro tipo di virus.

Anche se questa è un'opzione molto impopolare tra i miei clienti, personalmente consiglio sempre di **cancellare completamente i siti WordPress** prima, o qualsiasi sistema che il cliente non aggiorna e che ha molte minacce alla sicurezza. Per esempio, se ospitate 30 siti su un server che hanno più di 2 anni, è praticamente garantito che almeno uno di essi contiene una qualche forma di vulnerabilità.

Se non capisci il problema, è meglio che contatti presto uno specialista adatto che abbia una profonda conoscenza del tuo problema e capisca le cose in un ampio contesto.

**Nota etica:**

Se state gestendo un sito web per un cliente che ha avuto un incidente di sicurezza, è vostra responsabilità informarlo. Se qualche dato (come le password, ma spesso molto di più) è trapelato, dovete anche informare gli utenti finali che la loro password è di dominio pubblico e dovrebbero cambiarla ovunque. Se usi una funzione di hashing lenta (per esempio, **Bcrypt + sale**), renderai molto più difficile per un attaccante craccare le password. (Sfortunatamente, [Bcrypt passwords can be cracked](https://arstechnica.com/information-technology/2015/08/cracking-all-hacked-ashley-madison-passwords-could-take-a-lifetime/)). Se desideri ricevere informazioni regolari per sapere se il tuo account è stato violato, ti consiglio di registrarti con [Have i been pwned?](https://haveibeenpwned.com/). Per ulteriori informazioni su [violazione della sicurezza](https://m.uoou.cz/vismo/zobraz_dok.asp?id_ktg=5020&n=poruseni-zabezpeceni), visita il sito web di OOOO.

Abitudini atomiche
--------------

Negli ultimi sei mesi ho finito di leggere il libro [Atomic Habits] (https://www.melvil.cz/kniha-atomove-navyky/), che mi ha aperto gli occhi. Descrive un processo di miglioramento incrementale dell'1% ogni giorno che farà molto a lungo termine.

Dato che sono avvicinato da aziende e individui in varie fasi del debito tecnologico, vorrei riassumere le cose peggiori:

- **FTP per la comunicazione con il server non ha senso**. È un protocollo insicuro che è controllato da password. Se vuoi dare l'accesso a qualcuno, deve conoscere la password e può fare la stessa cosa che fai tu. Se vuoi togliergli l'accesso, non puoi, l'unico modo è cambiare la password. Meglio passare completamente a SSH e usare chiavi RSA. Spesso mi sorprende che anche le aziende risolvano questo problema al giorno d'oggi. Non fare mai una tabella di tutte le password. E certamente mai uno condiviso per tutta la squadra!
- Il **codice sorgente appartiene a Git**, i dati al database e il contenuto statico (immagini, PDF, ...) ai file su disco. Scegliere l'archiviazione dei dati sbagliata peggiora il design e la sicurezza dell'applicazione. L'URL di un PDF (per esempio con una fattura) dovrebbe contenere un contenuto generato casualmente che solo il destinatario conosce. Per contenuti estremamente sensibili (come un estratto conto bancario), è importante criptare il contenuto su disco e fornirlo solo dopo il login.
- Le parti non pubbliche del sito devono contenere l'intestazione META `noindex` e `nofollow` per evitare di essere indicizzate da Google. I contenuti sensibili che i vostri concorrenti non devono conoscere devono essere protetti da password.
- Disabilita [directory content listing](https://www.simplified.guide/apache/disable-directory-listing) sul tuo server. Se impostato in modo inappropriato, l'intero sito può essere scansionato dalla macchina per trovare file sensibili.
- Progetto radice! Non ci deve mai essere un URL diretto ai file di configurazione. Per esempio, [Nette does it right](https://doc.nette.org/cs/3.0/quickstart/getting-started#toc-obsah-web-projectu) fin dal primo tutorial. Lo stesso vale per [publicly available git repositories](https://smitka.me/open-git/). Questo tipo di vulnerabilità è estremamente facile da sottovalutare e le conseguenze tendono ad essere tragiche.
- Il bug può anche essere causato da una svista involontaria nello sviluppo. È molto importante fare una revisione del codice da parte di un umano dal vivo per ogni cambiamento. Molti bug sono scoperti da [PHPStan](https://github.com/phpstan/phpstan) o strumenti simili prima che un umano veda il codice.
- Mai [inviare password in forma leggibile via e-mail](https://www.lupa.cz/clanky/reset-a-poslani-hesla-v-citelne-podobe-e-mailem-nebezpecna-praktika/), c'è sempre un altro modo per gestirla.
- Problemi di sicurezza nelle vecchie versioni di librerie e software. I robot strisciano su internet ogni secondo e attaccano falle di sicurezza conosciute (SQL injection, XSS, CSRF, ...). Un sacco di buchi noti hanno vecchie versioni di WordPress.

Ci sono molte altre vulnerabilità e i problemi variano da progetto a progetto. Se hai bisogno di fare un rapido audit del server, ti consiglio di usare [Maldet](https://www.rfxn.com/projects/linux-malware-detect/) e poi di assumere uno specialista adatto che ti aiuti a fare un [audit completo del sito](https://baraja.cz/audit-webu), e non solo per ragioni di sicurezza.

Gli ingegneri della sicurezza sono come la plastica nell'oceano - solo per sempre. Abituatevi.

Il principio di azione e reazione della natura
-------------------------------

Avrete anche notato che la natura usa sempre il principio di reazione. Ciò significa che qualcosa accade in un certo ambiente e gli organismi circostanti reagiscono ad esso in modi diversi. Questo approccio ha molti vantaggi, ma un enorme svantaggio: sarete sempre lasciati indietro.

Come persona pensante (designer, sviluppatore, consulente) avete un grande vantaggio, ed è quello di essere perseguibili - cioè, conoscere in anticipo una certa parte dei grandi rischi e prevenire attivamente che accadano in primo luogo. Non si possono mai prevenire tutti gli errori, ma si possono almeno mitigare le conseguenze, o costruire meccanismi di controllo che rilevano i problemi in anticipo in modo da avere il tempo di reagire.

La maggior parte degli attacchi avviene in un lungo periodo di tempo, e se si fa il log, di solito si ha abbastanza tempo per individuare il problema.
