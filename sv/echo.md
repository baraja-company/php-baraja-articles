Echo - utskrift till källkoden
==============================

> id: d6a1a7bf-03bf-49ea-9947-d4d6753ca6e4
> slug:
> 	cs: echo
> 	sv: echo---utskrift-till-kaellkoden
> 
> perex:
> 	- Echo - výpis řetězce do stránky a na standardní výstup. Možnosti escapování a HTTP hlaviček.
> 	- Echo - dumpar strängen till sidan och till standardutgången. Alternativ för escaping och HTTP-huvuden.
> 
> publicationDate: '2020-02-16 18:40:31'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '7d611691825ec537f58f387696d13e28'

Konstruktionen `echo` används för att dumpa en variabel eller sträng i källkoden.

| Stöd: | Alla versioner
|----------------|------
| Kort beskrivning: | Skriv ut en eller flera strängar
| Typ: | <a href="/kommandon och funktioner">kommando, konstruktion</a> (inte en funktion)

Beskrivning
-----

```php
echo 'Hej, världen';
```

Det står "hello world".

```php
$var = 'Text';
echo $var;
```

Skriver ut värdet av variabeln `$var`, dvs. "Text".

**Echo** är inte en funktion (det är ett <a href="/commands-and-functions">kommando</a>), så du kan använda en parentes eller inte. Att skriva `echo ('hello world');` är alltså också korrekt.

> **Extra anmärkning:** PHP behandlar **Echo** som ett kommando (en konstruktion) och därmed som ett uttryck. Parentesen är valfri i detta fall. Om vi använder notationen: `echo ('something');`, blir **Echo**-kommandot inte en funktion och behandlas inte som en sådan. Parentesen innebär i det här fallet att man omsluter det exakta värdet av uttrycket, på samma sätt som i matematik.

Angivelsemärken
--------

Strängar kan omges av citationstecken och apostrofer.

Så detta:

```php
echo "Hej";
```

Det är samma sak som här:

```php
echo 'Hej';
```

Men tänk på att varje sträng måste börja och sluta med samma typ av citationstecken och att citationstecknet inte får användas i strängen.

Om du till exempel vill skriva ut en HTML-länk (eller någon HTML-kod) måste du föregå citattecknet med ett snedstreck. Ett snedstreck betyder "exakt det här tecknet", så det uppfattas inte som ett uttryck i språket.

```php
echo "<a href="index.php\">länktext</a>";
```

Teknisk anmärkning: Angivelsetecken har en <a href="/quotation-meaning">särskild betydelse</a> i PHP.

Parametrar
---------

- `arg` Utgångsparameter.

Returvärden
-----------------

Inget värde returneras.

Kan inte användas som en variabel.

Obs
--------

Observera: Eftersom detta är en **språkskonstruktion** (konstruktion = kommando) (inte en funktion) kan den inte laddas in i en variabel.

Exempel
-------

```php
echo "Hej, världen";

echo "echo kan skriva ut flera rader text.
Men akta dig för HTML-taggen <br>, den skrivs inte ut. Det är vad funktionen nl2br() är till för.";

$a = "php";				// definition av variabler

echo "Jag gillar" . $a;	// Han skriver: Jag gillar php
```

**Echo** har också en förkortad syntax, där det är möjligt att endast använda likhetstecknet efter den inledande php-taggen.

```php
Ahoj <?=$jmeno;?>!
```

Detta är användbart om vi behöver skriva lite snabb information till sidan. Till exempel det innevarande året:

```php
Píše Jan Barášek © <?=date('Y');?>
```

> Den här förkortade syntaxen fungerar bara om förkortade öppningsfält för php-taggar är aktiverade, dvs. om `short_open_tag`-direktivet är inställt på `on`.

Operation
-------

Alla vanliga matematiska operationer kan utföras med kommandot **echo**.

För en detaljerad diskussion om matematik, se <a href="/matematik">en separat artikel</a>.

```php
echo 5 + 3 * 2; // skriver ut 11
```
