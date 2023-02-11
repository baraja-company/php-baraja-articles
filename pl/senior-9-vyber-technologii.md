Jak wybierać technologie? Kiedy przechodzimy na JavaScript?
===========================================================

> id: d3ea7ea7-3e2f-455d-a424-cce374bcd1d2
> slug:
> 	cs: senior-9-vyber-technologii
> 	pl: jak-wybierac-technologie-kiedy-przechodzimy-na-javascript
> 
> publicationDate: '2023-02-11 14:40:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: '72807451867478e1a7b6d67f4e5c31b3'

Wybór odpowiednich technologii to warunek konieczny, aby zostać senior developerem. Te decyzje często nie są łatwe, ponieważ musisz wziąć pod uwagę obecny stan techniczny aplikacji, gdzie idziesz rozwojowo, jaka jest wiedza twojego obecnego zespołu, jaka wiedza jest powszechna na rynku pracy, jakie są koszty każdej technologii, jakie ryzyko przyniesie ona do twojego działania, jak bezpieczna i stabilna jest technologia i wreszcie, co będzie interesowało deweloperów, powiedzmy, za 5 lat, kiedy 80% twojego obecnego zespołu zostanie zastąpione.

Przeszedłem przez 6 dużych firm, które rozwijają się w PHP. Tylko 2 z nich próbuje w dłuższej perspektywie przejść na inną technologię, pozostali zostają. To wiąże się z wieloma problemami. Na przykład, obecnie próbuję znaleźć starszego programistę PHP dla projektu korporacyjnego, który rozwijam dla O2, z wymogiem dojeżdżania do biur w Pradze, i widzę, jak rynek programistów PHP oczyścił się w ciągu ostatnich 5 lat. PHP po prostu nie jest już cool i mało kto chce to robić. Jest w nim za mało juniorów.

Z wywiadów z młodszymi osobami odbieram, że React i ogólnie "cienkie" technologie są obecnie bardzo popularne. Z perspektywy architektury aplikacji ma to sens, jeśli wcześnie odkryjesz ten kierunek i masz czas na dostosowanie. Zamiast skomplikowanego walenia układów i formularzy webowych w Latte, do którego potrzebujesz praktycznie przeciętnego developera do już nieco bardziej skomplikowanego zadania, w React wystarczy junior, który zaczął w zasadzie miesiąc temu i nadal nie popełnia zbyt wielu błędów w przyszłym rozwiązaniu.

React pozwala wyrzucić duży kawałek backendu, który został napisany tylko po to, aby frontend mógł istnieć w pierwszej kolejności. W skrócie, sprawia, że rozwój jest tańszy, a jako bonus otrzymujesz szybsze dostarczanie nowych funkcji, ponieważ programiści nie muszą zajmować się w kółko skomplikowanymi problemami, które wynikają z języka projektowego PHP.

Większość aplikacji internetowych nie potrzebuje już nawet backendu, lub tylko minimalnego backendu. Kiedy wystawiasz punkty końcowe API w Node.js (również technologia zbudowana na javascript), nagle deweloper, który wcześniej robił tylko React, może napisać kawałki backendu również, ponieważ jest to ten sam język.

W głębszej analizie projektów, które rozwijałem przez ostatnie 5 lat, w Node.js brakuje tylko kilku rzeczy, które wciąż sprawiają, że używam PHP do niektórych operacji.

Mianowicie:

- Doktryna (i w ogóle dostęp do relacyjnej bazy danych oparty na encjach obiektów)
- Wysyłanie i zarządzanie wiadomościami e-mail
- SOAP API (niestety nadal czasami jest w pobliżu)
- Sessions (musisz zastąpić go np. tokenem JWT)
- Złożona logika, która została napisana w PHP i nie mogę jej łatwo refaktoryzować
- Szybkie przetwarzanie złożonych struktur danych, gdzie dane muszą być mutowane
- Istniejące osoby w zespole, które musisz przekwalifikować, aby robiły coś nowego

Ale potem pojawia się Node.js, który robi resztę rzeczy lepiej. Na przykład:

- Możliwość wyładowania aplikacji prosto do chmury
- Dużo (może nawet dwa razy) tańszy rozwój tej samej funkcjonalności
- Ta sama logika w BE i FE bez konieczności podwójnego pisania kodu
- Punkty końcowe REST API
- Równoległe wywołania wielu kodów jednocześnie
- Możliwość wysłania odpowiedzi HTTP, ale kod nadal działa
- kolesiostwo
- Biblioteki do pracy z usługami w chmurze
- Znacznie lepszy czas reakcji, ponieważ nie trzeba uruchamiać ogromnej aplikacji
- W pełni funkcjonalny paradygmat (pozbywasz się DI, których nie potrzebujesz np. w JS)
- Praca z formularzami i danymi
- Łatwe aktualizacje i aktywna społeczność deweloperów

**Komentarz dotyczący Doctrine:** Wiem, że JS przynosi wiele bibliotek do pracy z bazami danych. Albo nawet nowe paradygmaty, takie jak Mongo. Podoba mi się, gdzie zmierza kierunek przetwarzania danych. Z drugiej strony uważam, że tabularne relacyjne bazy danych nigdy nie odejdą. Kiedy robisz naprawdę duży projekt, w którym zarządzasz dziesiątkami milionów rekordów, po prostu potrzebujesz tradycyjnej technologii, z którą jesteś bardzo zaznajomiony i wiesz, czego się spodziewać. Na przykład pomysł, że chcę dodać kolumnę (właściwość) i oznaczałoby to remapowanie wszystkich podmiotów za pomocą skryptu migracji, jest dla mnie dość przerażający.
