Il principio dell'incapsulamento nei DPI
========================================

> id: '54968a42-b678-4385-91ac-c13ba96c9b34'
> slug:
> 	cs: zapouzdreni
> 	it: il-principio-dell-incapsulamento-nei-dpi
> 
> publicationDate: '2020-02-16 21:21:35'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '3fbce6cdcbf5f274312496604569d9c2'

Uno dei principi principali della OOP è il **principio di incapsulamento**, che dice che i problemi complessi dovrebbero essere suddivisi in tanti piccoli problemi che possiamo risolvere indipendentemente e simultaneamente. Allo stesso tempo, a noi come utenti non importa come avviene e i dati (stato interno) rimangono isolati.

Per esempio, se stiamo risolvendo il problema di come restituire il risultato `1.6` basato su una query utente con l'espressione `(5+3)*(2/(7+3))`, probabilmente nessuno di noi può scrivere una singola funzione o metodo che risolva questo problema in una volta sola.

**TIP:** Una soluzione pronta per questo tipo di esempio è nell'articolo <a href="/pokrocila-kalkulacka">Processare un'espressione matematica come una stringa</a>, ma siate preparati al fatto che non è facile.

L'incapsulamento porta l'astrazione sugli oggetti
-----------------------------------------

Con l'incapsulamento, sarete in grado di usare gli oggetti "come utente", cioè chiamare i loro metodi e non preoccuparvi affatto di come funzionano internamente.

Supponiamo di avere a che fare con il calcolo dello stipendio di un dipendente e vogliamo usare una classe esistente di un altro programmatore per farlo. Abbiamo solo bisogno di conoscere i parametri obbligatori del costruttore e possiamo "semplicemente usare" la classe:

```php
$mzda = new MzdaZamestnance(
    25000, // stipendio lordo
    6,     // numero di anni nell'azienda
    10,    // numero di anni di esperienza
    true   // è un uomo?
);

echo $mzda->getHruba(); // 25000

echo $mzda->getCista(); // 17800
```

I parametri dell'oggetto sono fittizi e non corrispondono realisticamente a come viene calcolato il salario. In particolare, il principio è illustrato dal fatto che abbiamo **solo bisogno di conoscere l'interfaccia pubblica generale** e non abbiamo nemmeno bisogno di occuparci dello **stato interno dell'oggetto**, nemmeno dell'**implementazione interna**, e certamente non del **perché funziona come funziona**. Chiamiamo semplicemente il metodo `getCista()` e otteniamo il guadagno netto.

L'incapsulamento è un problema di progettazione
----------------------------

È importante notare che **l'incapsulamento in sé non è una caratteristica o sintassi del linguaggio**. Il fatto che una classe e un'applicazione siano incapsulate è solo una questione del programmatore che progetta l'applicazione e pensa al codice.

Pensate sempre al design della classe in questo modo:

- KISS (keep it simple), mantenere l'interfaccia semplice e non forzare l'utente a pensare inutilmente. Risolvete la logica complessa per l'utente e lui ve ne sarà grato.
- L'utente della classe (un altro programmatore o voi del futuro) non ha bisogno di conoscere la logica interna e solo i nomi dei metodi e i loro parametri dovrebbero essere sufficienti.
- Se ho bisogno di calcoli ausiliari per il calcolo che non sono di interesse per l'utente e sono solo tecnici, non ha senso creare un getter per loro e dovrebbero essere calcolati solo internamente.
- La classe deve soddisfare le proprietà di base dell'algoritmo, in particolare che funzioni in generale per qualsiasi dato.
- I metodi pubblicamente disponibili dovrebbero essere progettati per fornire abbastanza informazioni per estendere facilmente l'oggetto con nuove caratteristiche in futuro, in modo da poter calcolare facilmente nuovi dati da ciò che già conosciamo.

Mantenere i dati interni non pubblici
-------------------------------

Per le proprietà e i metodi che trattano la logica interna, ha senso impostare la visibilità come `privata`. Il vantaggio principale di questo è che non saranno chiamati dall'esterno e l'utente sarà costretto a usare la vostra interfaccia progettata, proteggendo così i dati e lo stato interno dell'oggetto.

Per esempio, abbiamo un oggetto che rappresenta un conto bancario in cui vogliamo inserire dei pagamenti e trattare il saldo corrente:

```php
class BankAccount
{
    private int $sum;

    public function __construct(int $startSum)
    {
        $this->sum = $startSum >= 0 ? $startSum : 0;
    }

    public function getSum(): int
    {
        return $this->sum;
    }

    public function pay(int $price): void
    {
        $newSum = $this->sum - $price;
        if ($newSum < 0) {
            throw new \Exception('Tu non hai tutti quei soldi!');
        }

        $this->sum = $newSum;
    }

    public function addMoney(int $money): void
    {
        $this->sum += $money;
    }
}
```

Si noti che la classe contiene solo una singola proprietà `privata` `$sum`, che contiene il saldo corrente.

Se vogliamo ottenere il saldo corrente, c'è un metodo `getSum()` per questo, ma non abbiamo modo di cambiare il nuovo valore del saldo. Possiamo solo rimuovere denaro usando il metodo `pay()` o aggiungere denaro usando il metodo `addMoney()`.

Grazie a questo principio, sappiamo sempre con certezza che nessuno può rompere l'oggetto.

Se l'utente cerca di pagare più denaro di quello che c'è effettivamente nel conto, il metodo `pay()` non lo permetterà perché esegue un calcolo di controllo prima di sovrascrivere la proprietà `$sum` e se il saldo dovesse essere negativo (meno di zero), viene lanciata un'eccezione di errore e l'operazione si ferma.

Conclusione
-----

Abbiamo dimostrato il principio di base dell'incapsulamento, che ci permette di pensare meglio all'astrazione degli oggetti e porta una prospettiva completamente nuova.

Una volta che hai afferrato bene questo principio, vedrai che <a href="/proc-use-frameworks">i frameworks iniziano ad avere un enorme senso</a>, perché incapsulano internamente un sacco di intelligenza che puoi semplicemente usare.

La prossima volta guarderemo <a href="/dedicatezza-e-visibilità">dedicatezza e visibilità</a>.
