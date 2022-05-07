Parsowanie i przetwarzanie danych w PHP
=======================================

> id: '7dfc991d-a7ee-4e2b-8159-16cef1d27483'
> slug:
> 	cs: parsovani-a-zpracovani-dat
> 	pl: parsowanie-i-przetwarzanie-danych-w-php
> 
> publicationDate: '2017-10-15 09:54:02'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '33947488a0005f2977e6283c532a08d7'

> W tym artykule zostaną omówione metody przetwarzania danych w PHP, ale nie jest on jeszcze zakończony.

Tak więc chwilowo tylko krótki szkic możliwości:

- Przekształcanie znak po znaku** to bardzo stara metoda, która powoduje, że kod jest niechlujny, ale wszystkie inne metody robią to wewnętrznie.
- <a href="/explode">Explode</a>, dzielenie łańcucha znaków przez ogranicznik
- <a href="/regex">**wyrażenia regularne**</a> to najlepszy sposób na obsługę prostych ciągów znaków
- **Tokenizer**, dzieli złożony ciąg znaków na części (tokeny) zgodnie z wyrażeniami regularnymi, na przykład w ten sposób przetwarzany jest PHP
