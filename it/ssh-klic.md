Comunicazione tramite SSH e chiave RSA2
=======================================

> id: '09ec87dd-810d-4618-b76b-fd30f63be30a'
> slug:
> 	cs: ssh-klic
> 	it: comunicazione-tramite-ssh-e-chiave-rsa2
> 
> publicationDate: '2022-11-12 15:00:00'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '4427ec2be03799f94a860d04be6a72ff'

SSH è un protocollo di rete per il trasferimento criptato di file e terminali. SSH è comunemente utilizzato per il controllo remoto di un server Web e per il trasferimento sicuro di file. A differenza dell'FTP, offre una connessione nativa crittografata. SSH comunica sulla porta predefinita `22`. La connessione viene inizializzata con il nome utente e l'indirizzo del server di destinazione. Per l'autenticazione si può usare una password (non consigliata) o una chiave SSH RSA2 (consigliata).

Ottenere (generare) la chiave
--------------------------

Prima di connetterci al server, dobbiamo ottenere (o generare) la nostra prima chiave SSH RSA2. È importante che si tratti di un algoritmo `RSA2`. Questo perché le chiavi sono numerose e non tutte possono essere utilizzate.

In Linux, per generarla si utilizza l'utility `ssh-keygen`, alla quale si specifica la complessità della chiave (4096 in questo caso) e l'e-mail dell'utente autorizzato:

```shell
ssh-keygen -t rsa -b 4096 -C "jan@barasek.com"
```

Dopo aver eseguito il comando, ci verrà chiesto il percorso in cui memorizzare la chiave e l'eventuale `password` (password di autorizzazione). Non inserite nulla come percorso (la posizione predefinita viene scelta automaticamente) e inserite la passphrase facoltativamente (se lo fate, dovrete inserire la stessa password ogni volta prima di usare la chiave).

La chiave generata viene automaticamente salvata nella posizione predefinita `~/.ssh`, cioè nella directory `.ssh` della home directory dell'utente corrente.

Generazione di una chiave SSH in Windows
-------------------------------

Purtroppo, Windows non ha un percorso predefinito per la chiave SSH. È quindi ideale installare, ad esempio, l'utility `Putty` e `PuttyGen` per generare la chiave. Scegliere sempre l'algoritmo `RSA2`. Anche in questo caso, verrà generata una coppia di chiavi, quindi conservatele in modo sicuro da qualche parte. Prima di utilizzare la chiave SSH in `Putty`, è necessario selezionare il percorso del disco da cui recuperare la chiave. In Linux questo non è necessario, esiste un percorso del disco predefinito.

Sicurezza delle chiavi
---------------

Quando si generano le chiavi, ne vengono effettivamente generate due. Una chiave pubblica (quella che si dà all'altra parte per consentire la comunicazione) e una chiave privata (quella che è solo vostra, che non dovete mai dire a nessuno e che serve per decifrare la comunicazione).

È essenziale non perdere mai la chiave privata. Perderlo significa interrompere l'intera comunicazione!

In genere si consiglia di generare una coppia di chiavi pubbliche/private unica per ogni dispositivo e utente per ridurre la probabilità di una fuga di notizie. Tuttavia, se si desidera trasferire la chiave tra i dispositivi, è possibile farlo. La chiave SSH è a livello di password, quindi quando la si sposta su un'altra macchina la connessione funziona subito.

Alcuni server ricordano l'ultimo dispositivo con cui hanno comunicato via SSH. Pertanto, è possibile che la connessione non funzioni dopo lo spostamento della chiave sul nuovo computer. In questo caso, è necessario cancellare la cache delle chiavi sul server.

Autorizzare la chiave a connettersi al server
--------------------------------------

Il comando `ssh` viene utilizzato per connettersi al server. È sufficiente inserire l'utente e il dominio:

```shell
ssh root@baraja.cz
```

Quindi cercherà di stabilire una connessione SSH. Se si dispone di una chiave SSH funzionante e correttamente configurata, la connessione verrà effettuata automaticamente. In caso contrario, è necessario inserire una password.

Se si desidera autenticarsi sul server utilizzando una chiave SSH invece di una password, è necessario trasferire la propria **chiave pubblica** al server.

È sufficiente visualizzarlo con il comando:

```shell
cat ~/.ssh/id_rsa.pub
```

E copiare l'intero contenuto sul server di destinazione nella posizione `~/.ssh/authorized_keys`. Se si dispone di più di un tasto, ognuno su una linea separata.
