Pliki cookie w PHP
==================

> id: '392dd88b-d2f5-4943-a993-01aaad7ccd32'
> slug:
> 	cs: cookies
> 	pl: pliki-cookie-w-php
> 
> perex:
> 	- 'Cookies je krátká textová informace v paměti prohlížeče, kam můžeme v PHP zapisovat a označit si uživatele.'
> 	- 'Plik cookie to krótki fragment informacji tekstowej w pamięci przeglądarki, w którym możemy zapisać i oznaczyć użytkownika w PHP.'
> 
> publicationDate: '2019-09-11 10:18:29'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '78f9aad0278acd51b98179e7d29fe3d8'

> **Ostrzeżenie:** Ten artykuł został napisany wiele lat temu i niektóre informacje mogą być nieaktualne lub nieprawidłowe. Należy o tym pamiętać podczas lektury.

Pliki cookie to małe fragmenty informacji tekstowych przechowywane w przeglądarce internetowej osoby odwiedzającej witrynę. Są one zawsze przesyłane z każdą kolejną wczytywaną stroną i mogą być w każdej chwili usuwane, zmieniane i odczytywane przez użytkownika, dlatego nie nadają się dobrze do przechowywania danych osobowych.

*Ostrzeżenie: jeśli witryna wykorzystuje pliki cookie do śledzenia użytkowników lub dodatki innych firm (np. przycisk "Lubię to" na Facebooku, licznik ruchu w Google Analytics, banery reklamowe), należy poinformować o tym użytkownika.

> "Jeszcze jedna uwaga: witryna nie powinna zawierać reklam ani kodów pomiarowych, dopóki nie uzyskasz na to zgody. I to jest do bani".
>
> -- <a href="https://phpfashion.com/jak-na-souhlas-s-cookie-ve-zkurvene-eu">David Grudl</a>.

Odczytaj wartość z pliku cookie
--------------------------

Wszystkie pliki cookie są przechowywane w zmiennej superglobalnej `$_COOKIE`, która przechowuje każdy klucz jako tablicę.

Na przykład, jeśli w pliku cookie pod kluczem `user` zapisaliśmy nazwę aktualnie zalogowanego użytkownika, możemy ją łatwo odzyskać:

```php
echo $_COOKIE['użytkownik'];
```

Uwaga: Pliki cookie mogą nie zawsze istnieć (np. jeśli jesteś nowym użytkownikiem). W związku z tym zawsze należy sprawdzić, czy pliki cookie istnieją, zanim rozpoczniemy publikację, a w razie potrzeby wyświetlić alternatywny komunikat o błędzie.

```php
if (isset($_COOKIE['użytkownik']) && $_COOKIE['użytkownik']) {
    echo 'Zalogowany użytkownik:' . $_COOKIE['użytkownik'];
} else {
    echo 'Nikt nie jest zalogowany.';
}
```

Pobieranie wszystkich dostępnych plików cookie
--------------------------------

Ponieważ wszystkie ciasteczka są przechowywane w zmiennej superglobalnej `$_COOKIE`, można je łatwo wylistować:

```php
var_dump($_COOKIE);
```

Alternatywnie można przejść przez cały cykl i pobrać wszystkie klucze i wartości:

```php
foreach($_COOKIE as $key => $value) {
    echo $key . ':' . $value;	// wypisz klucz i wartość
    echo '<br>.';				// zawiń linię
}
```

Przechowywanie wartości w pliku cookie
--------------------------

Funkcja `setcookie()` służy do przechowywania danych w plikach cookie.

Pierwszym parametrem jest klucz cookie, który jest używany do odczytywania go z pola `$_COOKIE`, a drugim - same dane w postaci łańcucha.

Za pomocą trzeciego parametru możemy (opcjonalnie) ustawić okres ważności, przez jaki plik cookie będzie dostępny. Czas dostępności jest podawany jako <a href="/date">timestamp</a>, więc jeśli chcemy ustawić ciasteczko o ważności 1 godziny od tego momentu, wystarczy, że napiszemy `time() + 3600`.

```php
$data = 'Pewna zawartość, którą chcemy przechowywać.';

setcookie('TestCookie', $data);
setcookie('TestCookie', $data, time() + 3600);
```

Przechowywanie większych danych
-------------------

Pliki cookie nie nadają się do przechowywania większych danych (przeglądarki zazwyczaj zezwalają na przechowywanie tylko 4 kB i maksymalnie 20 plików cookie, przy czym rozmiar ten obejmuje również nazwy plików cookie, ustawienia ważności itp.)

Większe dane lepiej jest przechowywać na serwerze, a w pliku cookie umieścić identyfikator, dzięki któremu będziemy mogli stwierdzić, do jakiego użytkownika należy dany plik. Metoda ta nosi nazwę `$_SESSION` i jest omówiona w osobnym artykule.

Jeśli niekoniecznie trzeba przechowywać dane zawsze synchronicznie na serwerze, można skorzystać z **<a href="https://jecas.cz/localstorage">localstorage</a>** pamięci masowej dostępnej w javascript. Jego pojemność jest rzędu MB, a dane nie podlegają wygaśnięciu.
