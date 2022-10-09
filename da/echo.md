Echo - output til kildekoden
============================

> id: d6a1a7bf-03bf-49ea-9947-d4d6753ca6e4
> slug:
> 	cs: echo
> 	da: echo-output-til-kildekoden
> 
> perex:
> 	- Echo - výpis řetězce do stránky a na standardní výstup. Možnosti escapování a HTTP hlaviček.
> 	- Echo - sender strengen til siden og til standardudgangen. Indstillinger for escaping og HTTP-headere.
> 
> publicationDate: '2020-02-16 18:40:31'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '7d611691825ec537f58f387696d13e28'

Konstruktionen `echo` bruges til at dumpe en variabel eller streng i kildekoden.

| Støtte: | Alle versioner
|----------------|------
| Kort beskrivelse: | Output en eller flere strenge
| Type: | <a href="/kommandoer-og-funktioner">kommando, konstrukt</a> (ikke en funktion)

Beskrivelse
-----

```php
echo 'Hej, verden';
```

Der står "hello world".

```php
$var = 'Tekst';
echo $var;
```

Udskriver værdien af variablen `$var`, dvs. "Text".

**Echo** er ikke en funktion (det er en <a href="/kommandoer-og-funktioner">kommando</a>), så du kan bruge en parentes, men du kan også undlade at bruge en parentes. Derfor er det også korrekt at skrive `echo ('hello world');`.

> **Ekstra note:** PHP behandler **Echo** som en kommando (en konstruktion) og behandler det derfor som et udtryk. Parentesen er valgfri i dette tilfælde. Hvis vi angiver notationen: `echo ('something');`, bliver **Echo**-anvisningen ikke en funktion og behandles ikke som sådan. Parentesen betyder i dette tilfælde, at den nøjagtige værdi af udtrykket er indesluttet, ligesom det fungerer i matematik.

Anførselstegn
--------

Strings kan omsluttes af anførselstegn og apostroffer.

Så dette:

```php
echo "Hej";
```

Det er det samme som dette:

```php
echo 'Hej';
```

Men vær opmærksom på, at hver streng skal starte og slutte med den samme type citationstegn, og at citationstegnet ikke må bruges i strengen.

Hvis du f.eks. ønsker at udskrive et HTML-link (eller enhver HTML-kode), skal du sætte en skråstreg foran anførselstegnet. En skråstreg betyder "præcis dette tegn", så den forstås ikke som et udtryk i sproget.

```php
echo "<a href="index.php\">link tekst</a>";
```

Teknisk note: Anførselstegn har en <a href="/citat-betydning">særlig betydning</a> i PHP.

Parametre
---------

- `arg` Udgangsparameter.

Returneringsværdier
-----------------

Der returneres ingen værdi.

Kan ikke bruges som en variabel.

Bemærk
--------

Bemærk: Da dette er en **sprogkonstrukt** (konstrukt = kommando) (ikke en funktion), kan den ikke indlæses i en variabel.

Eksempel
-------

```php
echo "Hej, verden";

echo "echo kan udsende flere linjer tekst.
Men pas på HTML-tag <br>, det bliver ikke udskrevet. Det er det, som nl2br()-funktionen er beregnet til.";

$a = "php";				// definition af variabel

echo "Jeg kan godt lide" . $a;	// Han skriver: Jeg kan godt lide php
```

**Echo** har også en forkortet syntaks, hvor det er muligt kun at bruge lighedstegnet efter det indledende php-tag.

```php
Ahoj <?=$jmeno;?>!
```

Dette er nyttigt, hvis vi har brug for at skrive nogle hurtige oplysninger til siden. For eksempel det indeværende år:

```php
Píše Jan Barášek © <?=date('Y');?>
```

> Denne forkortede syntaks vil kun fungere, hvis forkortede php-åbningstags er aktiveret, dvs. at `short_open_tag`-direktivet er sat til `on`.

Operation
-------

Alle almindelige matematiske operationer kan udføres i **echo**-kommandoen.

For en detaljeret gennemgang af matematik, se <a href="/matematik">en separat artikel</a>.

```php
echo 5 + 3 * 2; // udskriver 11
```
