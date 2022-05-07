UUID a wydajność aplikacji wielkoskalowych
==========================================

> id: '2f072ce8-13b1-41f6-b328-2bd3b416cdd2'
> slug:
> 	cs: uuid-performance
> 	pl: uuid-a-wydajnosc-aplikacji-wielkoskalowych
> 
> publicationDate: '2019-11-08 10:09:54'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: '3b6d0e37684aedc0aedd92f2654e3626'

Gdy rozmiar bazy danych przekracza miliony wierszy, zaleca się rozpoczęcie skalowania aplikacji i rozdzielenie bazy danych na wiele serwerów fizycznych.

Największym problemem związanym z podziałem bazy danych na wiele części jest jej późniejsza synchronizacja, gdy użytkownik zażąda określonych danych.

Dlaczego warto używać identyfikatora UUID i jakie są jego zalety w porównaniu z autoincrement
--------------------------------------------------------

Załóżmy, że masz tabelę `artykulów`, ale ponieważ masz ogromną witrynę, na wyższych poziomach znajdują się dziesiątki milionów artykułów i musisz fizycznie rozdzielić je na wiele maszyn.

Gdybyśmy mieli użyć zwykłej liczby całkowitej jako `id` (klucz główny) z ustawieniem autoinkrementacji, bardzo szybko okazałoby się, że podczas tworzenia rekordów na różnych maszynach w sposób zdecentralizowany, a następnie ich synchronizacji, dochodzi do kolizji identyfikatorów i musimy w skomplikowany sposób przenumerować rekordy. Dodatkowo, jeśli rozwiązujemy wiele sesji do innych tabel, może to być bardzo skomplikowane i łatwo popełnić błąd.

Dlatego zamiast identyfikatora numerycznego można wygenerować `UUID`, czyli ciąg tekstowy generowany przez złożony algorytm, który gwarantuje, że będzie on unikalny, nawet jeśli zostanie wygenerowany niezależnie na wielu maszynach.

Zalety:

- Jeśli masz wiele niezależnych baz danych, które następnie synchronizujesz, użycie identyfikatora UUID oznacza, że jeden identyfikator jest unikalny we wszystkich bazach danych, a nie tylko w tej, w której się znajdujesz i w której został wygenerowany. Po połączeniu w jeden klaster nie powstają żadne konflikty.
- Możesz znać swój `klucz główny` przed faktycznym wstawieniem rekordu do bazy danych. Zmniejsza to liczbę zapytań SQL, upraszcza logikę transakcji i pozwala łatwo użyć go jako klucza obcego, zanim kolekcja rekordów będzie istnieć.
- Identyfikator UUID nie ujawnia informacji o liczbie dat i sekwencji i jest bezpieczniejszy do stosowania w adresach URL. Na przykład, jeśli stwierdzę, że jestem użytkownikiem `19010018`, łatwo się domyślić, że istnieje również użytkownik `19010017` i inni. Atak ten nazywany jest atakiem wektorowym.

Generowanie nowego identyfikatora UUID
----------------------

UUID można uzyskać albo za pomocą prostego zapytania SQL `SELECT UUID();`, ale zwiększa to liczbę zapytań do bazy danych i tracimy możliwość przygotowania danych najpierw hurtowo w logice aplikacji, a następnie zapisania ich od razu.

Dlatego chętnie korzystam z pakietu <a href="https://github.com/ramsey/uuid">ramsey/uuid</a> uzyskanego przez Composera jako dobrego rozwiązania. Sam identyfikator UUID ma kilka wersji, a pakiet może dowolnie generować ich rodzaje.

Dzięki temu jest łatwy w użyciu:

```php
require 'vendor/autoload.php';

use Ramsey\Uuid\Uuid;

// Generuje obiekt UUID wersji 1 (oparty na czasie)
$uuid1 = Uuid::uuid1();
echo $uuid1->toString() . "\n"; // e4eaaaf2-d142-11e1-b3e4-080027620cdd

// Generuje obiekt UUID w wersji 3 (oparty na nazwie i haszowany jako MD5)
$uuid3 = Uuid::uuid3(Uuid::NAMESPACE_DNS, 'php.net');
echo $uuid3->toString() . "\n"; // 11a38b9a-b3da-360f-9353-a5a725514269

// Generuje obiekt UUID w wersji 4 (losowy)
$uuid4 = Uuid::uuid4();
echo $uuid4->toString() . "\n"; // 25769c6c-d34d-4bfe-ba98-e0ee856f3e7a

// Generuje obiekt UUID w wersji 5 (oparty na nazwie i haszowany jako SHA1)
$uuid5 = Uuid::uuid5(Uuid::NAMESPACE_DNS, 'php.net');
echo $uuid5->toString() . "\n"; // c4a760a8-dbcf-5254-a0d9-6a4474bd1b62
```

Jeśli korzystasz z Doctrine, istnieje rozszerzenie <a href="https://github.com/ramsey/uuid-doctrine">ramsey/uuid-doctrine</a>, które generuje identyfikator bezpośrednio jako typ danych.

Fizyczne przechowywanie danych w bazie danych
---------------------------

W moich pierwszych próbach użyłem `varchar(36)` jako klucza głównego (ID), ale <a href="https://www.facebook.com/groups/backendisti/permalink/2465260887049808/">to wcale nie jest dobry pomysł</a>.

> **Objaśnienie logiki wewnętrznej:**.
>
> > Bazy danych MySql (i wiele innych) nie potrafią efektywnie używać `varchar`, `char` lub innych typów danych wyrażających ciąg znaków jako klucza głównego.
> W niektórych bazach danych istnieje typ danych `GUID`, który jest przeznaczony do bezpośredniego przechowywania identyfikatorów UUID. Jeśli nie można użyć tego typu, istnieje odpowiedni zamiennik w postaci `binary(16)`.

Podczas fizycznego sprawdzania bazy danych identyfikator jest wtedy reprezentowany w formacie HEX (ponieważ nie można wyświetlić formatu binarnego), zamiast ładnego identyfikatora `726c67c4-e5eb-4a4c-8fcc-031da5d6f3c6`, zobaczysz po prostu `726C67C4E5EB4A4C8FCC031DA5D6F3C6`, co wygląda jak `'?kYߟKg2c;'` w zapytaniu INSERT.

Konwersja oryginalnych danych z `varchar(36)` na `binary(16)`.
----------------------------------------------------

Zakładam, że reprezentujesz (lub planujesz reprezentować) nowo ustawiony identyfikator w bazie danych jako:

```sql
`id` binary(16) NOT NULL
```

Jednak sama zmiana typu danych nie działa, dlatego należy zastosować coś takiego:

```sql
SET FOREIGN_KEY_CHECKS=0;

ALTER TABLE article CHANGE id id BINARY(16) NOT NULL

SET FOREIGN_KEY_CHECKS=1;
```

Istnieją zasadniczo dwa powody:

- Klucz główny i sesja do niego `muszą mieć ten sam typ danych`. Dlatego należy zmienić zarówno typ danych dla identyfikatora artykułu, jak i na przykład w tabeli relacyjnej, która dopasowuje artykuły do autorów.
- Format binarny zawiera coś nieco innego niż oryginalny ciąg znaków. Należy użyć funkcji konwersji.

Dlatego jedynym właściwym rozwiązaniem jest utworzenie kopii zapasowej danych (ale i tak należy to robić przed każdą migracją), przygotowanie pustej bazy danych z funkcjonalnymi relacjami i ponowne umieszczenie w niej danych za pomocą migracji.

Jeśli identyfikatory UUID zostały wcześniej wygenerowane w dziwny sposób, lepiej jest wybrać jakąś sekwencyjną metodę uzyskiwania identyfikatora UUID i przenumerować wszystkie rekordy. Powodem jest to, że układ sekwencyjny pozwala na lepsze uporządkowanie wartości i utworzenie `drzewa`, co sprawia, że wydajność jest prawie identyczna jak w przypadku `bigint`.

**Jeśli znasz lepszy sposób na konwersję istniejącej bazy danych z UUID przechowywanych w formacie varchar do formatu binarnego bez konieczności opracowywania skomplikowanych migracji i z zachowaniem kluczy obcych, byłbym bardzo wdzięczny za informacje zwrotne**.
