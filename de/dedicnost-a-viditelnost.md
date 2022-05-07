Vererbbarkeit und Sichtbarkeit von PSA
======================================

> id: '71896a55-dcfd-4bb6-9f53-64f22b1514eb'
> slug:
> 	cs: dedicnost-a-viditelnost
> 	de: vererbbarkeit-und-sichtbarkeit-von-psa
> 
> publicationDate: '2020-02-16 22:17:05'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '4584f3dcfdcfdc506d34a37c36fe6dd1'

Eine der grundlegenden Eigenschaften der objektorientierten Programmierung ist die **Vererbung** und die <a href="/verkapselung">Verkapselung</a>. Mit diesen Funktionen können Sie problemlos komplexe Anwendungslogik erstellen und gleichzeitig eine gute Lesbarkeit der Implementierung gewährleisten.

Der Grundsatz der Vererbung
-------------------

Vererbung drückt aus, dass die Implementierung einer Klasse auf einer anderen basiert. In der OOP-Terminologie spricht man von **Descendant** (die Klasse, die erbt) und **Cancestor** (die Klasse, die wir erben).

Im Allgemeinen funktioniert die Vererbung so, dass der Nachkomme alle Eigenschaften des Vorfahren erhält, sie entweder genau so übernimmt, wie sie der Vorfahre hatte, oder sie auf seine eigene Art und Weise modifiziert, oder sie komplett außer Kraft setzt und seine eigene Implementierung verwendet.

Die Verwendung dieses Ansatzes ist sehr breit gefächert, und die Vererbung wird von einer Reihe von <a href="/design-patterns">Designmustern</a> verwendet.

Reale Nutzung der Vererbung - Präsentatoren in einer Anwendung
--------------------

Die Vererbung eignet sich gut für den Entwurf von so genannten **Präsentern**, einer speziellen Art von Klassen, die die Verknüpfungslogik im **MVC**-Designmuster darstellen.

Nehmen wir zum Beispiel ein Trio aus den Seiten "Homepage", "Kontakt" und "Anmeldung".

Bei der Implementierung jeder Seite wird ein Großteil der Logik wiederholt (z. B. Annahme einer Anfrage, Aufbau der URL, Rendering der Vorlage und Übermittlung des resultierenden HTML). Es ist daher praktisch, einen einzigen Vorfahren mit dieser Logik zu implementieren und sie nur in den Nachkommen zu verwenden.

Wir beginnen mit der Definition des Vorgängers (der Klassenname spielt keine Rolle, ich verwende eine Konvention aus dem Nette-Framework):

```php
abstract class BasePresenter
{
   public function link(string $route, array $params = []): string
   {
      // Implementierung der Methode zur Erstellung der URL
   }

   public function renderTemplate(string $path, array $params = []): string
   {
      // Logik für das Rendern von Vorlagen
   }
}
```

Bei der Definition der Klasse habe ich das neue Schlüsselwort `abstract` verwendet, das besagt, dass die Klasse `BasePresenter` abstrakt ist. Das bedeutet, dass wir keine Instanz davon erstellen können, sondern sie nur verwenden müssen, damit eine andere Klasse sie erbt und implementiert. Die Abstraktion hat weitere nützliche Vorteile, die wir später erörtern werden. Eine Klasse muss nicht abstrakt sein, um vererbbar zu sein - das ist nur eine der möglichen Einstellungen.

Jetzt können wir eine zweite Klasse implementieren, zum Beispiel `HomepagePresenter`:

```php
final class HomepagePresenter extends BasePresenter
{
   public function run(): void
   {
       // Rendering-Logik
       $this->renderTemplate('Homepage', [
          'KontaktLink' => $this->link('Kontakt:Standard'),
       ]);
   }
}
```

Jetzt haben Sie eine funktionierende "HomepagePresenter"-Klasse. Beachten Sie, dass die Klasse `final` ist, was bedeutet, dass sie nicht mehr vererbt werden kann, was garantiert, dass die Methoden genau so verwendet werden, wie wir sie angegeben haben.

Als wir die Klasse implementiert haben, haben wir eine neue "run()"-Methode erstellt, die nur von "HomepagePresenter" bedient werden kann. Innerhalb der Methode rufen wir die Methode `renderTemplate()` und `link()` auf, die die Klasse nicht enthält. Das macht aber nichts, denn das Schlüsselwort `extends` sagt uns, von wem die Methoden geerbt werden sollen, also werden diese verwendet.

Dank der Vererbung konnten wir eine Wiederverwendbarkeit des Codes erreichen, da einmal geschriebene Methoden an mehreren Stellen verwendet werden können.

Überschreiben der Implementierung einer bestimmten Methode
------------

Sehr oft kann es sinnvoll sein, das Verhalten einer bestimmten Methode bei der Vererbung zu überschreiben. Wenn wir zum Beispiel das Verhalten der Methode `link()` aus dem vorherigen Beispiel in `ContactPresenter` ändern wollten, würde es wie folgt aussehen:

```php
final class ContactPresenter extends BasePresenter
{
   public function run(): void
   {
      // Rendering-Logik
      echo $this->link('Startseite:Standard', []);
   }

   public function link(string $route, array $params = []): string
   {
      return 'https://baraja.cz';
   }
}
```

Um die Implementierung außer Kraft zu setzen, definieren Sie die Methode einfach erneut im untergeordneten Element und überschreiben Sie den Methodenkörper. Wichtig ist, dass die Schnittstelle eingehalten wird und die gleichen Eingabeargumente implementiert werden.

Sichtbarkeit der Vererbung
--------------------------

Manchmal möchten wir einige Methoden bei der Vererbung ausblenden und nur intern verwenden. Alternativ können sie nur bei der Vererbung und nicht als öffentliche Schnittstelle verwendet werden.

Im Allgemeinen gibt es daher einige einfache Regeln für die Sichtbarkeit. Wir bezeichnen Methoden mit "öffentlich", "geschützt" oder "privat", und die Regeln für die Sichtbarkeit lauten wie folgt:

- Jeder kann "öffentliche" Methoden von überall her aufrufen, d.h. wenn er eine Instanz sowohl eines Vorfahren als auch eines Nachkommens erzeugt.
- Nur ein Vorfahre oder Nachfahre kann "geschützte" Methoden aufrufen, aber sie können nicht von der öffentlichen Schnittstelle aufgerufen werden, wenn eine Instanz erstellt wird. Dies sind interne Methoden, um Vererbung zu erreichen (eine praktische Anwendung ist zum Beispiel die `link()` Methode im vorherigen Beispiel).
- Nur die aktuelle Klasse kann "private" Methoden aufrufen, unabhängig von den Einstellungen der Vererbung und der öffentlichen Schnittstelle.

Ändern der Sichtbarkeit zur Laufzeit
----------------------------

In sehr speziellen Fällen kann es sinnvoll sein, die Sichtbarkeit einer Methode zur Laufzeit zu ändern und sie dann aufzurufen. Dies wird zum Beispiel von verschiedenen Doctrine-Bibliotheken verwendet.

Um die Sichtbarkeit zu ändern, verwenden wir dann die native Klasse **ReflectionClass**, die von PHP selbst implementiert wird.
