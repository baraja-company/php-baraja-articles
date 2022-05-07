Test sprawdzający podstawową wiedzę o sieci Nette
=================================================

> id: d137c177-503c-4e84-8709-0e65b0ce6060
> slug:
> 	cs: test-nette-zaklady
> 	pl: test-sprawdzajacy-podstawowa-wiedze-o-sieci-nette
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '28a2aef7-7490-43a9-add7-80d8a051f8a9'
> sourceContentHash: '66fb122b33aa8ce41158345bb01769ab'

Próg sukcesu: 15 punktów

*Za każdą poprawną odpowiedź na pytanie otrzymasz 1 punkt. Za każdą błędną odpowiedź na pytanie nie otrzymasz nic. Jeśli odpowiedź jest tylko częściowa (i nie da się na jej podstawie zaprogramować zadania), pytanie liczy się jako niepoprawne (nie da się uzyskać połowy punktu). Jeśli rozwiązanie zawiera błąd w zabezpieczeniach lub literówkę w kodzie, odpowiedź jest uznawana za niepoprawną, ponieważ nie zostanie uruchomiona.

-----------

1 Wyjaśnij różnicę między pętlami `for`, `while` i `foreach`. Dla każdego z nich podaj jeden konkretny przykład zastosowania, który jasno pokazuje jego główną zaletę.


2. mamy zmienną, o której prawie nic nie wiemy (znamy tylko jej nazwę). Jak możemy zobaczyć jego zawartość? Na przykład nosi on nazwę `$data`.


Napisz poniższe polecenia, aby pracować z repozytorium Git:
- Pobierz najnowsze zmiany z serwera
- tag pliku `Statistic.php`.
- oznaczanie wszystkich plików w projekcie
- oznaczenie wszystkich plików w katalogu `cron`.
- wprowadzanie zmian z komunikatem "My commit".
- wysłanie zobowiązania do serwera


W zmiennej umieśćmy łańcuch tekstowy. Podaj przykład funkcji służącej do obliczania sumy kontrolnej.


5. napisz fragment kodu, który tworzy akcję `delete` w `Presenter`, która przyjmuje identyfikator elementu jako liczbę całkowitą i usuwa wiersz z tabeli `question` według podanego identyfikatora. Po pomyślnym usunięciu zostanie wyświetlony komunikat "Pytanie zostało usunięte" i nastąpi przekierowanie do akcji `list`.

Pod znakiem zapytania o dodatkowy punkt: Jeśli z jakiegoś powodu usuwanie nie powiedzie się, nie wyrzuca błędu krytycznego, ale informuje o tym użytkownika za pomocą komunikatu (komunikat flash).

Po utworzeniu formularza Nette staje się on komponentem. Co to jest składnik Nette?

7. muszę utworzyć prosty formularz Nette, który będzie wstawiał rekord do tabeli `question` zawierającej listę pytań. Struktura tabeli jest następująca:

| Kolumna | Właściwości |
|-----------|----------------------------------|
| identyfikator | int(8), unsigned, auto increment |
| pytanie | varchar(255) |
| is_active | tinyint(1), unsigned, wartość domyślna: 1

Utwórz odpowiednie pola formularza, aby wstawić nowy wiersz do tej tabeli. Po wstawieniu rekordu musi zostać odpalony komunikat FlashMessage informujący o pomyślnym wstawieniu rekordu + przekierowanie do edycji rekordu (akcja `edit`).

- Walidacja, czy pole formularza zostało wypełnione
- Walidacja, czy tekst pytania mieści się w varchar zgodnie ze strukturą tabeli
- Potwierdzenie, że pytanie z takim tekstem już nie istnieje
- Zdefiniuj kolejną niestandardową tabelę `group`, która będzie zawierać informacje o grupach. Podczas tworzenia pytania będzie można określić, do jakiej grupy należy dane pytanie. Konieczne będzie zorganizowanie sesji między stołami (opisz, jak to się robi i jak to będzie wyglądało).
- Jakiego makra Latte należy użyć do renderowania formularza do szablonu (render domyślny)?

W `Presenter` znajduje się formularz edycji, który jest tworzony jako komponent. Chcemy przekazać wartości domyślne z tego, co znajduje się w bazie danych, tzn. musimy pobrać dane z tabeli w jakiś wygodny sposób.
- Jak należy postępować i jakie elementy kodu są potrzebne?
- Jak przekazać do formularza identyfikator aktualnie edytowanego elementu?
- Jak ustawić wartość domyślną dla elementu formularza?
- Jak można sprawdzić, czy użytkownik próbuje edytować element, który nie istnieje w bazie danych, i jak odpowiednio go o tym poinformować?

9 Rozważmy następujące dane pobrane z bazy danych (przy użyciu zwykłej bazy danych Nette Database):

```php
$questions = $this->db->questions()->fetchAll();
```

- Jak wyświetlić tekst wszystkich pytań w postaci listy wypunktowanej?
- Jak przekazać dane z tabeli do szablonu Latte?
- Jakie makra Latte będą potrzebne do sporządzenia listy elementów? Podaj konkretną implementację wypisywania kolumn `id` i `name` w formacie:

	*1024: Jak się masz?
	*1025: Co jadłeś dzisiaj na obiad?

Wymień przykład co najmniej 3 różnych pól formularza, które są zapisane w formularzu:

```php
$form->add(tady bude příklad);
```

i dla każdego z nich wyjaśnić, do czego służy i jakie dane wyjściowe zwraca (typ danych + przykład).


Utwórzmy przesłany formularz Nette.
- W jaki sposób można przesłać wszystkie pola (nazwy i wartości)?
- Podaj przykład wyprowadzania danych z pola o nazwie `question`.
- Podaj konkretną implementację kodu, który będzie przemierzał tablicę wartości i kluczy oraz zwracał pojedynczy łańcuch zawierający całą listę kluczy i wartości, tak abyśmy mogli na przykład zapisać ten łańcuch w bazie danych lub wysłać go pocztą elektroniczną (zapisywanie i wysyłanie nie jest przedmiotem zadania i nie będzie oceniane).


Dla każdego warunku zdecyduj, czy wynik jest PRAWDĄ czy FAŁSZEM:
- `1 > 0`
- `1 == 1`
- `1 == "1"`
- `1 === "1"`
- `1 == true`
- `1 == true`
- `1 == false`.
- `1 == "1" && 1== true`


13. Jakie typy danych znamy w PHP?
- Podaj co najmniej 5 przykładów nazw typów danych
- Dla każdego przykładu wymień co najmniej 3 możliwe wartości, które mogą być przechowywane w typie danych (jeśli typ danych nie może przechowywać tylu możliwych wartości, napisz o tym).
- Dla każdego typu danych podaj typowy przykład jego zastosowania (co jest w nim przechowywane w praktyce)
- Jaka jest różnica w stanie między `==` (dwa równe) i `==` (trzy równe)?
- Wyjaśnij wadę używania `==` w warunkach i jak konkretnie `==` rozwiązuje ten problem (przykład, gdzie `==` może zawieść, a `==` ratuje sytuację)


Utwórzmy tabelę koordynacji (tabela koordynacji) zawierającą listę wszystkich koordynacji między dwiema osobami. Jeden z nich organizuje koordynację, a drugi jest gościem. Napisz selekcję do bazy danych, która zwraca wszystkie wiersze z koordynacjami, w których biorę udział (czy jestem organizatorem koordynacji, czy jestem gościem koordynacji). W tabeli znajdują się kolumny `id`, `id_user_organizer` (id organizatora), `id_user_quest` (id gościa). Mój identyfikator jest przechowywany w zwykły sposób w `Presenter`.


15. grupa pytań dotyczących Latte:
- Co to jest Latte?
- Jaka jest różnica między `zmienną`, `makro`, `filtrem` i `n:atrybutem`? Co gdzie jest stosowane?
- Jak utworzyć odwołanie `DashboardPresenter` do akcji `default`?
- Jak wygenerować link do konkretnej edycji (akcja `QuestionPresenter`, `edit`) pytania, aby przekazać ID aktualnie wylistowanego pytania? Napisz specjalny kod Latte.

Symbolicznie zapisane (próbka w PHP, przetłumacz na Latte):

```php
foreach ($questions as $question) {
   echo $question->id; // Identyfikator pytania
   echo $question->question; // tekst pytania
}
```

- Jak napisać stałą spację HTML?


16) Do czego służą usługi w Nette?
- W jaki sposób jest on inicjowany?
- Co musi robić usługa, aby można było z niej korzystać?
- Jak załadować usługę do programu Presenter?
- Na przykład, rozważmy usługę `StatisticManager`, która posiada publiczną metodę `getStatistics()`, która nie przyjmuje żadnych parametrów. W jaki sposób mogę załadować tę usługę w Prezenterze i wywołać metodę publiczną `getStatistics()` w akcji domyślnej oraz przekazać wynik do szablonu?
- Jaka jest różnica między `object`, `class`, `service`?
- Co to jest `model`, `entity` i `value object`?
- Wszystkie usługi mają głównego menedżera, który je zna i może je odebrać w tym menedżerze. Jak się nazywa ten kierownik? Jak zarejestrować w nim nową usługę?


17. neon
- Co to są pliki neonowe?
- Jakie są ich rodzaje i według czego są sortowane?
- Co zawierają pliki neonowe? Jakie dane są w nich przechowywane?
- Podaj przykład, jak zarejestrować poniższe pole (przepis jest w PHP, musisz go przetłumaczyć), aby można go było użyć jako parametru:

```php
$imageGenerator = [
   "punkty" => [
      480: [910, 30, 1845, 1150],
      600: [875, 95, 1710, 910],
      768: [975, 130, 1743, 660]
   ]
];
```

- Podaj przykład rejestracji usługi w Neonie, do której przekazujemy parametr `imageGenerator`, który zarejestrowaliśmy w poprzednim zadaniu, tak aby usługa otrzymała go w konstruktorze i mogła go wykorzystać w usłudze (w sensie konfiguracji). Dla usługi podaj przykładową implementację konstruktora tak, aby pierwszy parametr wejściowy był traktowany jako typ danych dla tablicy.


Obiekty w ogólności
- Co to są `method`, `properties` i `constants`? Jaka jest różnica między nimi?
- Zarówno metody, jak i właściwości mają 3 podstawowe stany dostępności (`public`, `private`, `protected`). Wyjaśnij różnicę i podaj konkretny przykład użycia oraz wyjaśnij, kto może zobaczyć co i kiedy.
- Mam klasę `kurs`, w której znajduje się prywatna właściwość `bieżącyKurs`, w której przechowywany jest bieżący kurs. Jak sprawić, aby właściwość była tylko do odczytu i nie można było jej zapisywać z zewnątrz?


Gdy tworzę w bazie danych tabele, które są ze sobą logicznie powiązane (np. tabelę dla użytkownika, a następnie tabelę dla jego artykułów), muszę mieć pewność, że dane zostaną poprawnie połączone.
- Co gwarantuje, że dane w tabelach są prawidłowo połączone w bazie danych?
- Co to jest integralność referencyjna i jaka jest jej rola w bazie danych?
- Jakie rodzaje sesji prowadzimy? Jaki jest cel każdego typu?
- Jakie parametry ustawiamy dla sesji, aby w różny sposób obsługiwały usuwanie lub modyfikowanie danych? Podaj 3 przykłady i konkretne zastosowania + opis sposobu działania.


20) Jaki jest cel stosowania fabryk (wzorzec projektowy OOP)?
- Podaj przykład zastosowania.
- Czy usługi Nette są fabrykami?
- Jaki jest cel wstrzykiwania zależności?
- Jaka jest różnica między `DI` a `DIC`?
