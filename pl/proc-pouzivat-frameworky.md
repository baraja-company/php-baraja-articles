Dlaczego i jak używać frameworków i bibliotek?
==============================================

> id: '5ea6cdde-f67c-4f3f-8871-37b27ee8e23a'
> slug:
> 	cs: proc-pouzivat-frameworky
> 	pl: dlaczego-i-jak-uzywac-frameworkow-i-bibliotek
> 
> publicationDate: '2020-02-16 22:13:46'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '0926b7ec4f59904c008388431ff22eb5'

Znany jest dowcip o tym, że programiści zaczynają używać frameworków dopiero wtedy, gdy napiszą własny i stwierdzą, że nie ma on sensu. Najzabawniejsze jest to, że to prawda. Sam tego doświadczyłem. Nawet dwa razy.

Jednak na stronie głównej <a href="https://nette.org">Nette</a> jest napisane:

> Prawdziwi programiści **nie używają frameworków**. Piszą oni aplikacje internetowe za pomocą wiersza poleceń bezpośrednio na serwer. To jest hołd dla nich. Pozostałym użytkownikom Nette znacznie ułatwi i uprzyjemni pracę.

Rola frameworków w tworzeniu aplikacji
-----------------------------------

Jestem pewien, że sam o tym wiesz:

Wpadasz na pomysł świetnego projektu, więc zaczynasz pisać kod od podstaw bezpośrednio w czystym PHP... po kilku godzinach okazuje się, że większość pracy jest wciąż powtarzalna i że rozwiązujesz podstawowe problemy systemowe. Na przykład jak połączyć się z bazą danych, jak wyświetlić formularz, jak sprawdzić poprawność danych, jak wysłać wiadomość e-mail i wiele innych.

Prawdopodobnie użytkownik napisze własne funkcje, które będzie wywoływał w celu wykonania tych zadań. Jeśli piszesz kilka projektów naraz, zaczniesz udostępniać te funkcje między projektami w jednym dużym, niechlujnym pliku i będziesz je stale poprawiać, gdy będą potrzebne.

A gdy funkcje są już na takim etapie rozwoju, że za każdym razem można na nich budować nowy projekt i od razu się rozwijać, wtedy można mówić o napisaniu własnego frameworka lub przynajmniej biblioteki.

Czym jest i do czego służy rama
-------------------------

Jak już pokazaliśmy na praktycznym przykładzie z życia, framework to zbiór wielu funkcji (ale najlepiej klas i obiektów), które oszczędzają programiście pracy. Podczas pracy nad programem nie musi zastanawiać się, jak połączyć się z bazą danych, ale wie, że jest to już gdzieś zaprogramowane i "po prostu działa".

Gotowe frameworki są więc efektem pracy setek ludzi, którzy przez długi czas stworzyli tysiące aplikacji, aby móc korzystać z jednego rozwiązania i jednego ekosystemu przy realizacji nowych projektów.

Wraz z frameworkiem otrzymujemy również zestaw **najlepszych praktyk**, czyli sposobów rozwiązywania problemów bez konieczności zastanawiania się nad tym. Jeśli napotkasz jakiś problem, prawdopodobnie ktoś już go rozwiązał przed Tobą, a zawsze lepiej znaleźć rozwiązanie w dokumentacji, niż samemu zmagać się z nim w skomplikowany sposób.

Programowanie abstrakcyjne i zasada hermetyzacji
---------------------------------------------

Dobrze zaprojektowane frameworki, takie jak Nette, pozwalają na programowanie na **bardzo wysokim poziomie abstrakcji**.

W ten sposób programista (użytkownik frameworka) nie musi rozumieć, co się dzieje wewnętrznie i jak dokładnie działają poszczególne komponenty, co pozwala zaoszczędzić wiele czasu i wysiłku. Może on skupić się na rozwiązywaniu rzeczywistych problemów związanych z jego aplikacją i bardzo szybko osiągać wyniki.

Sam David Grudl (autor frameworka Nette) powiedział kiedyś, że zaprojektował Nette, aby móc zaprogramować stronę internetową dla klienta w ciągu jednej nocy, kiedy ten wraca późno z pubu i musi ją uruchomić rano. Wynika to głównie z faktu, że programista jest całkowicie odsunięty od spraw technicznych.

Aby to działało i aby programista mógł po prostu korzystać z gotowego frameworka jako całości, bardzo ważne jest poprawne użycie <a href="/encapsulation">zasady enkapsulacji</a>.
