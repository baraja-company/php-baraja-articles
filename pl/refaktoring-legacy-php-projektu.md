Refaktoryzacja starszego projektu PHP - jak nadrobić dług technologiczny
========================================================================

> id: '8f37305a-e9dd-4b1e-9f53-04f0fca648f7'
> slug:
> 	cs: refaktoring-legacy-php-projektu
> 	pl: refaktoryzacja-starszego-projektu-php---jak-nadrobic-dlug-technologiczny
> 
> publicationDate: '2021-05-11 20:45:00'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: b13673b23a86bc82a88636e4a9464b4b

Konsultując się z doświadczonymi i kompetentnymi właścicielami projektów, często spotykam się z pytaniem o długoterminową trwałość projektu cyfrowego. Wiele dużych projektów, które trwają dłużej niż 3 lata, staje się przestarzałych wewnętrznie i nie nadaje się już do realizacji - wyobraźmy sobie teraz zespół programistów o różnym poziomie wiedzy, doświadczenia i, co najważniejsze, pracowitości.

Aby utrzymać projekt cyfrowy na najwyższym poziomie technicznym, należy:

- **Kompetentni programiści i kierownik projektu**, którzy mają świetną komunikację i każdy wie, co robi i jak to wpływa na innych,
- **Narzędzia funkcjonalne i przepływ pracy**, które zna cały zespół i odpowiednio dostosowuje swoją pracę do innych,
- **Zadania zautomatyzowane**, które stanowią codzienną rutynę, o której tak łatwo zapomnieć, są niezwykle ważne.

Jeśli pracujesz nad dużym projektem, nie masz łatwo. Ważne jest, aby robić rzeczy dobrze, ale jeszcze ważniejsze jest, aby robić rzeczy właściwe.

Czym jest refaktoryzacja i dlaczego jest potrzebna
---------------------------------------

Koncepcja refaktoryzacji obejmuje zestaw czynności, które modyfikują wygląd i wewnętrzną logikę kodu programu bez wpływu na otaczające środowisko. Można myśleć o procesie refaktoryzacji jak o świątecznym sprzątaniu - wszystko pozostaje takie samo, ale jest o wiele lepiej zorganizowane, a niepotrzebne rzeczy są wyrzucane. W przypadku refaktoryzacji należy pamiętać, że nie jest to jednorazowe wydarzenie, ale długotrwały proces, który należy powtarzać w regularnych cyklach. Jeśli tego nie zrobisz, możesz narazić się na różnego rodzaju dziwne błędy, straty finansowe, problemy z wdrożeniem i płaczem.

Jeśli uda Ci się utrzymać stabilność i aktualność projektu, zyskasz wiele korzyści:

- Projekt będzie **bezpieczniejszy**. W używanych przez użytkownika bibliotekach regularnie pojawiają się łaty na błędy i luki w zabezpieczeniach. Należy zająć się bezpieczeństwem - jest to jedna z podstawowych funkcji życiowych zdrowego projektu.
- Kod i architektura wewnętrzna staną się prostsze i lepiej przemyślane. Będziesz w stanie lepiej wyszukiwać błędy, a wielu z nich można nawet uniknąć.
- Mając uproszczony kod, można również zatrudnić młodszych programistów, aby <a href="https://www.youtube.com/watch?v=1wZtdIb-Ppk">znacznie zmniejszyć</a> całkowity budżet.
- Może się okazać, że niektóre rzeczy są zbyt skomplikowane i nadmiernie złożone - projekt zostanie ogólnie uproszczony.

Aby utrzymać się na właściwym kursie w przypadku dużego projektu cyfrowego, trzeba biegać. Ale chcesz się rozwijać.

Jak bezpiecznie refaktoryzować
-------------------------

Każdy refaktoring to duży zakład, który może się nie opłacić.

Zawsze dbam o stabilne środowisko na długo przed rozpoczęciem refaktoryzacji.

Dla stabilności szczególnie potrzebne jest **funkcjonalne rejestrowanie błędów**. Do prostych projektów wystarczy program <a href="https://tracy.nette.org">Tracy</a>, który rejestruje błędy w pliku HTML. Do bardziej zaawansowanych projektów można wykorzystać takie narzędzia, jak <a href="https://sentry.io/welcome/">Sentry</a> lub <a href="https://rollbar.com">Rollbar</a>. Osobiście używam programu Tracy do wszystkich projektów, inne narzędzia zależą od rodzaju projektu.

Mniej więcej miesiąc przed refaktoryzacją zaczynam śledzić błędy i tworzę arkusz znanych błędów, na których będę mógł się później skupić. Zawsze warto wiedzieć, jakie błędy zostały wprowadzone przez refaktoryzację, a jakie projekt zawierał już w przeszłości. Pozwoli to lepiej bronić wyników pracy przed klientem.

Gdy dzięki dziennikowi wiem, że pracuję we względnie stabilnym środowisku, mogę przystąpić do ciężkiej pracy. Inne rubryki zawierają opisy metod w zależności od tego, jak duże ryzyko się za nimi kryje.

Poprawianie stylu kodowania i standardu kodowania
-----------------------------------

W większości projektów nie stosuje się spójnego stylu formatowania kodu. Jest to duży błąd. Dobrze napisany projekt wygląda jak projekt jednej osoby, w którym wszystkie rzeczy są napisane tak samo.

Podstawowe błędy formatowania są dobrze korygowane za pomocą narzędzia <a href="https://doc.nette.org/en/3.1/code-checker">Nette Code Checker</a>, które regularnie uruchamiam w całym projekcie.

Z kolei styl kodowania obejmuje sposób formatowania kodu oraz wcięcia poszczególnych wyrażeń języka. Standard <a href="https://www.php-fig.org/psr/psr-12/">PSR-12</a> jest bardzo często używany w świecie PHP, a wiele innych standardów jest na nim opartych. Zalecam stosowanie.

Niektóre projekty w niezręczny sposób łączą <a href="/spaces-and-tabs">tabularze i spacje</a>. Aby skutecznie zapobiegać łamaniu konwencji białych spacji (i innym błędom), należy utworzyć plik `.editorconfig` w katalogu głównym projektu, który będzie mówił edytorowi, jak ma formatować nowo napisany kod.

Osobiście używam takiej konfiguracji:

```txt
root = true

[*]
charset = utf-8
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true

[*.{php,phpt}]
indent_style = tab
indent_size = 4
```

Popraw także wszystkie <a href="/apostrofy-i-przypisy">przypisy do apostrofów</a>. Można to zrobić automatycznie.

Możesz także automatycznie sprawdzać formatowanie swojego kodu, przygotowałem w tym celu <a href="https://github.com/baraja-core/sandbox/blob/0db1f41068b6cedf79500c6a8b0ac26eae94a8eb/.github/workflows/coding-style.yml">w pełni funkcjonalne demo na GitHubie</a>. Jeśli zachodzi potrzeba automatycznej korekty kodu, można to zrobić za pomocą tego samego narzędzia. PhpStorm jest również bardzo dobry w automatycznym poprawianiu kodu bezpośrednio, nawet masowo w całym projekcie.

Jak poprawić styl kodowania w dużych projektach
----------------------------------------------

W przypadku dużych projektów zalecam wykonywanie automatycznego formatowania kodu po jednym module na raz, ponieważ może to spowodować powstanie ogromnej liczby konfliktów w Gicie. Dlatego najlepszą strategią jest poprawienie jak największej liczby linii za pomocą jednego dużego commitu, przepchnięcie tego commitu bezpośrednio na master, a następnie rozprowadzenie zmiany do innych gałęzi.

Jeśli konflikty są zbyt duże, sensowne jest sformatowanie kodu najpierw na oddziale głównym, a następnie ponownie na każdej dużej gałęzi, w której wprowadzono wiele zmian. Bardzo wiele zmienionych linii będzie się wzajemnie znosić (wykonaj szybkie przewijanie do przodu), dzięki czemu rozwiążesz bardzo mało konfliktów, a czasem nie rozwiążesz ich wcale.

Jeśli nic nie zrobisz, kod nadal będzie zły. Iteracyjne przepisywanie kodu przez wiele lat zdecydowanie się nie sprawdza, ponieważ każde scalanie wprowadza konieczność poprawiania dziesiątek wierszy, w których w rzeczywistości nie zaszła żadna zmiana, i niepotrzebnie komplikuje zarówno przeglądanie kodu, jak i potencjalne odwracanie zmian.

PhpStan - statyczna analiza kodu i korekcja błędów typu
------------------------------------------------------

Przed przystąpieniem do naprawiania logiki na dużą skalę bardzo ważne jest przejrzenie i poprawienie **podstawowych cech cyklu życia projektu**. Należą do nich tak oczywiste rzeczy, których oczekuje się od każdego projektu, a które jednak czasami nie są spełniane.

Na przykład:

- Wszystkie klasy, metody i funkcje muszą istnieć
- Dziedziczenie klas nie może być przerwane
- Klasy muszą implementować używany interfejs lub interfejs przodka. Jednocześnie nie wolno dziedziczyć po klasach `final`.
- Nie wolno wywoływać niebezpiecznych funkcji, takich jak `eval()`, `shell_exec()`, `var_dump()` i tak dalej. A jeśli mimo to zadzwonisz do nich, należy to wyraźnie zaznaczyć w komentarzu
- Należy zawsze wyłapywać wyjątki i nie dopuszczać do awarii całej aplikacji

Rozwiązaniem tego problemu jest zainstalowanie w projekcie <a href="https://phpstan.org">PhpStan</a> i naprawienie go **co najmniej do poziomu 1**. Tak, jest to trudne i wymaga wiele pracy. Jeśli jednak tego nie zrobisz, każdy refaktoring staje się rosyjską ruletką, a programista ma tylko nadzieję, że szkody będą jak najmniejsze.

Poziom bazowy PhpStan nie wymaga, aby typy danych i inne formalne rzeczy istniały wszędzie, na przykład, czym zajmiemy się w przyszłości. Z drugiej strony, może się zdarzyć, że funkcja lub metoda zwróci pewien typ danych, ale w informacji typehint poda coś innego.

Rektor - bezpieczna naprawa iteracyjna
-----------------------------------

Znana zasada programowania mówi, że przed dodaniem do systemu nowego błędu (np. rzucenia wyjątku) należy najpierw dodać ten błąd jako ostrzeżenie, a dopiero potem zamienić go w błąd krytyczny, jeśli nie ujawnia się przez dłuższy czas.

Tak samo jest z typami danych. Podczas refaktoryzacji nieznanego kodu pozwalam najpierw automatycznym narzędziom, takim jak <a href="https://getrector.org">Rector</a>, dodać do kodu adnotacje z komentarzami, co niczego nie psuje, ale pomaga wyjaśnić, co gdzie będzie później. Komentarze te są następnie odsłuchiwane przez program PhpStan, za pomocą którego można bezpiecznie sprawdzić, czy niczego nie zepsuliśmy i czy jest to bezpieczna modyfikacja.

Na ogół dodaję komentarze do właściwości i argumentów w jednym commitze, potem czekam może miesiąc, a gdy wszystko działa w dłuższej perspektywie, uciekam się do przepisania na stałe typy danych w miejscach, w których nie było problemów w dłuższej perspektywie.

Zobacz artykuł <a href="https://tomasvotruba.com/blog/2019/07/29/how-we-completed-thousands-of-missing-var-annotations-in-a-day/">Jak w ciągu jednego dnia uzupełniliśmy tysiące brakujących adnotacji @var</a>.

Więcej informacji na temat <a href="https://github.com/rectorphp/rector">Rector można znaleźć na GitHubie</a>.

Aktualizowanie zależności
----------------------

Przed przystąpieniem do aktualizacji zależności w pakietach, a nawet wersji PHP, ważne jest, aby dowiedzieć się, jak używać <a href="/github-actions-best-ci-for-2021">testów automatycznych</a>.

Jeśli testy przebiegły pomyślnie, możemy przejść do narzędzia <a href="/composer">Composer</a>, aby przeprowadzić aktualizację. Jeśli możesz, skorzystaj z pomocy robota <a href="https://dependabot.com">Dependabot</a>, który automatycznie aktualizuje plik `composer.json` twojego projektu i może sprawdzić zmiany w kompatybilności.

W przypadku dużej liczby zmian należy zawsze wprowadzać je powoli i zwiększać ich liczbę wersja po wersji. Nigdy nie należy aktualizować wielu pakietów jednocześnie. Po każdej aktualizacji należy przeskanować cały projekt za pomocą programu PhpStan i usunąć błędy. Jest to długi proces, który trwa kilka godzin, ale stawka jest wysoka.

Co dalej?
-------

Kolejne kroki są indywidualne, w zależności od rodzaju i statusu projektu. Ogólnie rzecz biorąc, dobrze zaprojektowany kod napisany przez starszych programistów jest utrzymywany o rzędy wielkości lepiej niż kod kupiony po taniości od juniora pracującego w agencji medialnej, dla której tworzenie stron internetowych stanowi niejako dodatek.

Trzymamy kciuki! Będzie ciężko, ale dacie sobie radę.
