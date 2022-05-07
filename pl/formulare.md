Formularze HTML - część w przeglądarce
======================================

> id: cb0015c7-b7b6-41ac-8263-4068960e16b3
> slug:
> 	cs: formulare
> 	pl: formularze-html---czesc-w-przegladarce
> 
> perex:
> 	- 'Zpracování formulářů v PHP, zejména možnosti odeslání získaných dat na server metodou GET a POST.'
> 	- 'Przetwarzanie formularzy w PHP, w szczególności możliwość wysyłania uzyskanych danych do serwera za pomocą metod GET i POST.'
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: a23332c0-a233-4093-abd7-85b1b00a383b
> sourceContentHash: '8d2a0df0667b3da5d6b19e6c25ad2517'

Zanim będziemy mogli przetwarzać dane użytkownika po stronie serwera za pomocą PHP, musimy je najpierw uzyskać. Odbywa się to w przeglądarce za pomocą formularzy HTML, w których zdefiniowane są podstawowe elementy służące do odbierania danych. Celem tego artykułu nie jest przedstawienie wszystkich możliwości formularzy, lecz jedynie podstawowych możliwości przyjmowania danych i zrozumienia zasady działania.

Podstawowe źródło formularza HTML
-----------------------------

```html
<form action="script.php" method="get">

<!-- Zde bude celý obsah formuláře -->

</form>
```

Każdy formularz rozpoczyna się od znacznika HTML `<form>`, a kończy znacznikiem `</form>`. Wszystkie pola formularza umieszczone między tymi znacznikami zostaną przesłane.

Następnie należy określić, gdzie wysłać formularz za pomocą atrybutu `action` (nazwa skryptu) oraz jaką metodę zastosować za pomocą atrybutu `method` (GET lub POST). Jeśli metoda i miejsce docelowe nie zostaną określone, formularz domyślnie wysyła się za pomocą metody GET.

Podstawowe pola formularza
-------------------------

Najczęściej używane pole jest używane do uzyskania tekstu (łańcucha). Każde pole ma swój własny typ i nazwę, dzięki której można je rozpoznać po przesłaniu.

Wspólne pola tekstowe
------------------

Co najważniejsze, wymagane jest zwykłe pole tekstowe:

```html
<input type="text" name="food">
```

<input type="text" name="food">.

Pole hasła
---------------------

```html
<input type="password" name="heslo">
```

<input type="password" name="password">.

Pole wyboru
--------

Służy do sprawdzania wartości boolean (`TRUE` i `FALSE`):

```html
<input type="checkbox" name="vop" checked="checked">
```

<label>.
	<input type="checkbox" name="vop" checked="checked"> Czy zgadzasz się na warunki?
</label>.

Przycisk radiowy umożliwiający wybór wielu opcji
------------------------------------

```html
<input type="radio" name="language" value="cz" checked> Čeština
<input type="radio" name="language" value="sk"> Slovenština
<input type="radio" name="language" value="en"> Angličtina
```

Umożliwia on wybór spośród kilku opcji. Wybrana opcja wysyła swoją wartość. Domyślnie dobrze jest wybrać jedno pole z atrybutem `checked="checked"`:

<label>.
	<input type="radio" name="language" value="cz" checked="checked"> Język czeski
</label><br>.
<label>.
	<input type="radio" name="language" value="en"> Słowacki
</label><br>.
<label>.
	<input type="radio" name="language" value="en"> Angielski
</label>.

Duże pole tekstowe
------------------

Stworzony do wprowadzania tekstu wielowierszowego. Służy również do wprowadzania danych:

- `cols` ~ liczba kolumn
- `rows` ~ liczba wierszy

```html
<textarea name="article" cols="40" rows="6">
Ahoj lidi!
</textarea>
```

<textarea name="article" cols="40" rows="6">.
Hej, chłopaki!
</textarea>.

Pole wyboru
---------

Przedstawia wygodny sposób wyboru spośród wielu danych.

```html
<select name="gender">
	<option value="man">Muž</option>
	<option value="woman">Žena</option>
</select>
```

Po wysłaniu formularza wysyłana jest wartość zawarta w `value`.

<select name="gender">.
	<option value="man">Mężczyzna</option>.
	<option value="woman">Kobieta</option>.
</select>.

Przycisk Wyślij
---------------------

Formularz może zawierać nieograniczoną liczbę przycisków wysyłania. Są one łatwe do wprowadzenia:

```html
<input type="submit" value="Odeslat">
```

Po kliknięciu pobiera on wszystkie dane z pól formularza i wysyła je do ustawionego skryptu:

<input type="submit" value="Prześlij">.

Przetwarzanie danych na serwerze
-------------------------

Następnie trzeba wysłać dane na serwer i tam je przetworzyć, o czym piszemy w <a href="/processing-formula-in-php">kolejnym artykule</a>.
