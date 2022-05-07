Htmlspecialchars
================

> id: '46f04729-3956-4889-bb40-58362cb46b2a'
> slug:
> 	cs: htmlspecialchars
> 	pl: htmlspecialchars
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0743dd02-12d0-4766-ba42-8fd7e9c4ae8a'
> sourceContentHash: '566e10c57527a45f32906beb6c21430f'

Htmlspecialchars() to funkcja służąca do konwersji znaków specjalnych na encje HTML.

Opis
-----

```php
$promenna = htmlspecialchars($text);
```

Niektóre znaki specjalne mają specjalne znaczenie dla przeglądarek, dlatego należy je zamienić na encje. Zapobiega to ogólnemu bezpieczeństwu skryptów i uniemożliwia nieprawidłowe wyświetlanie strony.

Jest on najczęściej używany do ochrony formularzy i wszelkich miejsc, w których użytkownik wstawia tekst oraz ryzykuje wstawianie znaczników HTML.

| Znak | Uwaga | Zmiany do
|------|-------------------------|-----------
| `&` | ampersand | `&amp;`
| `"` | cudzysłów (zmienia się, gdy `ENT_NOQUOTES` jest wyłączone) | `&quot;`
| `'` | apostrof (zmienia się, gdy włączona jest opcja `ENT_QUOTES`) | `&#039;`
| `<` | mniej niż, nawias HTML | `&lt;`
| `>` | większy niż, nawias HTML | `&gt;`

Parametr
--------

**String** do konwersji

**flagi** Różne ustawienia zachowania

**charset** Określa zestaw znaków (kodowanie). Domyślnym zestawem znaków jest `ISO-8859-1`.

Można używać `ISO-8859-1`, `ISO-8859-15`, `UTF-8`, `cp866`, `CP1251`, `CP1252` i `KOI8-R`.

> Uwaga: Obsługa tylko od wersji PHP 4.3.0 i nowszych. Wszelkie inne zestawy znaków nie są rozpoznawane i obsługiwane.

**double_encode** Gdy `double_encode` jest wyłączone, PHP nie będzie kodować istniejących encji HTML, domyślnie wszystko będzie konwertowane.

Wartości zwracane
-----------------

Konwertuje ciąg znaków.

Jeśli łańcuch zawiera nieprawidłowe jednostki, w ramach podanego zestawu znaków w `ENT_IGNORE` (nie ustawiono), zwracany jest pusty łańcuch.

Zmiany w wersjach
----------------

| Wersja | Uwaga
|-------|---------
| 5.4.0 | Dodanie stałych `ENT_SUBSTITUTE`, `ENT_DISALLOWED`, `ENT_HTML401`, `ENT_XML1`, `ENT_XHTML` i `ENT_HTML5`.
| 5.3.0 | Dodanie stałej `ENT_IGNORE`.
| 5.2.3 | Dodanie parametru `double_encode`.
| 4.1.0 | Dodanie parametru `charset`.

Przykład
-------

```php
$new = htmlspecialchars(
	'<a href="test">Test</a>.',
	ENT_QUOTES
);

echo $new; // <a href="test">Test</a>.
```
