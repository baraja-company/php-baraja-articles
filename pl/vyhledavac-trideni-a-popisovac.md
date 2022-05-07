Algorytm wyszukiwarki internetowej - sortowanie i deskryptor
============================================================

> id: ca4aaac5-0cd8-41db-b860-c32661c43d82
> slug:
> 	cs: vyhledavac-trideni-a-popisovac
> 	pl: algorytm-wyszukiwarki-internetowej---sortowanie-i-deskryptor
> 
> publicationDate: '2016-09-11 13:00:00'
> mainCategoryId: '367f936c-073f-44bd-b399-30738e93137a'
> sourceContentHash: '541310dfdc96b75a320c7cad1b03e734'

W tej lekcji poświęconej zasadom działania wyszukiwarki internetowej dowiemy się, w jaki sposób wyszukiwarka sortuje, opisuje i ocenia wyniki.

Sortowanie wyników
----------------

Wyobraźmy sobie gotową beczkę, która znajduje się aktualnie na serwerze wyszukiwania. Otrzymaliśmy pierwsze zapytanie od użytkownika i teraz musimy wykonać pierwsze "zgrubne" sortowanie, które będzie dalej udoskonalane.

Przyjrzyjmy się następującemu przykładowemu zapytaniu wejściowemu:

```txt
[O (and) pejskovi (and) a (and) kočičce]
```

Tak, jest to forma, w której serwer wyszukiwania otrzymuje od użytkownika przetworzone zapytanie, a teraz czeka na zwrócenie wyniku. W sumie mamy na to mniej niż `300 ms`, więc szybko :)

W pierwszym kroku otrzymujemy beczki pełne identyfikatorów przyszłych wyników (zwanych również kandydatami) oraz operacji do wykonania. Ta pobrana beczka może w niektórych przypadkach nie być kompletna, ale zawiera tylko te wartości, które serwer zdołał odczytać z dysku w pewnym z góry określonym przedziale czasu (zwanym też **timeout**). Na przykład otrzymujemy następujące serie liczb (które są z góry częściowo posortowane):

| Beczki | Dokumenty |
|-------|---------|
| o | 2,3,4,5,9,12,15,19,23,26,28,34,39,42,58,67,83,91,115 |
| Pies | 5,19,23,42,46,58 |
| a | 2,3,4,5,9,12,15,19,23,26,28,34,39,42,58,67,83,91,115 |
| cipki | 9,19,42,57,58,62,68,83 |

Teraz wykonujemy zgrubne przecięcie wszystkich zbiorów i otrzymujemy listę identyfikatorów dokumentów, które są takie same we wszystkich beczkach. W praktyce przechodzimy przez najkrótszą listę i przeszukujemy pozostałe.

Wspólne identyfikatory wszystkich beczek to "19, 42, 58", więc strona z przyszłymi wynikami zawiera 3 elementy w jakiejś, nieznanej jeszcze, kolejności. Nadal traktujemy dokumenty jako identyfikatory, ponieważ pozwala to zaoszczędzić ogromną ilość danych. W wynikach wyszukiwania zazwyczaj nie chcemy podawać użytkownikowi numerów dokumentów, ale raczej ich tytuł (nagłówek), opis, adres URL i inne informacje. Jest to zapewniane przez komponent "deskryptor".

Deskryptor
---------

Z poprzedniego rozdziału pozostała nam lista kandydatów do przyszłych wyników. Pobieramy je w postaci posortowanej pod względem systemowym, tzn. tak, jak wyszukiwarka uszeregowała je sekwencyjnie w indeksie. W tym momencie nie możemy już wykonywać sekwencyjnego odczytu dużych danych, lecz musimy sekwencyjnie przemierzać relacyjną bazę danych w celu znalezienia dodatkowych informacji dla innego rodzaju sortowania, bardziej zrozumiałego dla użytkownika.

Na przykład wycinek bazy danych może wyglądać następująco:


| ID | Tytuł | Etykieta | URL | Ranga |
|----|---------|---------|-----|------|
| 19 | Josef Čapek: Opowieść o psie i kocie | To było wtedy, kiedy pies i kot jeszcze razem gospodarowali; mieli swój mały domek przy lesie i mieszkali tam razem, i chcieli robić wszystko tak, jak robią to wielcy ludzie | www.troglodytarium.cz/webz/axf/.../Capek_J_Pejsek_a_kocicka.htm | 72 |
| W porządku," powiedział pies, a kot wziął mydło i garnek z wodą, uklęknął na podłodze, wziął psa jako szczotkę i wyszorował całą podłogę razem z psem. Podłoga była cała mokra | www.capek.narod.ru/povidani.htm | 86 |
| 58 | Jak pies i kot upiekli ciasto na święto| Jutro było święto psa i urodziny kota. Dzieci wiedziały o tym i chciały zrobić niespodziankę psu i kotu z okazji ich urodzin. Zastanawiali się, co mogą zrobić dla psa i | a.da.mek.sweb.cz/capek.j/dort.htm | 34 |

Obliczanie istotności
-----------------

Ostatnim krokiem jest obliczenie trafności odpowiedzi. W przypadku większości wyników wystarczy przedstawić trafność jako pojedynczą liczbę z ustalonym promieniem, według którego wyniki będą sortowane. Według rangi wyniki są ponownie "z grubsza" sortowane do kilku grup (liczba grup zależy od wariancji rang) i te grupy są dalej opracowywane.

Grupy o najwyższej randze są brane pod uwagę w pierwszej kolejności, często wystarcza to do posortowania pierwszej strony wyników i nie musimy zajmować się niczym innym. Jeśli jednak kilka różnych dokumentów ma tę samą rangę, musimy przeprowadzić bardziej szczegółową analizę i obliczyć dodatkowe rangi pomocnicze, które pomogą w uszeregowaniu strony. Dodatkową rangę może stanowić na przykład jakość linków, wiek dokumentu, długość tytułu, wygląd i sposób działania adresu URL oraz wiele innych czynników.

Zwracanie wyników do użytkownika
---------------------------

Hurra! Mamy już wyniki i teraz pozostaje tylko wyrenderować stronę dla użytkownika. Pakiet wyników jest przekazywany z powrotem do serwera, który dopasowuje wyniki do przygotowanego wcześniej szablonu, umieszcza wokół strony banery reklamowe i odsyła stronę do użytkownika. Jednocześnie może też wykonywać inne operacje pomocnicze, takie jak buforowanie wyników. Jeśli ktoś inny wyszuka to samo zapytanie w jakimś momencie w najbliższej przyszłości (na przykład w ciągu ostatniej godziny), wyniki nie zostaną ponownie wyszukane, a jedynie odczytane z pamięci tymczasowej - co często pomaga poprawić ogólną wydajność wyszukiwarki.

Wnioski i wgląd w semantykę
---------------------------

Celem tego artykułu było opisanie podstawowych zasad wewnętrznego działania wyszukiwarek i tego, w jaki sposób udaje im się wyszukiwać teoretycznie nieograniczoną liczbę dokumentów w rozsądnym czasie. Są to ogromne optymalizacje matematyczne, które były opracowywane przez wiele lat przez najlepsze zespoły programistów.

Pełny opis szczegółów wykracza poza zakres niniejszego opracowania. Całą sprawę komplikuje również fakt, że większość pozostałych kroków nie jest szczegółowo opisana przez żadną z wyszukiwarek, ponieważ stanowią one ich tajemnicę handlową.

Należy także zauważyć, że wyszukiwarki nadal nie rozumieją zawartości stron ani znaczenia wyników, a ich działanie to jedynie obliczenia statystyczne oparte na mocy obliczeniowej i zdolności ludzi do umieszczania linków do tekstów wysokiej jakości. System ten jest także dość podatny na ataki (weźmy przykład "bomby Google", gdzie podstępny webmaster narzuca treść na zapytanie, które jest nieistotne).

Problem ten powinna częściowo rozwiązać sztuczna inteligencja, którą stopniowo starają się zintegrować wszystkie wyszukiwarki. Jest to jednak wciąż odległa przyszłość, ponieważ nie jest to łatwe zadanie, a w większości przypadków jest to tylko kwestia udoskonalenia metod obliczeń statystycznych. Przykładem niech będzie wyszukiwanie grafów Google, które stara się bezpośrednio odpowiedzieć na zapytanie (choć w swojej wewnętrznej strukturze nadal przeszukuje tylko predefiniowaną bazę wiedzy).

Dlatego następnym razem, gdy zadasz zapytanie swojej ulubionej wyszukiwarce, przypomnij sobie, ile pracy musiała wykonać wyszukiwarka i ile TB danych musiała odczytać z dysku, aby znaleźć 10 najlepszych dokumentów dla Twojego zapytania.
