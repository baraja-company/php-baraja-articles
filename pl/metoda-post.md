Odbieranie danych metodą POST
=============================

> id: c2f273fa-7730-4521-82d0-1b3096269fac
> slug:
> 	cs: metoda-post
> 	pl: odbieranie-danych-metoda-post
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '51a30cb759cdbb184ee5bb0bf3070af1'

Wysyłanie danych metodą POST różni się znacznie od <a href="/method-get">GET</a>, jest bezpieczniejsze, tekst może być dłuższy, a jego wartość nie może być określona inaczej niż przez formularz lub nagłówek (którego nie otrzymasz przez pomyłkę).

Źródło
--------------------------

Źródło nie różni się zbytnio od metody <a href="/method-get">GET</a>. Wygląda to bardzo podobnie, z tą różnicą, że parametry nie są wyświetlane w adresie URL, widoczna jest tylko nazwa pliku.

```php
echo $_POST['Artykuł'] ?? '';
```

Cechy charakterystyczne, zalety i wady
--------------------------

- Nie można łączyć się z parametrami, ale należy przesłać formularz
- Nie można indeksować (związane z poprzednim punktem)
- Bezpieczniejsze niż <a href="/method-get">GET</a> (dane są przesyłane w sposób ukryty, wartości nie są wyświetlane w historii)
- Nie należy mylić z SSL, POST nie jest szyfrowany.
