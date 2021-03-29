HTML formuláře - část v prohlížeči
==================================

> id: cb0015c7-b7b6-41ac-8263-4068960e16b3
> slugCS: formulare
> perex: "Zpracování formulářů v PHP, zejména možnosti odeslání získaných dat na server metodou GET a POST."
> publicationDate: "2019-08-22 20:48:46"
> mainCategoryId: a23332c0-a233-4093-abd7-85b1b00a383b

Než budeme moci zpracovávat nějaká uživatelská data na straně serveru přes PHP, tak je musíme nejprve získat. To se dělá v prohlížeči přes HTML formuláře, které definují základní prvky k přijímání dat. Smyslem tohoto článku není představit veškeré možnosti formulářů, ale jen základní možnosti přijmutí dat a pochopení principu.

Základní zdroj HTML formuláře
-----------------------------

```html
<form action="script.php" method="get">

Zde bude celý obsah formuláře

</form>
```


Každý formulář začíná HTML značkou `<form>` a končí značkou `</form>`. Všechna formulářová pole umístěná mezi těmito značky budou odeslána.

Dále je nutné nastavit, kam formulář odeslat atributem `action` (název scriptu), a jakou metodou atributem `method` (GET nebo POST). Pokud metodu a cíl neuvedeme, tak se formulář defaultně posílá sám na sebe metodou GET.

Základní formulářová pole
-------------------------

Nejvíce používané pole slouží pro získání textu (stringu). Každé pole má svůj typ a název, podle kterého jej po odeslání poznáme.

Běžné textové pole
------------------

Za nejdůležitější požaduji obyčejné textové pole:

```html
<input type="text" name="food">
```

<input type="text" name="food">

Pole pro zadání hesla
---------------------

```html
<input type="password" name="heslo">
```

<input type="password" name="heslo">

Checkbox
--------

Slouží pro zjištění booleanu (`TRUE` a `FALSE`):

```html
<input type="checkbox" name="vop" checked="checked">
```

<label>
	<input type="checkbox" name="vop" checked="checked"> Souhlasíte s podmínkami?
</label>

Radio button pro výběr více možností
------------------------------------

```html
<input type="radio" name="language" value="cz" checked="checked"> Čeština
<input type="radio" name="language" value="sk"> Slovenština
<input type="radio" name="language" value="en"> Angličtina
```


Umožňuje výber z několika možností. Vybraná možnost posílá svojí hodnotu (value). Defaultně je dobré vybrat jedno pole atributem `checked="checked"`:

<label>
	<input type="radio" name="language" value="cz" checked="checked"> Čeština
</label><br>
<label>
	<input type="radio" name="language" value="sk"> Slovenština
</label><br>
<label>
	<input type="radio" name="language" value="en"> Angličtina
</label>

Velké textové pole
------------------

Vzniklo pro zadávání víceřádkového textu. Zadává se také:

- `cols` ~ počet sloupců
- `rows` ~ počet řádků

```html
<textarea name="article" cols="40" rows="6">
Ahoj lidi !
</textarea>
```

<textarea name="article" cols="40" rows="6">
Ahoj lidi !
</textarea>

Selectbox
---------

Představuje pohodlnou možnost, jak vybrat z mnoha dat.

```html
<select name="gender">
	<option value="man">Muž</option>
	<option value="woman">Žena</option>
</select>
```

Po odeslání formuláře se posílá hodnota ve `value`.

<select name="gender">
	<option value="man">Muž</option>
	<option value="woman">Žena</option>
</select>

Tlačítko pro odeslání
---------------------

Formulář může mít odesílacích tlačítek neomezeně mnoho. Zapisují se snadno:

```html
<input type="submit" value="Odeslat">
```

Po kliknutí vezme všechna data z formulářových polí a odešle na nastavený script:

<input type="submit" value="Odeslat">

<br>

> TIP: Máte svoje formuláře ochráněné captchou proti spamu?

Zpracování dat na serveru
-------------------------

Odeslaná data je dále nutné na serveru přijmout a zpracovat, o tom pojednává další článek.

<a href="/formulare-2">
	<button class="btn btn-success">Zpracování dat na serveru</button>
</a>
