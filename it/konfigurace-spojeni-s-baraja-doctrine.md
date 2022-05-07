Configurare la connessione di Baraja Doctrine
=============================================

> id: fbd0961a-53fe-4713-8526-82e36bd1fb9b
> slug:
> 	cs: konfigurace-spojeni-s-baraja-doctrine
> 	it: configurare-la-connessione-di-baraja-doctrine
> 
> publicationDate: '2020-09-10 11:38:44'
> mainCategoryId: '818d311a-0f58-4df7-a9a4-da7d21489dd6'
> sourceContentHash: cee6f56731ae88d1de9aac0da49e0874

Per stabilire una connessione al database all'interno di [Baraja Doctrine] (https://github.com/baraja-core/doctrine) è necessario utilizzare il file di configurazione Neon, che è una parte comune del framework Nette.

La configurazione può assomigliare a questa:

```neon
baraja.database:
    connection:
        host: localhost
        dbname: my-database
        user: root
        password: ******
```

Quando il contenitore DI viene compilato, la configurazione viene verificata e viene lanciato un messaggio di errore che descrive l'errore specifico.

Le credenziali di accesso sono verificate in modo sicuro quando il contenitore viene compilato e poi fisicamente memorizzate nel contenitore. Solo il servizio che fornisce la connessione al database ha quindi accesso ai login, e non possono essere semplicemente ottenuti da un servizio esterno o da un visitatore disonesto dalla barra Tracy.

Compatibilità all'indietro
----------

In passato, si usavano definizioni tramite parametri, per esempio:

```neon
parameters:
    database:
        primary:
            host: localhost
            ...
```

Tuttavia, questa impostazione è segnata come **deprecata** per aumentare la sicurezza delle applicazioni. Quando si usano i parametri, qualsiasi servizio (o anche parte dell'applicazione) potrebbe richiedere le credenziali di accesso, o la barra Tracy attiva sulla pagina potrebbe darli via.
