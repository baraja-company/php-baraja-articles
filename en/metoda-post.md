Receiving data by POST method
=============================

> id: c2f273fa-7730-4521-82d0-1b3096269fac
> slug:
> 	cs: metoda-post
> 	en: receiving-data-by-post-method
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '51a30cb759cdbb184ee5bb0bf3070af1'

Sending data by POST is quite different from <a href="/method-get">GET</a>, it is safer, the text can be longer and its value cannot be determined except by a form or a header (which you will not get by mistake).

Source
--------------------------

Source is not that different from the <a href="/method-get">GET</a> method. It's pretty much the same, except the parameters are not displayed in the URL, only the filename is visible.

```php
echo $_POST['clanek'] ?? '';
```

Characteristics, advantages and disadvantages
--------------------------

- Parameters cannot be linked to, but a form must be sent to them
- Cannot be indexed (related to the previous point)
- Safer than <a href="/method-get">GET</a> (data is sent in a hidden way, values are not displayed in the history)
- Not to be confused with SSL, POST is not encrypted
