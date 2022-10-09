Hentning af parametre fra URL ved hjælp af GET-metoden
======================================================

> id: bbf2cb2c-f7f7-4be9-a9cd-960014db0f51
> slug:
> 	cs: metoda-get
> 	da: hentning-af-parametre-fra-url-ved-hjaelp-af-get-metoden
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: caebe20b6b7cdf0b3703ff55dad6f094

Du ved, du har en side åben, du følger URL'en og ser et spørgsmålstegn med nogle parametre. En uerfaren programmør ville tro, at der er tale om separate filer, men se og hør. Prøv at oprette en fil, der har et spørgsmålstegn i navnet (det virker ikke). **Det er grunden til, at denne artikel blev skrevet**.

Hvad er det?
--------------------------

Pointen er faktisk, at det er en enkelt fil, som du sender variabler til via en URL, så jeg har f.eks. en **index.php**-fil, og jeg sender navnet på artiklen til den: **index.php?clanek=o-php**.

Kode + forklaring
--------------------------

Superglobal variabel `$_GET` indeholder nøgler med parametre fra URL

```php
echo $_GET['Artikel'] ?? '';
```

Sikkerheds- og længdegrænser
--------------------------

GET-metoden er ikke sikker, så fortrolige data bør ikke sendes via den, hvilket skyldes bl.a., at det er en ukrypteret kommunikation, og at den lagres i historikken.

Fortrolige data eller bare alt bør sendes ved hjælp af <a href="/method-post">POST</a>-metoden. GET er mere velegnet til furmulars hvor det er godt at vise parametre (f.eks. søgemaskiner, artikelside), så siden kan linkes til den.

Længden af GET er ikke ubegrænset! Mange begyndere betaler for dette. Den maksimale længde er ca. 1024 tegn (nogle steder er den 1088), så hvis du vil have længere tekster, skal du sende <a href="/method-post">POST</a>em.
