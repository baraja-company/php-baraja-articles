Serie della Dottrina - Introduzione
===================================

> id: fbd0461a-53fe-4713-8926-82e31bd1fb9b
> slug:
> 	cs: doctrine-uvod
> 	it: serie-della-dottrina---introduzione
> 
> publicationDate: '2021-08-27 11:40:00'
> mainCategoryId: '818d311a-0f58-4df7-a9a4-da7d21489dd6'
> sourceContentHash: '2bc62308fcb66acda5a09ac95b5eb2c2'

Doctrine è una libreria PHP avanzata per il lavoro su database orientato agli oggetti. Lo scopo principale e l'obiettivo di Doctrine è descrivere lo schema del database usando entità di dati e manipolare i dati in un modo completamente orientato agli oggetti.

Questo paradigma è chiamato ORM (Object-relational mapping), che è [design-pattern](/design-patterns) per convertire (wrapping) i dati memorizzati in un database relazionale in un oggetto che può essere usato in un linguaggio orientato agli oggetti. Quindi, per capire e usare Doctrine, devi conoscere almeno le basi della [programmazione orientata agli oggetti](/oop).

Perché imparare la Dottrina?
------------------------

Ci sono molte ragioni:

- Doctrine è il database ORM più diffuso, usato dalla maggior parte della comunità PHP avanzata.
- Semplificherà drasticamente la progettazione della vostra applicazione PHP
- Fornisci un modo coerente per progettare, versionare, trasferire e fare il backup dello schema del tuo database
- È possibile [ottenere molte tabelle di database scaricando il pacchetto] (https://github.com/baraja-core/shop-product) senza dover capire e configurare nulla
- Le relazioni tra le tabelle diventano entità fisiche reali
- Gli output del database non saranno normali array non tipizzati, ma si otterranno veri oggetti fisici
- Si ottiene un modo semplice per eseguire molte operazioni simultaneamente all'interno di una singola transazione
- Aumenterete facilmente la sicurezza e la resilienza delle applicazioni semplicemente sapendo quando succede ciò che succede, e che succede in modo sicuro
- Si ottiene un codice facilmente testabile e uno strato di database
- Scoprirete un intero ecosistema intorno a Doctrine che risolve molti problemi in modo elegante. Troverete spesso soluzioni semplici a problemi complessi che altrimenti sono quasi impossibili da risolvere facilmente
- Imparerai un sacco di cose nuove, esplorerai nuove idee e userai il database al suo pieno potenziale
- Sbarazzati delle complesse query SQL. Doctrine fornisce un'interfaccia di scrittura di query personalizzate (DQL) che è molto potente
- Le applicazioni diventeranno più veloci. Scoprirete facilmente lo spazio per l'ottimizzazione dell'applicazione, sfrutterete il lazy loading e troverete i colli di bottiglia dell'applicazione

La lunga opinione dell'autore di questo articolo ([Jan Barasek](https://baraja.cz)) è che Doctrine è il modo migliore per lavorare con un database PHP. Semplicemente non ha concorrenza.

Come iniziare?
----------

Prima di iniziare a usare Doctrine in modo completo, è necessario preparare un ambiente adatto. Se siete agli inizi con PHP o non avete conoscenze approfondite, la scelta migliore è quella di installare il Nette Framework con il pacchetto di estensione [Baraja Doctrine](https://github.com/baraja-core/doctrine), che integra automaticamente il supporto completo. Prima scarica il pacchetto tramite [Composer](/composer), poi imposta la DI Extension e Doctrine inizierà a lavorare automaticamente.

Affinché Doctrine funzioni correttamente, è necessario preparare una base di dati vuota (Doctrine può anche lavorare con un progetto esistente, ma questo è inappropriato per i primi passi perché rischia di sovrascrivere i dati esistenti) e configurare la connessione. Poiché Doctrine non è solo una libreria di database, ma fornisce un framework di database avanzato, è necessario [risolvere altre configurazioni](/configure-connections-with-baraja-doctrine). La maggior parte delle impostazioni sono sovrascritte automaticamente in quel pacchetto per Nette, tuttavia nella configurazione minima il tuo server deve supportare le estensioni `APCu Cache` o `SQLite3`.

Se tutto è stato configurato correttamente, un nuovo servizio DI `Baraja\Doctrine\EntityManager` sarà creato in Nette, che potrete [iniettare](https://doc.nette.org/cs/3.1/di-usage) in Presenter:

```php
namespace App\FrontModule\Presenters;

use Baraja\Doctrine\EntityManager;

final class HomepagePresenter extends BasePresenter
{
	#[Inject]
	public EntityManager $entityManager;
}
```

Se si riesce a iniettare il servizio EntityManager di base, si può iniziare a imparare e lavorare con Doctrine.

Come procedere?
--------

I seguenti capitoli sono una combinazione di una guida di riferimento alla tecnologia Doctrine, anni di esperienza, modelli di progettazione e soluzioni pronte all'uso. Insieme passeremo attraverso tutti gli elementi di base di Doctrine, dalla definizione della propria entità, alla generazione di uno schema di database fisico, al lavoro con uno strumento di versioning e al deployment in produzione.

Uso Doctrine da molto tempo e ho risolto migliaia di casi con esso. Mostreremo suggerimenti e trucchi su come usare Doctrine per ottimizzare la velocità del database e come progettare un database in modo appropriato. Potete anche usare Doctrine per un progetto esistente (se soddisfate certe condizioni) e vi mostreremo come fare.

Questa serie di articoli è stata creata per aiutare i miei studenti di formazione e consulenza. Se avete bisogno di discutere o spiegare alcuni argomenti in modo più dettagliato, potete mandarmi un'email a jan@barasek.com. Poiché si tratta di una tecnologia relativamente esigente, tutte le domande saranno trattate come una consultazione a pagamento.
