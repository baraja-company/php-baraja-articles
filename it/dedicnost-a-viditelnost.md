Ereditarietà e visibilità nei DPI
=================================

> id: '71896a55-dcfd-4bb6-9f53-64f22b1514eb'
> slug:
> 	cs: dedicnost-a-viditelnost
> 	it: ereditarieta-e-visibilita-nei-dpi
> 
> publicationDate: '2020-02-16 22:17:05'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '4584f3dcfdcfdc506d34a37c36fe6dd1'

Una delle proprietà fondamentali della programmazione orientata agli oggetti è **l'ereditarietà** e <a href="/incapsulamento">incapsulamento</a>. Con queste caratteristiche, sarete in grado di costruire facilmente una logica applicativa complessa mantenendo una buona leggibilità dell'implementazione.

Il principio di eredità
-------------------

L'ereditarietà esprime che l'implementazione di una classe è basata su un'altra. Nella terminologia OOP, si parla di **discendente** (la classe che eredita) e **ancestor** (la classe che eredita).

In generale, l'ereditarietà funziona con il discendente che ottiene tutte le caratteristiche dell'antenato, o adottandole esattamente come le aveva l'antenato, o modificandole a modo suo, o sovrascrivendole completamente e usando la sua propria implementazione.

L'uso di questo approccio è molto ampio, e l'ereditarietà è usata da un certo numero di <a href="/design-patterns">design patterns</a>.

Uso reale dell'ereditarietà - presentatori in un'applicazione
--------------------

L'ereditarietà è adatta per progettare i cosiddetti **presentatori**, che è un tipo speciale di classe che rappresenta la logica di collegamento nel design pattern **MVC**.

Per esempio, abbiamo un trio di pagine `Homepage`, `Contact` e `Login`.

Come ogni pagina è implementata, molta della logica sarà ripetuta (per esempio, accettare una richiesta, costruire l'URL, rendere il modello e inviare l'HTML risultante). È quindi conveniente implementare un singolo antenato con questa logica e usarlo solo nei discendenti.

Iniziamo definendo prima l'antenato (il nome della classe non ha importanza, sto usando una convenzione del framework Nette):

```php
abstract class BasePresenter
{
   public function link(string $route, array $params = []): string
   {
      // implementazione del metodo per costruire l'URL
   }

   public function renderTemplate(string $path, array $params = []): string
   {
      // logica di rendering del template
   }
}
```

Quando ho definito la classe, ho usato la nuova parola chiave `abstract`, che dice che la classe `BasePresenter` è astratta. Questo significa che non possiamo creare un'istanza di esso, e abbiamo solo bisogno di usarlo in modo che un'altra classe lo erediti e lo implementi. L'astrazione ha altri vantaggi utili, che discuteremo più avanti. Una classe non deve essere astratta per essere ereditabile - è solo una delle possibili impostazioni.

Ora possiamo implementare una seconda classe, per esempio `HomepagePresenter`:

```php
final class HomepagePresenter extends BasePresenter
{
   public function run(): void
   {
       // logica di rendering
       $this->renderTemplate('homepage', [
          'contattoLink' => $this->link('Contatto:default'),
       ]);
   }
}
```

Ora avete una classe `HomepagePresenter` funzionante. Si noti che la classe è `finale`, il che significa che non può più essere ereditata, garantendo che i metodi saranno utilizzati esattamente come li abbiamo specificati.

Quando abbiamo implementato la classe, abbiamo creato un nuovo metodo `run()` che solo `HomepagePresenter` può gestire. All'interno del metodo chiamiamo il metodo `renderTemplate()` e `link()`, che la classe non contiene. Questo non importa però, perché la parola chiave `extends` ci dice da dove i metodi devono essere ereditati, quindi vengono usati quelli.

Grazie all'ereditarietà, siamo stati in grado di ottenere la riusabilità del codice perché una volta scritto, i metodi possono essere usati in più posti.

Sovrascrivere l'implementazione di un metodo specifico
------------

Molto spesso può essere utile sovrascrivere il comportamento di un particolare metodo durante l'ereditarietà. Per esempio, se volessimo cambiare il comportamento del metodo `link()` dell'esempio precedente in `ContactPresenter`, sarebbe come questo:

```php
final class ContactPresenter extends BasePresenter
{
   public function run(): void
   {
      // logica di rendering
      echo $this->link('Homepage:default', []);
   }

   public function link(string $route, array $params = []): string
   {
      return 'https://baraja.cz';
   }
}
```

Per sovrascrivere l'implementazione, basta definire nuovamente il metodo nel figlio e sovrascrivere il corpo del metodo. L'importante è soddisfare l'interfaccia e implementare gli stessi argomenti di input.

Visibilità dell'eredità
--------------------------

A volte vorremmo nascondere alcuni metodi durante l'ereditarietà e usarli solo internamente. In alternativa, permettete loro di essere usati solo durante l'ereditarietà e non come interfaccia pubblica.

In generale, quindi, ci sono alcune semplici regole di visibilità. Denotiamo i metodi con `public`, `protected` o `private`, e le regole di visibilità sono le seguenti:

- Chiunque può chiamare metodi `public` da qualsiasi luogo, cioè quando si crea un'istanza sia di un antenato che di un discendente.
- Solo un antenato o un discendente può chiamare metodi `protetti`, ma non possono essere chiamati dall'interfaccia pubblica quando viene creata un'istanza. Questi sono metodi interni per realizzare l'ereditarietà (una comoda applicazione è per esempio il metodo `link()` nell'esempio precedente).
- Solo la classe corrente può chiamare metodi `privati`, indipendentemente dalle impostazioni dell'ereditarietà e dell'interfaccia pubblica.

Cambiare la visibilità in fase di esecuzione
----------------------------

In casi molto specifici, può essere utile cambiare la visibilità di un metodo in fase di esecuzione e poi chiamarlo. Questo è usato, per esempio, da varie librerie di Doctrine.

Per cambiare la visibilità, usiamo poi la classe nativa **ReflectionClass** implementata da PHP stesso.
