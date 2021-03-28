Přijmutí dat metodou POST
================================

> id: c2f273fa-7730-4521-82d0-1b3096269fac
> slugCS: metoda-post
> publicationDate: "2019-08-22 20:48:46"
> mainCategoryId: "2a1ef8bc-14aa-438a-87e7-5b3f9643f325"

Posílání dat POSTem se od <a href="/metoda-get">GET</a>u docela liší, je bezpečnější, text může být delší a jeho hodnota se nedá určit jinak než formulářem a nebo podstrčenou hlavičkou (což se vám určitě omylem nepovede).

Zdroj
--------------------------

Zdroják se od metody <a href="/metoda-get">GET</a> až tolik neliší. Je to skoro to samé, akorát se parametry nezobrazují v URL, ale je vidět jen jméno souboru.

```php
echo $_POST['clanek'] ?? ''; 
```

Charakteristika, výhody a nevýhody
--------------------------

- Na parametry se nedá odkázat odkazem, ale musí se na ně poslat formulář
- Nejdou zaindexovat (souvisí s předchozím  bodem)
- Bezpečnější než <a href="/metoda-get">GET</a> (data se odesílají skrytě, hodnoty se nezobrazují v historii)
- Neplést s SSL, POST není šifrovaný
