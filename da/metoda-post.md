Modtagelse af data ved hjælp af POST-metoden
============================================

> id: c2f273fa-7730-4521-82d0-1b3096269fac
> slug:
> 	cs: metoda-post
> 	da: modtagelse-af-data-ved-hjaelp-af-post-metoden
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '51a30cb759cdbb184ee5bb0bf3070af1'

At sende data via POST er helt anderledes end <a href="/method-get">GET</a>, det er mere sikkert, teksten kan være længere, og dens værdi kan ikke bestemmes undtagen af en formular eller en header (som du ikke får ved en fejltagelse).

Kilde
--------------------------

Source er ikke så forskellig fra <a href="/method-get">GET</a>-metoden. Det er stort set det samme, bortset fra at parametrene ikke vises i URL'en, kun filnavnet er synligt.

```php
echo $_POST['Artikel'] ?? '';
```

Egenskaber, fordele og ulemper
--------------------------

- Der kan ikke knyttes et link til parametre, men der skal indsendes en formular
- Kan ikke indekseres (relateret til det foregående punkt)
- Sikrere end <a href="/method-get">GET</a> (data sendes på en skjult måde, værdierne vises ikke i historikken)
- POST skal ikke forveksles med SSL, POST er ikke krypteret
