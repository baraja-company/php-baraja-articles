Włączanie zabezpieczeń w PHP i dołączanie plików
================================================

> id: '820f8de6-ff1e-406c-8fe5-95080642656f'
> slug:
> 	cs: bezpecnost-include
> 	pl: wlaczanie-zabezpieczen-w-php-i-dolaczanie-plikow
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '1207d637c20fcb5e8f609be2eb449135'

Często zdarza się, że chcemy dołączyć do strony plik, który mamy zapisany na dysku w innym miejscu. Jeśli wpiszemy jego dokładną nazwę bezpośrednio do funkcji dołączania, nie ma się czym martwić.

Bezpieczne dołączanie pliku
--------------------------

```php
include 'menu.html';
```

Poprzedni zapis jest całkowicie bezpieczny, ponieważ zawsze montujemy ten sam plik. W takim przypadku nie może wystąpić błąd zabezpieczeń. Jedyny problem, jaki może wystąpić, to brak pliku **menu.html**, co spowoduje wyświetlenie komunikatu ostrzegawczego (który prawdopodobnie i tak się nie pojawi), ale taka sytuacja jest rzadka, ponieważ zwykle dołączamy plik, którego istnienie jest praktycznie pewne.

Dołączanie pliku według wzoru
--------------------------

Ale co zrobić, jeśli na przykład chcemy dołączyć artykuły do prostej witryny z treścią, takiej jak ta? W tej witrynie mam fizyczny folder, w którym artykuły są przechowywane w formacie HTML i dołączam je bezpośrednio do kodu źródłowego.

Jednak samo podłączenie to nie wszystko! Początkujący użytkownik może w ten sposób nazywać poszczególne artykuły:

```php
include 'artykuły/' . $_GET['Artykuł'] . '.html';
```

> Ale to jest bardzo niebezpieczne. Atakujący może przekazać łącze do innego katalogu, używając `../` lub czegoś podobnego jako nazwy artykułu, a czasami możliwe jest pozbycie się końcówki przez przekazanie na końcu bajtu null. Powinieneś przynajmniej użyć funkcji `basename()`, ale lepiej dopuścić tylko wartości z białej listy.

Dlaczego nie wczytywać nieistotnych plików?
--------------------------

Często nie mamy nic przeciwko załadowaniu nieprawidłowego (nieoczekiwanego) pliku - to wina użytkownika, który zażądał strony, której w rzeczywistości nie chce, ale mogą być sytuacje, w których ma to znaczenie. W szczególności:

- Użytkownik ładuje plik, do którego nie ma dostępu publiczność, a jedynie serwer może się do niego dostać.
- Załadowanie innego skryptu PHP może wywołać nieoczekiwane działanie lub komunikat o błędzie, który może być wskazówką co do sposobu działania witryny i pomóc w przeprowadzeniu dalszych ataków.
- Wczytanie innego pliku może nie tylko spowodować jego dodanie do dokumentu, ale także jego uruchomienie.

Biała lista i sprawdzanie poprawności danych wejściowych
--------------------------

Jeśli nie mamy możliwości walidacji danych wejściowych w jakiś bezpieczny sposób (np. z białej listy), nie powinniśmy polegać na uczciwości użytkownika i bronić skryptów, przynajmniej na poziomie PHP.

Pierwszą ważną rzeczą jest umieszczenie wszystkich wczytanych plików w tym samym folderze (katalogu) i wyłączenie niektórych niebezpiecznych znaków, zwłaszcza ukośnika i kropki. Uniemożliwi to dostęp do innych folderów zawierających potencjalnie zagrożone pliki. Niebezpieczne znaki można również wyłączyć, usuwając je z łańcucha wejściowego.

```php
$load = '../index'; // to wejście może być potencjalnie niebezpieczne
$load = strtr($load, './', ''); // usuwa wszystkie kropki i ukośniki z ciągu znaków
include $load .'.html';
```

Uruchomienie plików?
--------------------------

Należy pamiętać, że konstrukcja **include** wykonuje pliki po podłączeniu w taki sam sposób, jak gdyby były one kodem PHP, więc dobrze jest dopuścić taką możliwość.

Często jednak będziemy dołączać pliki, które nie muszą być potem wykonywane, a interesuje nas tylko zapisany tekst (zawartość) w postaci łańcucha. Dlatego możemy wczytać plik do zmiennej i pracować z nim jako z łańcuchem znaków, co jest całkiem bezpieczne.

```php
$load = '../index'; // to wejście może być potencjalnie niebezpieczne
$load = strtr($load, './', ''); // usuwa wszystkie kropki i ukośniki z ciągu znaków
$file = file_get_contents($load . '.html'); // ładowanie zawartości do zmiennej
echo $file; // wyrzuć zawartość pliku
```

Na pierwszy rzut oka rozwiązanie to wygląda interesująco i bezpiecznie - i jest bezpieczne. Nawet jeśli użytkownikowi uda się wywołać plik PHP, nie zostanie on nigdy uruchomiony. Może jednak wyświetlać je (tzn. ich kod źródłowy), na co należy uważać.

Rozpoznawanie żądanego pliku ze skryptu
--------------------------

Nie ma na to jednoznacznej instrukcji, każdy musi to zrobić sam, zgodnie z potrzebami scenariusza. Na przykład rozpoznaję artykuł spośród innych plików po tym, że zawierają one nagłówek o rozmiarze H1. Jeśli więc ktoś wczyta plik, w którym nie ma nagłówka, nic nie wyświetli, a strona zakończy się komunikatem o błędzie. Zawsze ważne jest znalezienie jakiejś unikalnej cechy, którą mają tylko te pliki, których nie mają inne pliki, i dopiero wtedy można przejść dalej.

Wniosek
--------------------------

Mimo że sprawdzanie poprawności i ładowanie plików jest stosunkowo proste, wielu początkujących użytkowników wciąż popełnia błędy - i będzie je popełniać nadal. Najważniejsze jest, aby dobrze zrozumieć znaczenie tego, co ładujemy, i odróżnić treści, które chcemy uzyskać, od pozostałych. A co najważniejsze, należy pracować z treścią jako ciągiem znaków i nie wczytywać jej bezpośrednio na stronę.
