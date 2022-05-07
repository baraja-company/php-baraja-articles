Formularz wieloletni
====================

> id: '2abacddd-8a6b-4d25-a387-603fc7abc333'
> slug:
> 	cs: vicekrokovy-formular
> 	pl: formularz-wieloletni
> 
> publicationDate: '2019-09-16 09:30:19'
> mainCategoryId: a23332c0-a233-4093-abd7-85b1b00a383b
> sourceContentHash: '69cb85c856478d588b96e9db9e695d97'

Czasami musimy podzielić formularz na kilka części (stron), przetworzyć je osobno, a następnie złożyć w jeden wynik.

W tym artykule opisano metody i wzorce projektowe umożliwiające realizację tego celu.

> **Uwaga:**
>
> Problem dzielenia formularza na wiele kroków jest bardzo złożony, zwłaszcza jeśli chcesz to zrobić dobrze. W swoim życiu spotkałem się z wieloma podejściami, które tu omówię. Niektóre podejścia wyglądają atrakcyjnie, ale są naiwne i sprawdzają się tylko w określonych przypadkach. Dla każdego podejścia opisuję, kiedy ma ono sens, a kiedy nie.

Projekt rozwiązania teoretycznego
-------------------------

Zwykle celem jest pobranie podstawowych danych z pierwszego formularza na pierwszej stronie, sprawdzenie ich poprawności, a następnie zapisanie ich "gdzieś" i wyświetlenie następnej strony.

Gdy użytkownik dotrze do ostatniej strony, należy przesłać cały formularz i przetworzyć dane wejściowe.

Na każdym etapie należy zawsze dokładnie sprawdzać poprawność wszystkich danych i pozwalać użytkownikowi na dowolne przechodzenie między kolejnymi stronami, tak aby mógł on skorygować dane, gdy napotka błąd. Ponadto, jeśli formularz ma być renderowany warunkowo na podstawie już uzyskanych danych, jest to bardzo wymagający proces.

Wdrażanie samych formularzy
--------------------------------

Możemy albo sami zaimplementować poszczególne formularze w czystym HTML-u, a następnie zająć się ich przetwarzaniem w PHP, albo skorzystać z gotowych rozwiązań, takich jak <a href="https://doc.nette.org/cs/3.0/forms">Formularze Nette</a>.

> **Przykład z życia:**
>
> Bardzo często początkujący programiści wysyłają do mnie e-maile z pozornie prostymi pytaniami, na które istnieje gotowe rozwiązanie. Na przykład o przetwarzaniu formularzy w PHP.
>
> Zawsze zalecam rezygnację z ręcznego przetwarzania danych i korzystanie z gotowych rozwiązań. W rzeczywistości bardzo skomplikowane jest prawidłowe zaimplementowanie np. walidacji wprowadzonego adresu e-mail oraz zgodności 2 haseł w 2 polach, a jednocześnie chcemy przekierować użytkownika z powrotem do wstępnie wypełnionego formularza zgodnie z jego danymi i z komunikatem o błędzie w przypadku wystąpienia błędu.
>
> Ponieważ ludzie **nie wiedzą, że nie wiedzą, że nie wiedzą**, więc zamiast zainwestować 1 godzinę czasu w poznanie gotowego rozwiązania 99,99% problemów, wolą wybrać własne rozwiązanie, na którego debugowanie poświęcają dziesiątki godzin, a i tak zdarzają się przypadki, że formularze nie działają, wyrzucają błędy, mają luki w zabezpieczeniach i nie chronią wprowadzanych danych.

Celem tego kroku jest więc wdrożenie kilku stron pod różnymi adresami URL, które będą zawierały puste formularze.

Zalecam implementację każdego formularza niezależnie od pozostałych (atomowo) i obsługę przekazywania stanu w innej warstwie aplikacji. Powodem jest to, że każdy formularz będzie w praktyce inaczej obsługiwał sprawdzanie poprawności danych, inaczej zapisywał dane wyjściowe, inaczej obsługiwał błędy i prawdopodobnie z czasem będziemy chcieli go rozbudować lub zmienić, więc nie musimy znać kontekstu całego procesu i zmieniać w tym celu dziesiątków stron.

Przeniesienie własności państwa
---------------

Podczas przetwarzania pierwszego formularza chcemy najpierw zatwierdzić otrzymane dane i jeśli są one poprawne, przekierować użytkownika do drugiego kroku. Dobrym pomysłem jest obsługa przekierowania jako przekierowania HTTP, ponieważ łatwo może się zdarzyć, że dane nie są poprawne, a w takim przypadku chcemy odesłać użytkownika do pierwszego formularza, a nie do następnego kroku.

Stany możemy przechowywać na 4 sposoby:

**Nie zaleca się:**

- Ma to tę wadę, że użytkownik może raz zmienić dane już przesłane w adresie URL i w ten sposób sfałszować dane wejściowe. Alternatywnie, w adresie URL możemy ujawnić informacje wrażliwe, takie jak hasła.
- **Dodawaj w sposób ciągły do <a href="/sesje">sesji</a>**, tzn. przyrostowo wstawiaj nowo pozyskane dane według klucza do pola. Ma to tę wadę, że jeśli aplikacja popełni błąd, użytkownik zostaje uwięziony w sesji i nie może w żaden sposób usunąć błędu (poza usunięciem plików cookie, co dla większości osób jest niezwykle trudne), a ponadto w przypadku nieukończonego formularza istnieje ryzyko, że dane pozostaną wstępnie wypełnione i mogą być widoczne dla innych osób. Jednak znacznie gorszy przypadek występuje wtedy, gdy sesja ma bardzo krótki czas trwania (powiedzmy 5 minut) i użytkownik traci dane z pierwszego kroku podczas wypełniania ostatniego... może to być bardzo irytujące.

**Zalecane:**

- **Zapisywanie do bazy danych i przekazywanie identyfikatora**. Przy pierwszym przesłaniu formularza przechowujemy wszystkie zebrane dane w tabeli w bazie danych i generujemy losowy identyfikator (powiedzmy ciąg o długości 10 znaków), który jest przekazywany między stronami jako parametr. Zaletą tego rozwiązania jest to, że podczas przetwarzania dowolnego formularza możemy zapisać nowo pobrane i zweryfikowane dane bezpośrednio w tabeli, a w razie awarii mamy fizyczne kopie zapasowe przetworzonych formularzy i możemy na nich działać. Na przykład, gdy zamówienie jest niekompletne, możemy wysłać wiadomość e-mail do użytkownika, że nie dokończył zamówienia i zwiększyć szansę na sprzedaż.
- Opcja **Zapisz na aktualnie zalogowane konto** działa dokładnie tak samo jak przekazywanie przez ID, z tą różnicą, że zamiast losowego identyfikatora (tokena) używamy sesji do ID aktualnie zalogowanego użytkownika (jeśli taki istnieje). Zaletą tego rozwiązania jest to, że w przyszłości możemy dowolnie pokazywać użytkownikowi wstępnie wypełnione dane.

Wniosek
-----

Żadne z tych rozwiązań nie jest idealne ani jedyne właściwe. Sam łączę wiele podejść, gdy pracuję nad rozwiązaniami wieloetapowymi. Zazwyczaj, na przykład, rozwiązuję koszyk zakupów jako tabelę w bazie danych, do której przypisuję zebrane już dane i wiążę ją albo z użytkownikiem (jeśli jest zalogowany), albo z sesją (jeśli nie jest zalogowany i jeszcze się nie znamy).
