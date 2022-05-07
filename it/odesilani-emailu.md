Funzione PHP mail()
===================

> id: '6536e2e2-38e1-4496-80ef-6c1c9a0b5be5'
> slug:
> 	cs: odesilani-emailu
> 	it: funzione-php-mail
> 
> publicationDate: '2019-09-16 08:51:41'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: ca90a7366754a7e7a0ce43f4252296f8

La funzione `mail()` invia un messaggio di posta elettronica attraverso la configurazione predefinita del server. Per funzionare correttamente, è necessario abilitare la funzione sul server e impostare il server di posta per l'invio.

**La funzione è solo per l'invio. Dovete sistemare la ricezione dei messaggi a livello del server di posta. Per esempio, scaricare regolarmente messaggi usando il protocollo `IMAP` o `POP3`.

> Scoraggio fortemente l'uso della funzione al momento, perché il programmatore deve occuparsi di tutto da solo (per esempio, inviare le intestazioni corrette, o impostare la codifica).
>
> Molto meglio è connettersi tramite <a href="/send-email-mail-smtp">server SMTP</a>.

Utilizzando
-------

```php
mail ('jan@barasek.com', 'Oggetto', 'Testo dell'e-mail...');
```

Il primo parametro è l'indirizzo del destinatario, il secondo l'oggetto e il terzo il testo del messaggio. Il quarto parametro (opzionale) indica la configurazione supplementare del messaggio.
