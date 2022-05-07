UUID und Leistung von Großanwendungen
=====================================

> id: '2f072ce8-13b1-41f6-b328-2bd3b416cdd2'
> slug:
> 	cs: uuid-performance
> 	de: uuid-und-leistung-von-grossanwendungen
> 
> publicationDate: '2019-11-08 10:09:54'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: '3b6d0e37684aedc0aedd92f2654e3626'

Wenn die Größe der Datenbank Millionen von Zeilen überschreitet, ist es ratsam, die Anwendung zu skalieren und die Datenbank auf mehrere physische Server aufzuteilen.

Das größte Problem bei der Aufteilung der Datenbank in mehrere Teile ist die anschließende Synchronisierung, wenn der Benutzer bestimmte Daten anfordert.

Warum UUID und was sind die Vorteile gegenüber Autoinkrement
--------------------------------------------------------

Nehmen wir an, Sie haben eine Tabelle mit "Artikeln", aber da Sie eine riesige Website haben, gibt es mehrere Millionen Artikel und Sie müssen sie physisch auf mehrere Rechner verteilen.

Wenn wir eine gewöhnliche Ganzzahl als "ID" (Primärschlüssel) mit der Einstellung "Autoinkrement" verwenden würden, würden wir sehr schnell feststellen, dass es bei der dezentralen Erstellung von Datensätzen auf verschiedenen Rechnern und ihrer anschließenden Synchronisierung zu ID-Kollisionen kommt und wir die Datensätze auf komplizierte Weise neu nummerieren müssen. Wenn wir außerdem viele Sitzungen in andere Tabellen auflösen, kann dies ein sehr komplexer Overhead sein, bei dem man leicht Fehler machen kann.

Daher können wir anstelle eines numerischen Bezeichners eine "UUID" generieren, eine Textzeichenkette, die durch einen komplexen Algorithmus erzeugt wird, der garantiert, dass sie eindeutig ist, selbst wenn sie unabhängig auf mehreren Rechnern erzeugt wird.

Vorteile:

- Wenn Sie mehrere unabhängige Datenbanken haben, die Sie dann synchronisieren, bedeutet die Verwendung einer UUID, dass eine ID in allen Datenbanken eindeutig ist, nicht nur in der, in der Sie sich befinden und in der sie erstellt wurde. Wenn sie zu einem einzigen Cluster zusammengeführt werden, entstehen keine Konflikte.
- Sie können Ihren "Primärschlüssel" kennen, bevor Sie den Datensatz tatsächlich in die Datenbank einfügen. Dies reduziert die Anzahl der SQL-Abfragen, vereinfacht die Transaktionslogik und Sie können ihn problemlos als Fremdschlüssel verwenden, bevor die Datensatzsammlung existiert.
- Die UUID gibt keine Informationen über die Anzahl der Daten und Sequenzen preis und ist sicherer für die Verwendung in URLs. Wenn ich zum Beispiel feststelle, dass ich der Benutzer `19010018` bin, ist es leicht zu erraten, dass der Benutzer `19010017` und andere auch existieren. Der Angriff wird als Vektorangriff bezeichnet.

Erzeugen einer neuen UUID
----------------------

Die UUID kann entweder durch eine einfache SQL-Abfrage `SELECT UUID();` ermittelt werden, aber dadurch erhöht sich die Anzahl der Abfragen an die Datenbank und wir verlieren die Möglichkeit, die Daten zunächst in der Anwendungslogik in großen Mengen vorzubereiten und sie dann auf einmal zu schreiben.

Daher verwende ich gerne das <a href="https://github.com/ramsey/uuid">ramsey/uuid</a>-Paket von Composer als gute Lösung. Die UUID selbst hat mehrere Versionen, und das Paket kann spielerisch alle Arten nach Bedarf erzeugen.

Das macht die Nutzung einfach:

```php
require 'vendor/autoload.php';

use Ramsey\Uuid\Uuid;

// Erzeugt UUID-Objekt der Version 1 (zeitbasiert)
$uuid1 = Uuid::uuid1();
echo $uuid1->toString() . "\n"; // e4eaaaf2-d142-11e1-b3e4-080027620cdd

// Erzeugt UUID-Objekt der Version 3 (namensbasiert und als MD5 gehasht)
$uuid3 = Uuid::uuid3(Uuid::NAMESPACE_DNS, 'php.net');
echo $uuid3->toString() . "\n"; // 11a38b9a-b3da-360f-9353-a5a725514269

// Erzeugt ein UUID-Objekt der Version 4 (zufällig)
$uuid4 = Uuid::uuid4();
echo $uuid4->toString() . "\n"; // 25769c6c-d34d-4bfe-ba98-e0ee856f3e7a

// Erzeugt UUID-Objekt der Version 5 (namensbasiert und als SHA1 gehasht)
$uuid5 = Uuid::uuid5(Uuid::NAMESPACE_DNS, 'php.net');
echo $uuid5->toString() . "\n"; // c4a760a8-dbcf-5254-a0d9-6a4474bd1b62
```

Wenn Sie Doctrine verwenden, gibt es eine Erweiterung <a href="https://github.com/ramsey/uuid-doctrine">ramsey/uuid-doctrine</a>, die die ID direkt als Datentyp erzeugt.

Physische Speicherung in der Datenbank
---------------------------

In meinen ersten Versuchen habe ich `varchar(36)` als Primärschlüssel (ID) verwendet, aber <a href="https://www.facebook.com/groups/backendisti/permalink/2465260887049808/">das ist überhaupt keine gute Idee</a>.

> **Erläuterung der internen Logik:**
>
> > MySql-Datenbanken (und viele andere) können `varchar`, `char` oder andere Datentypen, die eine Zeichenkette ausdrücken, nicht effizient als Primärschlüssel verwenden.
> In einigen Datenbanken gibt es einen Datentyp "GUID", der für die direkte Speicherung von UUIDs vorgesehen ist. Wenn Sie diesen Typ nicht verwenden können, gibt es einen geeigneten Ersatz in der Form `binary(16)`.

Bei der physischen Untersuchung der Datenbank wird die ID dann im HEX-Format dargestellt (da das Binärformat nicht angezeigt werden kann), statt der schönen ID `726c67c4-e5eb-4a4c-8fcc-031da5d6f3c6` sehen Sie nur `726C67C4E5EB4A4C8FCC031DA5D6F3C6`, was in der INSERT-Abfrage wie `'?kYߟKg2c;'` aussieht.

Konvertierung der Originaldaten von "varchar(36)" in "binär(16)".
----------------------------------------------------

Ich nehme an, dass Sie die neu eingestellte ID in der Datenbank als darstellen (oder darstellen wollen):

```sql
`id` binary(16) NOT NULL
```

Allerdings funktioniert eine einfache Änderung des Datentyps nicht, also etwas wie:

```sql
SET FOREIGN_KEY_CHECKS=0;

ALTER TABLE article CHANGE id id BINARY(16) NOT NULL

SET FOREIGN_KEY_CHECKS=1;
```

Dafür gibt es im Wesentlichen zwei Gründe:

- Der Primärschlüssel und die zugehörige Sitzung "müssen denselben Datentyp haben". Daher müssen Sie sowohl den Datentyp für die Artikel-ID als auch z. B. in der relationalen Tabelle, die die Artikel den Autoren zuordnet, ändern.
- Das Binärformat enthält etwas anderes als die ursprüngliche Zeichenfolge. Sie müssen eine Umrechnungsfunktion verwenden.

Die einzig richtige Lösung besteht daher darin, die Daten zu sichern (was Sie aber ohnehin vor jeder Migration tun sollten), eine leere Datenbank mit funktionalen Beziehungen vorzubereiten und die Daten per Migration wieder dorthin zu übertragen.

Wenn Sie zuvor UUIDs auf seltsame Weise erzeugt haben, ist es besser, eine sequenzielle Methode zu wählen, um die UUID zu erhalten und alle Datensätze neu zu nummerieren. Der Grund dafür ist, dass ein sequentielles Layout eine bessere Anordnung der Werte und die Erstellung eines `Baums` ermöglicht, was die Leistung fast identisch mit `Bigint` macht.

**Wenn Sie eine bessere Möglichkeit kennen, eine bestehende Datenbank von UUID, die als varchar gespeichert ist, in ein binäres Format zu konvertieren, ohne komplexe Migrationen durchführen zu müssen und unter Beibehaltung der Fremdschlüssel, wäre ich Ihnen für eine Rückmeldung sehr dankbar**.
