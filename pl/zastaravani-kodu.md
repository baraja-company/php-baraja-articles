Starzenie się kodu - jak zachować kompatybilność
================================================

> id: '7837ee7e-a150-47bf-a338-2f4c03d5bbed'
> slug:
> 	cs: zastaravani-kodu
> 	pl: starzenie-sie-kodu-jak-zachowac-kompatybilnosc
> 
> publicationDate: '2022-07-29 17:10:00'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '727b60c7258fb70e21b5acb445404db0'

Przy tworzeniu dużych systemów (np. aplikacji dla przedsiębiorstw, pakietów oprogramowania współdzielonego, bibliotek, ...), w których komunikuje się ze sobą wiele warstw i deweloperów, pojawia się problem, jak obsłużyć wydawanie nowych wersji kodu.

Przyjrzyjmy się przykładowej sytuacji, w której chcemy opracować wspólny pakiet Composera dla społeczności deweloperów.

Semantyczne wersjonowanie
--------------------

Przed rozwiązaniem problemu kompatybilności wstecz i w przód musimy dowiedzieć się, jak śledzić zmiany w oprogramowaniu. Obecnie (2022) najlepszym sposobem na wersjonowanie wszystkich zmian jest Git. Repozytorium oprogramowania może być udostępnione np. poprzez GitHub lub GitLab. Każda zmiana oprogramowania ma unikalny identyfikator, który identyfikuje każdy commit i opisuje, co się właściwie stało.

Poniższa strategia zadziałała dobrze dla mnie podczas rozwijania bibliotek:

Na początku rozwoju, początkowy commit jest tworzony w gałęzi `master` (lub `main`), gdzie jest zaangażowana bazowa struktura plików.

Dla każdego nowego żądania tworzona jest osobna gałąź z master, w której można pracować. Gdy zmiana jest gotowa, do mastera wysyłany jest merge request w postaci `Pull request`. Nad żądaniem przeprowadzany jest code review i jeśli wszystko jest w porządku, zmiana jest łączona z masterem.

Jeśli oddział zawiera zmianę niekompatybilną wstecz (BC break, od `Back Compatibility Break`), musi to być odpowiednio oznaczone. Sposób oznaczania przerw BC omówiono w kolejnych rozdziałach.

Wersja produkcyjna biblioteki jest następnie znakowana przy użyciu znaczników o następującej strukturze (na podstawie **Semantic Versioning 2.0.0**):

Numer wersji zapisujemy w formacie `MAJOR.MINOR.PATCH`. Inkrementacja numerów wersji odbywa się w następujący sposób:

- `MAJOR` - gdy następuje zmiana, która nie jest wstecznie kompatybilna z innymi (API)
- `MINOR` - gdy funkcjonalność jest dodawana przy zachowaniu kompatybilności wstecznej
- `PATCH` - kiedy błąd jest naprawiony i zachowana jest kompatybilność wsteczna

Dzięki wykorzystaniu przedpremierowych wydań i dodaniu metadanych możliwe jest dopracowanie informacji. Na przykład: `1.0.0-alpha`, `1.0.1-beta+2`.

Więcej o semantycznym wersjonowaniu można przeczytać na oficjalnej stronie: https://semver.org.

Kompatybilność wstecz i w przód
-------------------------------

Projektując oprogramowanie, należy zawsze myśleć o **kompatybilności wstecznej** (nowe funkcje i zmiany muszą być kompatybilne ze starym kodem), a w niektórych przypadkach o **kompatybilności w przód** (obecne funkcje muszą być kompatybilne z przyszłymi zmianami interfejsu).

Właściwe wykonanie obu zadań jest bardzo trudne. Nie zawsze jest możliwe wprowadzenie zmiany bez naruszenia kompatybilności.

Wprowadzając zmiany, należy zawsze postępować etapami i dać użytkownikom wystarczająco dużo czasu na reakcję na zmiany.

W kolejnych częściach opisano, jak należy o tym myśleć.

Etap 1: Oznaczanie funkcji jako przestarzałej
--------------------------------------

Podstawowym typem zagrożenia kompatybilności jest usunięcie lub zmiana nazwy cechy, która istniała w przeszłości. Najczęściej dzieje się tak dlatego, że zmieniły się argumenty, które funkcja akceptuje, lub jest to stara logika, która powinna być obsługiwana inaczej w nowy sposób.

W pierwszym etapie stare części kodu powinny być oznaczone jako zdeprecjonowane, ale nie zmienione w żaden sposób.

W PHP istnieje do tego adnotacja `@deprecated`, która powinna być napisana bezpośrednio nad metodami, funkcjami, właściwościami, zmiennymi, stałymi i ogólnie całym zdeprecjonowanym kodem.

Dobrą praktyką jest również napisanie powodu, dla którego dana rzecz jest deprecjonowana i jak zostanie zmieniona w przyszłości. Na przykład podaj nazwę nowej funkcji lub sposobu jej wykorzystania.

Przykład z prawdziwego zdarzenia, w którym oznaczenie kodu jest przestarzałe: Stałe zostaną usunięte, lepiej użyć wbudowanego Enum (BC break ze względu na migrację do nowszej wersji PHP):

```php
class OrderNotification
{
	/** @deprecated od 2022-05-24, użyj enum OrderNotificationType */.
	public const
		TYPE_EMAIL = 'email',
		TYPE_SMS = 'tekst';
```

Adnotacja `@deprecated` spowoduje jedynie ciche ostrzeżenie dla IDE (narzędzia programistycznego) i narzędzi kompilacji. To niczego nie łamie.

Faza 2: Wywołanie nowej metody/logiki
--------------------------------------

W drugiej fazie zastępujemy starą implementację nową, ale używamy nowej metody w starej implementacji. Pomoże to zachować zgodność interfejsu bez zauważenia przez użytkownika.

Przykład: metoda jest zdeprecjonowana, ponieważ zamiast niej utworzono nową usługę statyczną. Ponieważ ktoś może go użyć, jest po prostu oznaczony jako zdeprecjonowany i wewnętrznie nazywa nową implementację. Deweloper może zasadniczo założyć, że w przyszłości metoda zostanie całkowicie usunięta.

```php
/** @deprecated od 2021-09-11 zamiast tego użyj Ip::get(). */
public static function userIp(): string
{
	return Ip::get();
}
```

Faza 3: Zmiana adnotacji dla analizy statycznej
-------------------------------------------

Jeśli używasz statycznej analizy, takiej jak PhpStan (gorąco polecam!), dobrym pomysłem jest najpierw przepisanie adnotacji PHPDoc przed faktyczną zmianą typów danych. Analiza statyczna powiadomi użytkownika, że coś jest zepsute, ale runtime pozostanie nietknięty.

Etap 4: Wyrzucenie wypowiedzenia
-----------------------

W czwartej fazie wywoływana jest nowa metoda i jednocześnie rzucany jest błąd poziomu `note`. Aplikacja nadal działa, po prostu zaczyna stopniowo przechowywać informacje w dzienniku systemowym, że funkcja jest deprecjonowana i zostanie zmieniona lub usunięta. Teraz będziemy aktywnie ostrzegać o tego typu zmianach. Deweloper będzie widział błędy podczas rozwoju lub kompilacji.

```php
/** @deprecated od 2021-05-01, zamiast tego użyj UserMetaManager. */
public function getMeta(int $userId, string $key): ?string
{
	trigger_error(__METHOD__ . ': Ta metoda jest zdeprecjonowana, zamiast niej użyj UserMetaManager.');
	return $this->userMetaManager->get($userId, $key);
}
```

Etap 5: Wyrzucenie wyjątku
------------------------

Zalecam rzucenie jednego z fatalnych wyjątków przed całkowitym usunięciem metody. Jest to szczególnie ważne, ponieważ aplikacja zostanie całkowicie zatrzymana, a błąd nie może zostać zignorowany. W przeciwieństwie do całkowitego usunięcia kodu, użytkownik zostanie powiadomiony o tym, co faktycznie się stało i może łatwo naprawić błąd.

Etap 6: Całkowite usunięcie kodu
-----------------------------

W ostatnim etapie stary kod zostanie całkowicie usunięty. Jeśli jakikolwiek użytkownik nie naprawił zależności, jego aplikacja zostanie zepsuta.

Poważne złamania BC w newralgicznych miejscach powinny być zawsze robione w następnym `MAJOR` release i powinny być zaznaczone przynajmniej jeden `MAJOR` release wcześniej poprzez wrzucenie powiadomienia. Jeśli tego nie zrobisz, aktualizacja biblioteki będzie niezwykle trudna.
