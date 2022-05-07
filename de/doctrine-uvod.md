Serie Lehre - Einführung
========================

> id: fbd0461a-53fe-4713-8926-82e31bd1fb9b
> slug:
> 	cs: doctrine-uvod
> 	de: serie-lehre---einfuehrung
> 
> publicationDate: '2021-08-27 11:40:00'
> mainCategoryId: '818d311a-0f58-4df7-a9a4-da7d21489dd6'
> sourceContentHash: '2bc62308fcb66acda5a09ac95b5eb2c2'

Doctrine ist eine fortgeschrittene PHP-Bibliothek für objektorientierte Datenbankarbeit. Der Hauptzweck und das Ziel von Doctrine ist es, das Datenbankschema mit Hilfe von Datenentitäten zu beschreiben und die Daten vollständig objektorientiert zu manipulieren.

Dieses Paradigma wird als ORM (Object-relational mapping) bezeichnet. Es handelt sich dabei um ein [Design-Pattern](/Design-Patterns) zur Umwandlung (Wrapping) von in einer relationalen Datenbank gespeicherten Daten in ein Objekt, das in einer objektorientierten Sprache verwendet werden kann. Um Doctrine zu verstehen und zu verwenden, müssen Sie also zumindest die Grundlagen der [objektorientierten Programmierung](/oop) kennen.

Warum Lehre lernen?
------------------------

Dafür gibt es viele Gründe:

- Doctrine ist die am weitesten verbreitete ORM-Datenbank, die von einem Großteil der fortgeschrittenen PHP-Gemeinschaft verwendet wird.
- Es wird das Design Ihrer PHP-Anwendung drastisch vereinfachen
- Sie bieten eine einheitliche Methode für Entwurf, Versionierung, Übertragung und Sicherung Ihres Datenbankschemas
- Sie können [viele Datenbanktabellen erhalten, indem Sie das Paket herunterladen] (https://github.com/baraja-core/shop-product), ohne etwas herausfinden und konfigurieren zu müssen
- Beziehungen zwischen Tabellen werden zu echten physischen Einheiten
- Die Datenbankausgaben sind keine gewöhnlichen untypisierten Arrays, sondern echte physische Objekte
- Sie erhalten eine einfache Möglichkeit, viele Vorgänge gleichzeitig in einer einzigen Transaktion durchzuführen
- Sie können die Sicherheit und Widerstandsfähigkeit Ihrer Anwendungen ganz einfach erhöhen, indem Sie wissen, wann was passiert, und dass es sicher passiert.
- Sie erhalten eine leicht testbare Code- und Datenbankschicht
- Sie werden ein ganzes Ökosystem rund um Doctrine entdecken, das viele Probleme auf elegante Weise löst. Sie finden oft einfache Lösungen für komplexe Probleme, die sonst kaum zu lösen sind
- Sie werden viel Neues lernen, neue Ideen entdecken und das Potenzial der Datenbank voll ausschöpfen.
- Befreien Sie sich von komplexen SQL-Abfragen. Doctrine bietet eine benutzerdefinierte Schnittstelle zum Schreiben von Abfragen (DQL), die sehr leistungsstark ist
- Die Anwendungen werden schneller. Sie werden leicht Spielraum für die Optimierung von Anwendungen entdecken, die Vorteile von "Lazy Loading" nutzen und Engpässe in Anwendungen finden.

Der Autor dieses Artikels ([Jan Barasek](https://baraja.cz)) ist seit langem der Meinung, dass Doctrine der beste Weg ist, um mit einer PHP-Datenbank zu arbeiten. Es hat einfach keine Konkurrenz.

Wie fängt man an?
----------

Bevor Sie Doctrine in vollem Umfang nutzen können, müssen Sie eine geeignete Umgebung vorbereiten. Wenn Sie gerade erst mit PHP anfangen oder noch keine Vorkenntnisse haben, installieren Sie am besten das Nette Framework mit dem [Baraja Doctrine](https://github.com/baraja-core/doctrine) Erweiterungspaket, das automatisch die volle Unterstützung integriert. Laden Sie zunächst das Paket über [Composer](/composer) herunter, richten Sie dann die DI-Erweiterung ein, und Doctrine wird automatisch in Betrieb genommen.

Damit Doctrine korrekt funktioniert, müssen Sie eine leere Datenbank vorbereiten (Doctrine kann auch mit einem bestehenden Projekt arbeiten, aber dies ist für die ersten Schritte ungeeignet, da die Gefahr besteht, dass bestehende Daten überschrieben werden) und die Verbindung konfigurieren. Da Doctrine nicht nur eine Datenbankbibliothek ist, sondern ein fortschrittliches Datenbank-Framework bietet, müssen Sie [andere Konfiguration](/configure-connections-with-baraja-doctrine) lösen. Die meisten Einstellungen werden in diesem Paket für Nette automatisch überschrieben, allerdings muss Ihr Server in der Mindestkonfiguration die Erweiterungen `APCu Cache` oder `SQLite3` unterstützen.

Wenn alles richtig konfiguriert wurde, wird ein neuer DI-Dienst `Baraja\Doctrine\EntityManager` in Nette erstellt, den Sie in Presenter [injizieren](https://doc.nette.org/cs/3.1/di-usage) können:

```php
namespace App\FrontModule\Presenters;

use Baraja\Doctrine\EntityManager;

final class HomepagePresenter extends BasePresenter
{
	#[Inject]
	public EntityManager $entityManager;
}
```

Wenn es Ihnen gelingt, den grundlegenden EntityManager-Dienst zu injizieren, können Sie beginnen, Doctrine zu erlernen und mit ihm zu arbeiten.

Wie soll man vorgehen?
--------

Die folgenden Kapitel sind eine Kombination aus einem Doctrine-Technologie-Referenzhandbuch, jahrelanger Erfahrung, Entwurfsmustern und vorgefertigten Lösungen. Gemeinsam werden wir alle grundlegenden Elemente von Doctrine durchgehen, von der Definition einer benutzerdefinierten Entität über die Erstellung eines physischen Datenbankschemas bis hin zur Arbeit mit einem Versionierungstool und dem Produktionseinsatz.

Ich benutze Doctrine schon sehr lange und habe Tausende von Fällen damit gelöst. Wir zeigen Tipps und Tricks, wie man mit Doctrine die Geschwindigkeit von Datenbanken optimiert und wie man eine Datenbank angemessen gestaltet. Sie können Doctrine auch für ein bestehendes Projekt verwenden (wenn Sie bestimmte Bedingungen erfüllen), und wir zeigen Ihnen, wie Sie das tun können.

Diese Artikelserie wurde erstellt, um meinen Trainings- und Beratungsstudenten zu helfen. Wenn Sie bestimmte Themen ausführlicher besprechen oder erklären möchten, können Sie mir eine E-Mail an jan@barasek.com schicken. Da es sich um eine relativ anspruchsvolle Technologie handelt, werden alle Fragen wie eine bezahlte Beratung behandelt.
