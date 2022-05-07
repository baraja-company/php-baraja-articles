Codici di stato HTTP
====================

> id: '97800bb6-004c-4bdb-b126-f87e77cc5120'
> slug:
> 	cs: stavove-http-kody
> 	it: codici-di-stato-http
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '6c1da4a7233c1eba531e9336daae5acb'

Nella comunicazione HTTP, vengono trasmessi i cosiddetti `codici di stato', che sono informazioni su come è stato fatto il trasferimento. Sono sicuro che sapete che un codice `200` significa successo e un codice `404` significa una pagina inesistente.

I codici di stato sono divisi in diversi gruppi in base al loro prefisso.

1xx Informativo
--------------

| Codice - Significato -
|-------|--------|
| 100` | Continua |
| `101` | Switch Protocol |

2xx Successo
----------

| Codice - Significato -
|-------|--------|
| 200` | OK (tutto OK) |
| Creata.
| `202` | Accettato |
| `203` | Informazioni non autorizzate |
| `204` | Nessun contenuto |
| `205` | Ripristina il contenuto |
| `206` | Contenuto parziale |

3xx Reindirizzamento
----------------

| Codice - Significato -
|-------|--------|
| 300` | Scelta multipla |
| `301` | Permanent Redirect |
| 302` | Trovato |
| `303` | Vedi altro |
| `304` | Nessun cambiamento |
| `305` | Usa il proxy |
| `306` | **Vecchio ma riservato per uso futuro** |
| `307` | Reindirizzamento temporaneo |

4xx Errore del cliente (utente)
-----------------------------

| Codice - Significato -
|-------|--------|
| `400` | Richiesta sbagliata |
| `401` | Connessione non autorizzata |
| `402` | Pagamento richiesto |
| `403` | Disabilitato |
| `404` | Non trovato |
| `405` | Metodo non consentito |
| `406` | Inaccettabile |
| `407` | Autenticazione proxy richiesta |
| 408` | Richiesta scaduta |
| `409` | Conflitto di rete |
| Dati spariti.
| `411` | La lunghezza richiesta non corrisponde |
| Assunzione fallita | `412` |
| `413` | La richiesta di entità è troppo grande |
| `414` | Request-URI è troppo lungo |
| 415` | Tipo di media non supportato |
| `416` | L'ambito richiesto non è soddisfacente |
| 417` | Aspettativa fallita |

5xx Errore del server
--------------

| Codice - Significato -
|-------|--------|
| Errore interno del server.
| `501` | Non implementato |
| `502` | Bad Gateway |
| 503` | Servizio non disponibile |
| `504` | Gateway timeout scaduto |
| `505` | Versione HTTP non supportata |
| `509` | Limite di larghezza di banda superato |
