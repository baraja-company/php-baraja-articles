Formuláre HTML - časť v prehliadači
===================================

> id: cb0015c7-b7b6-41ac-8263-4068960e16b3
> slug:
> 	cs: formulare
> 	sk: formulare-html---cast-v-prehliadaci
> 
> perex: 'Zpracování formulářů v PHP, zejména možnosti odeslání získaných dat na server metodou GET a POST.'
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: a23332c0-a233-4093-abd7-85b1b00a383b
> sourceContentHash: '8d2a0df0667b3da5d6b19e6c25ad2517'

Predtým, ako môžeme spracovať akékoľvek údaje používateľa na strane servera prostredníctvom jazyka PHP, musíme ich najprv získať. To sa vykonáva v prehliadači prostredníctvom formulárov HTML, ktoré definujú základné prvky na prijímanie údajov. Cieľom tohto článku nie je predstaviť všetky možnosti formulárov, ale len základné možnosti prijímania údajov a pochopenia princípu.

Základný zdroj formulára HTML
-----------------------------

<form action="script.php" method="get">

<!-- Zde bude celý obsah formuláře -->

</form>

Každý formulár začína značkou HTML `<form>` a končí značkou `</form>`. Všetky polia formulára umiestnené medzi týmito značkami budú odoslané.

Ďalej je potrebné nastaviť, kam sa má formulár odoslať pomocou atribútu `action` (názov skriptu) a akú metódu použiť pomocou atribútu `method` (GET alebo POST). Ak nie je zadaná metóda a cieľ, formulár sa štandardne odošle metódou GET.

Základné polia formulára
-------------------------

Na získanie textu (reťazca) sa používa najpoužívanejšie pole. Každé pole má svoj typ a názov, podľa ktorého ho možno po odoslaní rozpoznať.

Bežné textové polia
------------------

Najdôležitejšie je, že požadujem obyčajné textové pole:

<input type="text" name="food">

<input type="text" name="jedlo">

Pole s heslom
---------------------

<input type="password" name="heslo">

<input type="heslo" name="heslo">

Zaškrtávacie políčko
--------

Používa sa na kontrolu logickej hodnoty (`TRUE` a `FALSE`):

<input type="checkbox" name="vop" checked="checked">

<label>
	<input type="checkbox" name="vop" checked="checked"> Súhlasíte s podmienkami?
</label>

Rádiové tlačidlo na výber viacerých možností
------------------------------------

<input type="radio" name="language" value="cz" checked> Čeština
<input type="radio" name="language" value="sk"> Slovenština
<input type="radio" name="language" value="en"> Angličtina

Môžete si vybrať z niekoľkých možností. Vybraná možnosť odošle svoju hodnotu. V predvolenom nastavení je dobré vybrať jedno pole s atribútom `checked="checked"`:

<label>
	<input type="radio" name="jazyk" value="cz" checked="začiarknuté"> Čeština
</label><br>
<label>
	<input type="radio" name="language" value="en"> Slovenčina
</label><br>
<label>
	<input type="radio" name="language" value="en"> Angličtina
</label>

Veľké textové pole
------------------

Vytvorené na zadávanie viacriadkového textu. Používa sa aj na zadávanie:

- `cols` ~ počet stĺpcov
- `rows` ~ počet riadkov

<textarea name="article" cols="40" rows="6">
Ahoj lidi!
</textarea>

<textarea name="article" cols="40" rows="6">
Ahoj, chlapci!
</textarea>

Selectbox
---------

Predstavuje pohodlný spôsob výberu z mnohých údajov.

<select name="gender">
	<option value="man">Muž</option>
	<option value="woman">Žena</option>
</select>

Po odoslaní formulára sa odošle hodnota v poli `value`.

<select name="pohlavie">
	<option value="man">Muž</option>
	<option value="žena">Žena</option>
</select>

Tlačidlo Odoslať
---------------------

Formulár môže mať neobmedzený počet tlačidiel na odoslanie. Je ľahké do nich vstúpiť:

<input type="submit" value="Odeslat">

Po kliknutí sa z polí formulára prevezmú všetky údaje a odošlú sa do nastaveného skriptu:

<input type="submit" value="Odoslať">

Spracovanie údajov na serveri
-------------------------

Ďalej je potrebné odoslať údaje na server a tam ich spracovať, o čom sa píše v <a href="/processing-formula-in-php">ďalšom článku</a>.
