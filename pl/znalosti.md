Przegląd wiedzy z zakresu tworzenia stron internetowych
=======================================================

> id: e5e9613c-66fe-4d5e-a686-a182aab8061a
> slug:
> 	cs: znalosti
> 	pl: przeglad-wiedzy-z-zakresu-tworzenia-stron-internetowych
> 
> publicationDate: '2019-09-11 10:07:07'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: b7284b1fc11b40ddb7628995070198a1

Wiedza o tym, jak stworzyć stronę internetową, a następnie kompleksowo się nią opiekować, to nie tylko kwestia jej stworzenia. Po drodze jest wiele przeszkód i dobrze jest mieć przynajmniej podstawowe pojęcie o każdej z nich. Kiedy zaczynałam, nie bardzo wiedziałam, czego się uczyć. Ta strona służy jako drogowskaz przez tematy, które musiałem stopniowo zgłębiać, aby móc zrozumieć tworzenie stron internetowych i radzić sobie z najczęstszymi sytuacjami.

Administracja serwerem
--------------

> Serwer internetowy to komputer, na którym działa sieć WWW. Gdy użytkownik ogląda stronę, serwer WWW wysyła ją do użytkownika.
>
> W chwili obecnej (2021) nie ma już sensu korzystać z darmowego hostingu, jeśli poważnie myślisz o sieci. Serwer można **wynająć od 50 koron miesięcznie**.

- Instalacja serwera (różni się w systemach Windows i Linux)
- Konfiguracja serwera
	- Ustawienia Apache
	- Konfiguracja PHP
	- <a href="/info">zapoznanie się z aktualną konfiguracją PHP</a> (funkcja `phpinfo()`)
	- routing Ngnix (poprawia wydajność serwera)

Internet i przeglądarka internetowa
--------------------------------

- Przeglądarki internetowe
- Zasada żądania i odpowiedzi
	- Adres URL
	- Ajax i Ajaj
	- Generowanie kodu HTML (systemy szablonów)

Struny
-----------------

- Czytanie, pisanie i konkatenacja ciągów znaków, w szczególności podstawowe operacje na ciągach znaków
- Przetwarzanie ciągów znaków
	- Przeglądanie znaków według znaków
	- <a href="/if">porównywanie ciągów znaków</a>.
	- Podobieństwo łańcuchów (funkcje `levenshtein()`, `similar_text()` i `soundex()`)
	- <a href="/explode">Explode</a>, dzielenie przez separator
	- **wyrażenia regularne** dzielą ciągi znaków według uniwersalnie konfigurowalnej maski
	- **Tokenizer** rozbija ciągi znaków na małe części (tokeny)
- Serializacja danych do postaci łańcucha znaków
	- <a href="/json">Json</a>, obiekt javascript przechowywany w łańcuchu znaków
