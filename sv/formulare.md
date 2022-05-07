HTML-formulär - en del i webbläsaren
====================================

> id: cb0015c7-b7b6-41ac-8263-4068960e16b3
> slug:
> 	cs: formulare
> 	sv: html-formulaer---en-del-i-webblaesaren
> 
> perex:
> 	- 'Zpracování formulářů v PHP, zejména možnosti odeslání získaných dat na server metodou GET a POST.'
> 	- 'Behandling av formulär i PHP, särskilt möjligheten att skicka de erhållna uppgifterna till servern med metoderna GET och POST.'
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: a23332c0-a233-4093-abd7-85b1b00a383b
> sourceContentHash: '8d2a0df0667b3da5d6b19e6c25ad2517'

Innan vi kan behandla användardata på serversidan via PHP måste vi först få in dem. Detta görs i webbläsaren via HTML-formulär som definierar de grundläggande elementen för att ta emot data. Syftet med den här artikeln är inte att presentera alla möjligheter till formulär, utan bara de grundläggande möjligheterna att ta emot data och förstå principen.

Grundläggande HTML-blankettkälla
-----------------------------

```html
<form action="script.php" method="get">

<!-- Zde bude celý obsah formuláře -->

</form>
```

Varje formulär börjar med HTML-taggen `<form>` och slutar med taggen `</form>`. Alla formulärfält som placeras mellan dessa taggar kommer att skickas in.

Därefter måste du ange vart formuläret ska skickas med attributet `action` (skriptnamn) och vilken metod som ska användas med attributet `method` (GET eller POST). Om metoden och destinationen inte anges är standardinställningen att formuläret skickas med GET-metoden.

Grundläggande formulärfält
-------------------------

Det mest använda fältet används för att få fram texten (sträng). Varje fält har en egen typ och ett eget namn som gör det möjligt att känna igen det efter inlämning.

Vanliga textfält
------------------

Viktigast av allt är att jag behöver ett fält för vanlig text:

```html
<input type="text" name="food">
```

<input type="text" name="food">

Fältet för lösenord
---------------------

```html
<input type="password" name="heslo">
```

<input type="password" name="password">

Kryssrutan
--------

Den används för att kontrollera boolean (`TRUE` och `FALSE`):

```html
<input type="checkbox" name="vop" checked="checked">
```

<label>
	<input type="checkbox" name="vop" checked="checked"> Godkänner du villkoren?
</label>

Radioknapp för att välja flera alternativ
------------------------------------

```html
<input type="radio" name="language" value="cz" checked> Čeština
<input type="radio" name="language" value="sk"> Slovenština
<input type="radio" name="language" value="en"> Angličtina
```

Du kan välja mellan flera alternativ. Det valda alternativet skickar sitt värde. Som standard är det bra att välja ett fält med attributet "Checked="checked"`:

<label>
	<input type="radio" name="language" value="cz" checked="checked"> Tjeckiska
</label><br>
<label>
	<input type="radio" name="language" value="en"> Slovakiska
</label><br>
<label>
	<input type="radio" name="language" value="en"> Engelska
</label>

Stort textfält
------------------

Skapad för att skriva in text på flera rader. Den används också för att skriva in:

- `cols` ~ antal kolumner
- `rows` ~ antal rader

```html
<textarea name="article" cols="40" rows="6">
Ahoj lidi!
</textarea>
```

<textarea name="article" cols="40" rows="6">
Hej killar!
</textarea>

Väljbox
---------

Det är ett bekvämt sätt att välja mellan många uppgifter.

```html
<select name="gender">
	<option value="man">Muž</option>
	<option value="woman">Žena</option>
</select>
```

Efter att formuläret har skickats skickas värdet i `value`.

<select name="kön">
	<option value="man">Man</option>
	<option value="woman">Kvinna</option>
</select>

Skicka-knappen
---------------------

Formuläret kan ha ett obegränsat antal knappar. Det är lätt att komma in i dem:

```html
<input type="submit" value="Odeslat">
```

När du klickar på den tar den alla data från formulärfälten och skickar dem till det inställda skriptet:

<input type="submit" value="Submit">

Databehandling på servern
-------------------------

Därefter måste du skicka data till servern och bearbeta den där, detta behandlas i <a href="/processing-formula-in-php">nästa artikel</a>.
