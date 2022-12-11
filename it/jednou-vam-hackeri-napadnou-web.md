Un giorno gli hacker attaccheranno il vostro sito web
=====================================================

> id: a11ca0a6-77e1-4ba4-8eca-83449c9cbf48
> slug:
> 	cs: jednou-vam-hackeri-napadnou-web
> 	it: un-giorno-gli-hacker-attaccheranno-il-vostro-sito-web
> 
> publicationDate: '2020-08-04 12:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '0f0ac4c091115e9c1f0833d318f34f5d'

Non ti succederà? :DDDDDDD

Gestendo più di 300 siti web, di tanto in tanto mi si presentano varie emergenze. A volte sono piuttosto accesi, ma spesso si tratta di una banalità assoluta. Se, come me, siete stati tentati dalla programmazione in passato e sapete anche che [la programmazione è una sofferenza](http://borisovo.cz/programming-sucks-cz.html), sarete sicuramente d'accordo con me.

Nella morsa degli hacker malvagi
----------------------

Quando un'applicazione web diventa popolare, diventa un bersaglio allettante per gli hacker. Di solito la loro motivazione non è quella di far crollare l'intero sito web, al contrario. In effetti, **gli hacker vogliono che voi, in qualità di amministratori del server, ne siate completamente all'oscuro**. Un buon hacker aspetta per mesi, sorvegliando il server web e ottenendo la cosa più preziosa: i vostri dati.

Per proteggere i vostri utenti, è indispensabile criptare tutti i dati e disporre di più livelli di protezione. Per le password, utilizzare una delle funzioni lente come [Bcrypt + salt](https://php.baraja.cz/hashovani), Argon2, PBKDF2 o Scrypt. Michal Spacek ha già scritto su [security rating](https://pulse.michalspacek.cz/passwords/storages/rating#slow-hashes), e lo ringrazio per aver arricchito l'articolo. Il proprietario di un sito deve insistere sull'hashing corretto delle password quando discute con un programmatore. Altrimenti, vi metterete a piangere, proprio come molti altri prima di voi che si sono illusi che la cosa non li riguardasse.

Riesci a capire quando sei sotto attacco?
---------------------------------

Ho notato che quando un server Web raggiunge una certa soglia di traffico (nella Repubblica Ceca, si tratta di circa 50+ visitatori al giorno), inizia a ricevere una serie di attacchi dal nulla. Per non rendere le cose facili, l'attaccante ha sempre un vantaggio, perché può scegliere quale parte dell'infrastruttura iniziare ad attaccare e voi - in quanto proprietari del server web - dovete riconoscerlo in tempo, difendervi e prepararvi al meglio per la prossima volta.

Quanti attacchi vi sono stati rivolti nell'ultimo mese, ad esempio? Lo sai esattamente? Quanti ne ha difesi? Qualcun altro ha accesso al vostro server?

E se qualcuno vi stesse attaccando in questo momento? E ora... e ora anche...?

Purtroppo non esiste un modo univoco per riconoscere un attacco. Ma ci sono strumenti che possono darvi un vantaggio significativo.

Quello che ha funzionato meglio per me ultimamente:

- Quando un aggressore non sa dove si trovano fisicamente i vostri server**, ha una posizione molto più difficile. Le informazioni sull'architettura fisica possono essere offuscate molto bene con [Cloudflare](https://www.cloudflare.com/), che proxy (e cripta bidirezionalmente) tutte le comunicazioni di rete. Presenta inoltre una serie di altri vantaggi, come il recupero più rapido dall'estero, la protezione automatica contro gli attacchi DDOS, la compressione delle risorse e, ultimo ma non meno importante, il blocco automatico dei visitatori disonesti. Utilizzo Cloudflare per tutti i miei siti web (e quasi tutti i progetti utilizzano tecnologie diverse).
- **Registrazione degli errori**. Sempre e comunque. È probabile che la vostra applicazione contenga molti bug commessi dal vostro programmatore. Che si tratti di [eccezioni non catturate] (https://php.baraja.cz/vyjimky), errori di applicazione, SQL injection e così via. Se si programma in PHP e non si capisce il logging, ci sono abbastanza problemi che [Tracy](https://tracy.nette.org/) (e possibilmente altri strumenti come [Sentry](https://sentry.io/)) e i log di accesso al server stesso possono individuare. La registrazione degli errori non è una cosa bella da avere, è un must. Registro i bug su tutti i siti di cui mi occupo attivamente, e mi faccio inviare le informazioni sui nuovi bug per posta elettronica, in modo da venirne a conoscenza immediatamente.
- **Piattaforma sicura e aggiornata**. Avete WordPress sul vostro sito? Sapete come aggiornarlo correttamente? Conoscete tutti i rischi a cui andate incontro? Quando qualcosa è gratuito, è quasi sempre a spese di qualcos'altro. Personalmente, per lo sviluppo di applicazioni utilizzo [Nette framework](https://nette.org/cs/) versione 3, che aggiorno settimanalmente e cerco attivamente le vulnerabilità di sicurezza da correggere. Come per la manutenzione regolare della vostra auto, anche per il vostro sito web è necessaria una manutenzione regolare, almeno una volta al mese. La manutenzione del sito comprende l'aggiornamento di tutte le librerie esterne e il monitoraggio costante dei log. Se si utilizza una vecchia versione di una libreria, è molto probabile che un utente malintenzionato sia a conoscenza della vulnerabilità e cerchi di sfruttarla.
La domanda è quando
Un gran numero di persone nella mia zona sottovaluta in modo incredibile la sicurezza ed espone a Internet contenuti molto facili da violare e sfruttare.

In effetti, anche le grandi aziende commettono errori, anche su cose banali come l'attacco [XSS](https://www.zive.cz/clanky/vyvojar-objevil-zranitelnost-v-seznamu-dokazal-mezi-vysledky-propasovat-zakazany-kod/sc-3-a-200023/default.aspx) (cross-site scripting).

La questione non è se qualcuno violerà mai il vostro sito web. Il problema è quando accadrà e se sarete abbastanza preparati per affrontarlo. Forse un hacker ha avuto accesso al vostro server per mesi e voi non lo sapete (ancora).

Cosa fare in caso di problemi
-------------------------------

Quando si scopre il lavoro dell'hacker, di solito è troppo tardi e l'hacker ha fatto tutto quello che doveva fare. È molto probabile che abbia piazzato del malware su tutto il server e che abbia un controllo almeno parziale della macchina.

Si tratta di un **problema di tipo caotico e bisogna agire in fretta**.

Poiché questa è l'ultima fase dell'attacco, personalmente consiglio di scollegare completamente il server Web da Internet e iniziare a capire gradualmente cosa è successo. Inoltre, non sapremo mai esattamente se il server nasconde qualcos'altro. L'attaccante ha sempre un vantaggio significativo.

Se decidiamo di rimettere l'ultimo backup funzionante sul server, aiuteremo molto l'attaccante. Sa già come entrare nel vostro server e non c'è nulla che gli impedisca di farlo di nuovo tra qualche ora. Inoltre, il backup probabilmente contiene malware o qualche altro tipo di virus.

Sebbene sia un'opzione molto impopolare tra i miei clienti, personalmente consiglio sempre di **eliminare completamente i siti WordPress** o qualsiasi sistema che il cliente non aggiorna e che presenta molte minacce alla sicurezza. Ad esempio, se ospitate 30 siti su un server che hanno più di 2 anni, è praticamente garantito che almeno uno di essi contenga una qualche forma di vulnerabilità.

Se non capite il problema, è meglio che contattiate subito uno specialista adatto, che abbia una conoscenza approfondita del vostro problema e che capisca le cose in un contesto ampio.

**Nota etica

Se gestite un sito web per un cliente che ha avuto un incidente di sicurezza, è vostra responsabilità informarlo. In caso di fuga di dati (come le password, ma spesso anche molto di più), è necessario informare gli utenti finali che la loro password è di dominio pubblico e che devono cambiarla ovunque. Se utilizzate una funzione di hashing lenta (ad esempio, **Bcrypt + sale**), renderete molto più difficile per un aggressore decifrare le password (purtroppo, [Le password di Bcrypt possono essere decifrate](https://arstechnica.com/information-technology/2015/08/cracking-all-hacked-ashley-madison-passwords-could-take-a-lifetime/)). Se desiderate ricevere regolarmente informazioni sull'eventuale fuga del vostro account, vi consiglio di registrarvi su [Have i been pwned?](https://haveibeenpwned.com/). Per ulteriori informazioni su [violazione della sicurezza](https://m.uoou.cz/vismo/zobraz_dok.asp?id_ktg=5020&n=poruseni-zabezpeceni), visitare il sito Web di OOOO.

Abitudini atomiche
--------------

Negli ultimi sei mesi ho finito di leggere il libro [Atomic Habits] (https://www.melvil.cz/kniha-atomove-navyky/), che mi ha aperto gli occhi. Descrive un processo di miglioramento incrementale dell'1% ogni giorno, che a lungo termine farà molto.

Poiché vengo contattato da aziende e individui in varie fasi di indebitamento tecnologico, vorrei riassumere le cose peggiori:

- **FTP per la comunicazione con il server non ha senso**. Si tratta di un protocollo insicuro controllato da password. Se volete dare l'accesso a qualcuno, questi deve conoscere la password e può fare le stesse cose che fate voi. Se volete togliergli l'accesso, non potete, l'unico modo è cambiare la password. È meglio passare completamente a SSH e utilizzare chiavi RSA. Spesso mi stupisco che anche le aziende risolvano questo problema al giorno d'oggi. Non creare mai una tabella di tutte le password. E di certo non è mai stata condivisa da tutta la squadra!
- Il **codice sorgente si trova in Git**, i dati nel database e il contenuto statico (immagini, PDF, ...) in file su disco. La scelta dell'archiviazione dei dati sbagliata peggiora la progettazione e la sicurezza dell'applicazione. L'URL di un PDF (ad esempio di una fattura) deve contenere un contenuto generato in modo casuale che solo il destinatario conosce. Per i contenuti estremamente sensibili (come un estratto conto bancario), è importante crittografare il contenuto su disco e fornirlo solo dopo il login.
- Le parti non pubbliche del sito devono contenere gli header META `noindex` e `nofollow` per evitare di essere indicizzate da Google. I contenuti sensibili che i vostri concorrenti non devono conoscere devono essere protetti da password.
- Disattivare [directory content listing](https://www.simplified.guide/apache/disable-directory-listing) sul proprio server. Se impostato in modo non appropriato, l'intero sito può essere sottoposto a crawling automatico per trovare file sensibili.
- Progetto radice! Non deve mai esserci un URL diretto ai file di configurazione. Per esempio, [Nette does it right](https://doc.nette.org/cs/3.0/quickstart/getting-started#toc-obsah-web-projectu) fin dal primo tutorial. Lo stesso vale per [repository git pubblicamente disponibili](https://smitka.me/open-git/). Questo tipo di vulnerabilità è estremamente facile da sottovalutare e le conseguenze tendono a essere tragiche.
- Il bug può anche derivare da un errore involontario di sviluppo. È molto importante effettuare una revisione del codice da parte di una persona in carne e ossa per ogni modifica. Molti bug vengono scoperti da [PHPStan](https://github.com/phpstan/phpstan) o da strumenti simili prima che un essere umano veda il codice.
- Mai [inviare password in forma leggibile via e-mail](https://www.lupa.cz/clanky/reset-a-poslani-hesla-v-citelne-podobe-e-mailem-nebezpecna-praktika/), c'è sempre un altro modo per risolvere il problema.
- Problemi di sicurezza nelle vecchie versioni di librerie e software. I robot si muovono su Internet ogni secondo e attaccano le falle di sicurezza conosciute (SQL injection, XSS, CSRF, ...). Molte falle note hanno versioni precedenti di WordPress.

Le vulnerabilità sono molte di più e i problemi variano da progetto a progetto. Se dovete fare una rapida verifica del server, vi consiglio di usare [Maldet](https://www.rfxn.com/projects/linux-malware-detect/) e poi di assumere uno specialista adatto che vi aiuti a fare una [verifica completa del sito](https://baraja.cz/audit-webu), e non solo per motivi di sicurezza.

Gli ingegneri della sicurezza sono come la plastica nell'oceano: per sempre. Abituatevi.

Il principio di azione e reazione della natura
-------------------------------

Avrete anche notato che la natura utilizza quasi sempre il principio di reazione. Ciò significa che qualcosa accade in un particolare ambiente e gli organismi circostanti reagiscono ad esso in modi diversi. Questo approccio presenta molti vantaggi, ma un enorme svantaggio: rimarrete sempre indietro.

In qualità di pensatori (progettisti, sviluppatori, consulenti) avete un grande vantaggio, che è quello di essere perseguibili, cioè di conoscere in anticipo una certa parte dei grandi rischi e di evitare attivamente che si verifichino. Forse non riuscirete mai a prevenire tutti gli errori, ma potete almeno attenuarne le conseguenze o creare meccanismi di controllo che rilevino i problemi in anticipo, in modo da avere il tempo di reagire.

La maggior parte degli attacchi avviene in un lungo periodo di tempo e, se si registra, di solito si ha abbastanza tempo per rilevare il problema.
