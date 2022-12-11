Segnalatori di funzioni / interruttori di attivazione/disattivazione delle funzioni
===================================================================================

> id: '4209c32f-655c-46fa-9090-e5ec3883ea61'
> slug:
> 	cs: feature-flagy
> 	it: segnalatori-di-funzioni-interruttori-di-attivazione-disattivazione-delle-funzioni
> 
> publicationDate: '2022-12-11 15:00:00'
> mainCategoryId: '28a2aef7-7490-43a9-add7-80d8a051f8a9'
> sourceContentHash: '1e32c312eab2ca16539cdb7431e2dda9'

Quando si sviluppa un'applicazione più complessa, si apprezza la possibilità di sviluppare più funzioni in anticipo, distribuirle con la versione successiva del software e abilitarle in seguito.

È proprio per questo che sono state create le bandiere di funzionalità. Questo articolo vi mostrerà come utilizzarli.

Implementazione di base
---------------------

I flag delle caratteristiche sono fondamentalmente un concetto molto semplice di chiamata di una singola funzione/metodo che decide se una nuova caratteristica è attiva.

Ad esempio:

```php
echo '<h1>Applicazioni per il meteo</h1>';
echo 'Oggi è così:' . getWeather();

if (feature('mappa')) {
   echo 'Mappa:' . getMap();
}
```

Per verificare la disponibilità di una particolare notizia, viene richiamata la funzione `feature()`, che decide se consentire o ignorare la particolare caratteristica in base al nome della chiamata.

Implementazione della logica decisionale
-------------------------------

La logica decisionale è spesso complessa. Ad esempio, è possibile eseguire una funzione specifica solo a partire da una data specifica o per gli utenti di un gruppo specifico. Ad esempio, spesso verifico l'implementazione di una nuova funzionalità su, ad esempio, il 5% degli utenti, in modo che non colpisca tutti in una volta.

Ad esempio, quando sviluppiamo un software aziendale, è così che gestiamo campagne pubblicitarie e sconti validi a partire da una certa data.

Se una particolare nuova funzionalità si rompe, è possibile semplicemente disabilitarla con un flag di funzionalità per gli utenti e abilitarla per un gruppo di sviluppatori che la testeranno e porteranno una correzione, ad esempio.
