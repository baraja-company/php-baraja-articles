Moduli HTML - parte nel browser
===============================

> id: cb0015c7-b7b6-41ac-8263-4068960e16b3
> slug:
> 	cs: formulare
> 	it: moduli-html---parte-nel-browser
> 
> perex:
> 	- 'Zpracování formulářů v PHP, zejména možnosti odeslání získaných dat na server metodou GET a POST.'
> 	- 'Elaborazione di moduli in PHP, specialmente la possibilità di inviare i dati ottenuti al server utilizzando i metodi GET e POST.'
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: a23332c0-a233-4093-abd7-85b1b00a383b
> sourceContentHash: '8d2a0df0667b3da5d6b19e6c25ad2517'

Prima di poter elaborare qualsiasi dato utente sul lato server tramite PHP, dobbiamo prima ottenerlo. Questo viene fatto nel browser tramite moduli HTML che definiscono gli elementi di base per ricevere i dati. Lo scopo di questo articolo non è quello di presentare tutte le possibilità di forme, ma solo le possibilità di base per accettare i dati e capire il principio.

Sorgente del modulo HTML di base
-----------------------------

```html
<form action="script.php" method="get">

<!-- Zde bude celý obsah formuláře -->

</form>
```

Ogni modulo inizia con il tag HTML `<form>` e finisce con il tag `</form>`. Tutti i campi del modulo inseriti tra questi tag saranno inviati.

Poi, devi impostare dove inviare il modulo con l'attributo `action` (nome dello script), e quale metodo usare con l'attributo `method` (GET o POST). Se il metodo e la destinazione non sono specificati, il modulo si invia per default con il metodo GET.

Campi modulo di base
-------------------------

Il campo più usato è utilizzato per ottenere il testo (stringa). Ogni campo ha il suo tipo e nome con cui può essere riconosciuto dopo l'invio.

Campi di testo comuni
------------------

La cosa più importante è che ho bisogno di un campo di testo semplice:

```html
<input type="text" name="food">
```

<input type="text" name="food">

Campo password
---------------------

```html
<input type="password" name="heslo">
```

<input type="password" name="password">

Checkbox
--------

È usato per controllare il booleano (`TRUE` e `FALSE`):

```html
<input type="checkbox" name="vop" checked="checked">
```

<label>
	<input type="checkbox" name="vop" checked="checked"> Accetti i termini?
</label>

Pulsante radio per selezionare più opzioni
------------------------------------

```html
<input type="radio" name="language" value="cz" checked> Čeština
<input type="radio" name="language" value="sk"> Slovenština
<input type="radio" name="language" value="en"> Angličtina
```

Permette di scegliere tra diverse opzioni. L'opzione selezionata invia il suo valore. Per default è bene selezionare un campo con l'attributo `checked="checked"`:

<label>
	<input type="radio" name="language" value="cz" checked="checked"> Czech
</label><br>
<label>
	<input type="radio" name="language" value="en"> Slovacco
</label><br>
<label>
	<input type="radio" name="language" value="en"> Inglese
</label>

Campo di testo grande
------------------

Creato per l'inserimento di testo su più righe. Si usa anche per entrare:

- `cols` ~ numero di colonne
- `rows` ~ numero di righe

```html
<textarea name="article" cols="40" rows="6">
Ahoj lidi!
</textarea>
```

<textarea name="article" cols="40" rows="6">
Ehi ragazzi!
</textarea>

Selectbox
---------

Presenta un modo conveniente per selezionare da molti dati.

```html
<select name="gender">
	<option value="man">Muž</option>
	<option value="woman">Žena</option>
</select>
```

Dopo aver inviato il modulo, il valore in `value` viene inviato.

<select name="genere">
	<option value="man">Maschio</option>
	<option value="donna">Femmina</option>
</select>

Pulsante di invio
---------------------

Il modulo può avere un numero illimitato di pulsanti di invio. Sono facili da inserire:

```html
<input type="submit" value="Odeslat">
```

Quando viene cliccato, prende tutti i dati dai campi del modulo e li invia allo script impostato:

<input type="submit" value="Submit">

Elaborazione dei dati sul server
-------------------------

Successivamente, è necessario inviare i dati al server ed elaborarli lì, questo è coperto in <a href="/processing-formula-in-php">il prossimo articolo</a>.
