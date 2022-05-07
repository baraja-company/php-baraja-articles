Hämta parametrar från URL med GET-metoden
=========================================

> id: bbf2cb2c-f7f7-4be9-a9cd-960014db0f51
> slug:
> 	cs: metoda-get
> 	sv: haemta-parametrar-fran-url-med-get-metoden
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: caebe20b6b7cdf0b3703ff55dad6f094

Du vet, du har en sida öppen, du följer URL:en och ser ett frågetecken med några parametrar. En oerfaren programmerare skulle tro att det rör sig om separata filer, men se där. Försök att skapa en fil med ett frågetecken i namnet (det fungerar inte). **Detta är anledningen till att den här artikeln skrevs**.

Vad är det?
--------------------------

Poängen är faktiskt att det är en enda fil som du skickar variabler till via en URL, så jag har till exempel en fil **index.php** och jag skickar namnet på artikeln till den: **index.php?clanek=o-php**.

Kod + förklaring
--------------------------

Den superglobala variabeln `$_GET` innehåller nycklar med parametrar från URL-adressen.

```php
echo $_GET['Artikel'] ?? '';
```

Säkerhets- och längdgränser
--------------------------

GET-metoden är inte säker, så konfidentiella uppgifter bör inte skickas via den. En av de viktigaste orsakerna är att det är en okrypterad kommunikation och att den lagras i historik.

Konfidentiella uppgifter eller allting bör skickas med <a href="/method-post">POST</a>-metoden. GET passar bättre för furmulars där det är bra att visa parametrar (t.ex. sökmotorer, artikelsida) så att sidan kan länkas till.

Längden på GET är inte obegränsad! Många nybörjare betalar för detta. Den maximala längden är cirka 1024 tecken (på vissa ställen 1088). För längre texter skickar du <a href="/method-post">POST</a> med.
