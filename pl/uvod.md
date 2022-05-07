Wprowadzenie do PHP
===================

> id: '66b7fd22-6195-4999-ad8d-b799afdc1876'
> slug:
> 	cs: uvod
> 	pl: wprowadzenie-do-php
> 
> perex:
> 	- 'Úvodní tutoriál do jazyka PHP, možnosti jazyka, informace o tvorbě webu v PHP.'
> 	- 'Samouczek wprowadzający do języka PHP, opcje językowe, informacje o tworzeniu stron WWW w PHP.'
> 
> publicationDate: '2019-09-29 19:25:06'
> mainCategoryId: f4a34087-1b51-4761-8128-4459dfe83d8a
> sourceContentHash: db6ba8fe6d5ca28aab62d7302c1d7527

Jest to kurs online do nauki języka PHP, przeznaczony do kształcenia ogólnego. Jest on dostępny bezpłatnie na tej stronie internetowej. Teksty nie powinny być wykorzystywane w żadnym płatnym kursie bez pisemnego potwierdzenia. Użytkownik może bez ograniczeń wykorzystywać w swoich aplikacjach wzory używane w tej witrynie. [Warunki i zasady](https://baraja.cz/vseobecne-obchodni-podminky).

Uaktualnienie z HTML
--------------

"Ulepszenie"? Brzmi to raczej jak medialna manipulacja, i rzeczywiście nią jest.

Nie ma możliwości aktualizacji. PHP zachowuje wszystkie funkcje i cechy czystego dokumentu HTML, dodaje jedynie nowe opcje zapisu i wdrażania. Świetnie, prawda?

Z tworzenia statycznych stron HTML wiesz już, że kod składa się ze znaczników, które są zdefiniowane jako słowa kluczowe ujęte w nawiasy spiczaste (na przykład `<b>Hello!</b>` wyraża pogrubiony tekst **Hello!**).

PHP jest wstawiane do strony HTML w postaci znaczników `<?php` i `?>`, wewnątrz których zapisywana jest inna logika aplikacji. Co ważne, PHP ma własną składnię (zasady, według których pisany jest kod) i, w przeciwieństwie do HTML, nie jest odporny na błędy.

Konkretne przykłady, jak to zrobić i co oznaczają poszczególne znaczniki, zostaną podane później. Na początek warto zrozumieć ogólne zasady działania języka PHP na serwerze oraz sposób przetwarzania kodu.

Przepływ komunikacji między użytkownikiem a serwerem
---------------------------------------

W przypadku zwykłej strony HTML wygląda to mniej więcej tak:

> Użytkownik wysyła żądanie (prosi o konkretną stronę HTML), serwer przegląda dysk i odsyła dokładnie to, co jest na nim zapisane. Nie dzieje się nic szczególnego i nie oczekujcie niczego więcej. Strony będą po prostu statyczne, bez możliwości interakcji z serwerem.

Jeśli jednak dodamy PHP, zaczną się dziać cuda:

> Użytkownik ponownie zażąda wyświetlenia strony. Serwer otwiera plik na dysku, ale widzi, że zawiera on nie tylko czysty HTML, ale także specjalne znaczniki wskazujące na skrypt PHP. Dlatego najpierw je analizuje, a następnie wysyła to, co wygenerowało PHP.

Ocenianie kodu PHP jest domyślnie wykonywane przy każdym załadowaniu strony. W przyszłości dowiesz się, jak buforować kod (przechowywać go skompilowanego w celu szybszego przetwarzania).

Różnica między przetwarzaniem skryptów PHP a C/C++
------------------------------------------

Być może nauczyłeś się używać języka `C` lub `C++` w szkole. PHP jest bezpośrednio oparte na składni języka `C`, a wewnątrz jądra używany jest język `C`, więc dobrze jest znać pewne podobieństwa i różnice.

Podstawową zasadą powszechnie stosowanych programów kompilowanych (które są uruchamiane lokalnie bezpośrednio na komputerze lub smartfonie) jest ładowanie logiki aplikacji do pamięci operacyjnej, która komunikuje się bezpośrednio z systemem operacyjnym, który odbiera dane wejściowe od użytkownika, a następnie wyświetla dane wyjściowe programu. Ważne jest, aby program działał w izolacji od momentu uruchomienia do zakończenia pracy.

PHP zaczyna od każdego żądania renderowania strony, za każdym razem przeładowuje cały kod i dane, a następnie kończy pracę. Dlatego skrypty PHP mają dosłownie yuppie żywotność i zazwyczaj istnieją tylko przez kilkadziesiąt milisekund.

Zaletą tego podejścia jest wyższy poziom izolacji - jeśli coś pójdzie nie tak, wszystko zostanie załadowane ponownie przy następnym załadowaniu strony. Z drugiej strony, podejście to ma wyższe wymagania dotyczące wydajności, ponieważ musimy wielokrotnie łączyć się z bazą danych, odczytywać pliki z dysku itd.

W przyszłości dowiesz się, że skrypty PHP mogą być ładowane do pamięci operacyjnej za pomocą rozszerzenia `OP Cache`, które większość nowych serwerów (od PHP 7.1) ma ustawione w podstawowej konfiguracji.

Pobieranie obcych skryptów PHP
--------------------------

Stosunkowo częstym pytaniem, które omawiamy z uczniami, jest to, jak pobrać z serwera obce skrypty PHP i przejrzeć ich kod źródłowy. Pytanie to poprzedzone jest stwierdzeniem, że kod HTML strony można łatwo wyświetlić w przeglądarce internetowej.

Odpowiedź brzmi: **Skrypty PHP nie mogą być pobierane**. Dzieje się tak dlatego, że kod PHP jest najpierw przetwarzany na serwerze WWW, a następnie wygenerowany kod HTML (lub inne dane wyjściowe) jest wysyłany do przeglądarki użytkownika. W związku z tym można pobrać tylko dane wyjściowe skryptu PHP, a nie sam skrypt.

Jak działa generowanie do HTML?
---------------------------------

Nie działa to w dosłownym znaczeniu tego słowa - będziesz surfować po stronach HTML. Skrypt PHP musi zawsze znajdować się w pliku PHP.

Do tej pory większość z Was była przyzwyczajona do tworzenia ogromnych folderów pełnych plików z rozszerzeniem **.html**. Teraz będzie to znacznie mniej plików. W skrajnym przypadku może to być pojedynczy plik.
