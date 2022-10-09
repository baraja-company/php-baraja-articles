HTML-formularer - en del i browseren
====================================

> id: cb0015c7-b7b6-41ac-8263-4068960e16b3
> slug:
> 	cs: formulare
> 	da: html-formularer-en-del-i-browseren
> 
> perex:
> 	- 'Zpracování formulářů v PHP, zejména možnosti odeslání získaných dat na server metodou GET a POST.'
> 	- 'Behandling af formularer i PHP, især muligheden for at sende de indhentede data til serveren ved hjælp af GET- og POST-metoderne.'
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: a23332c0-a233-4093-abd7-85b1b00a383b
> sourceContentHash: '8d2a0df0667b3da5d6b19e6c25ad2517'

Før vi kan behandle brugerdata på serversiden via PHP, skal vi først hente dem. Dette sker i browseren via HTML-formularer, der definerer de grundlæggende elementer til modtagelse af dataene. Formålet med denne artikel er ikke at præsentere alle mulighederne for formularer, men blot de grundlæggende muligheder for at acceptere data og forstå princippet.

Grundlæggende HTML-formularkilde
-----------------------------

```html
<form action="script.php" method="get">

<!-- Zde bude celý obsah formuláře -->

</form>
```

Hver formular starter med HTML-tag `<form>` og slutter med tag `</form>`. Alle formularfelter, der er placeret mellem disse tags, vil blive indsendt.

Dernæst skal du angive, hvor formularen skal sendes hen med attributten `action` (scriptnavn), og hvilken metode den skal sendes med attributten `method` (GET eller POST). Hvis du ikke angiver en metode og destination, sendes formularen som standard med GET-metoden.

Grundlæggende formularfelter
-------------------------

Det mest anvendte felt bruges til at hente teksten (streng). Hvert felt har sin egen type og sit eget navn, som det kan genkendes efter indsendelse.

Almindelige tekstfelter
------------------

Vigtigst af alt er, at jeg har brug for et almindeligt tekstfelt:

```html
<input type="text" name="food">
```

<input type="text" name="food">

Adgangskodefelt
---------------------

```html
<input type="password" name="heslo">
```

<input type="password" name="password">

Afkrydsningsfelt
--------

Den bruges til at kontrollere boolske værdier (`TRUE` og `FALSE`):

```html
<input type="checkbox" name="vop" checked="checked">
```

<label>
	<input type="checkbox" name="vop" checked="checked"> Er du indforstået med betingelserne?
</label>

Radioknap til at vælge flere muligheder
------------------------------------

```html
<input type="radio" name="language" value="cz" checked> Čeština
<input type="radio" name="language" value="sk"> Slovenština
<input type="radio" name="language" value="en"> Angličtina
```

Den giver dig mulighed for at vælge mellem flere muligheder. Den valgte indstilling sender sin værdi. Som standard er det godt at vælge et felt med attributten `checked="checked"`:

<label>
	<input type="radio" name="language" value="cz" checked="checked"> Tjekkisk
</label><br>
<label>
	<input type="radio" name="language" value="en"> Slovakisk
</label><br>
<label>
	<input type="radio" name="language" value="en"> Engelsk
</label>

Stort tekstfelt
------------------

Oprettet til indtastning af tekst på flere linjer. Det bruges også til at indtaste:

- `cols` ~ antal kolonner
- `rows` ~ antal rækker

```html
<textarea name="article" cols="40" rows="6">
Ahoj lidi!
</textarea>
```

<textarea name="article" cols="40" rows="6">
Hej, venner!
</textarea>

Selectbox
---------

Præsenterer en praktisk måde at vælge mellem mange data på.

```html
<select name="gender">
	<option value="man">Muž</option>
	<option value="woman">Žena</option>
</select>
```

Når formularen er indsendt, sendes værdien i `value`.

<select name="køn">
	<option value="man">Mand</option>
	<option value="woman">Kvinde</option>
</select>

Indsend knap
---------------------

Formularen kan have et ubegrænset antal indsendelsesknapper. De er nemme at komme ind i:

```html
<input type="submit" value="Odeslat">
```

Når der klikkes på den, tager den alle dataene fra formularfelterne og sender dem til det indstillede script:

<input type="submit" value="Submit">

Databehandling på serveren
-------------------------

Dernæst skal du sende dataene til serveren og behandle dem der, dette er dækket i <a href="/processing-formula-in-php">den næste artikel</a>.
