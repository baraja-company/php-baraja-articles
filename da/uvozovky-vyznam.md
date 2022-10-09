Særlig betydning af anførselstegn
=================================

> id: '2649942d-94d6-48c3-9c78-5a303165bf72'
> slug:
> 	cs: uvozovky-vyznam
> 	da: saerlig-betydning-af-anforselstegn
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: c1b63b98a3e9d9c642247c363747616a

I PHP kan du ofte erstatte anførselstegn med apostroffer og opnå samme effekt. Nogle gange bruger vi endda en kombination af anførselstegn og apostroffer med vilje for at opnå forskellige resultater uden at skulle bruge escaping.

**TLDR: Det kan være farligt at bruge anførselstegn i visse sammenhænge, så brug apostroffer overalt i stedet. Anførselstegn er kun velegnede til særlige tilfælde.**

| Karakter | Navn |
|------|-----------
| `"` | Anførselstegn |
| `'` | Apostrof |

Eksempel et - samme resultat
-----------------------------

```php
echo "Hej, verden!"; // dette er det samme
echo 'Hej, verden!'; // sådan her
```

I dette tilfælde er brugen af anførselstegn eller apostroffer ligegyldig. På overfladen ser det ud til, at der ikke er nogen forskel på anførselstegn og apostroffer.

Kombination af anførselstegn og apostroffer
------------------------------

Overvej følgende kode:

```php
echo 'Min yndlingsfarve er "rød"!';
```

Hvis jeg brugte `carriage returns` til at afgrænse den streng, der skrives ud, ville det **være tvetydigt** hvor strengen **begynder** og hvor den slutter**, så jeg brugte `apostrofe` til at afgrænse strengen, hvilket løser dette problem.

Indsættelse af anførselstegn i en streng afgrænset af anførselstegn
---------------------------------------------------

Der kan lejlighedsvis opstå situationer, hvor jeg har brug for at angive både `quote` og `apostrophe` i den samme streng. Du kan ikke kun bruge sammenkædning af to strenge, men også såkaldt **escaping** af tegn.

```php
echo "Denne streng \"indeholder\" anførselstegn.";
```

Backslash har i dette tilfælde betydningen "præcis dette tegn", og derfor vil anførselstegnet ikke blive opfattet som slutningen af en streng (eller noget andet), og derfor vil det altid være et anførselstegn. Andre mærkelige tegn kan markeres på denne måde, hvis deres holdbarhed ikke kan garanteres, og hvis den korrekte betydning kan misforstås.

Variabel i en streng
-----------------------

Anførselstegn kan være meget vanskelige, fordi de giver dig mulighed for at indsætte variabler direkte i en streng:

```php
$color = 'Rød';

echo "Moje oblíbená barva je {$color}, a tvoje?";
```

Meget sikrere er apostroffer, som ikke tillader dette, og du skal folde strengen:

```php
$color = 'Rød';

echo 'Min yndlingsfarve er' . $color . 'og din?';
```

Gode vaner
--------------------------

Generelt anbefaler jeg at bruge apostroffer (hvis det er muligt) til at afgrænse noget, da de er langt mindre almindelige i strenge end anførselstegn.

Desuden er PHP et websprog, dvs. det bruges til at generere HTML-dokumenter, hvor anførselstegn er meget almindelige, netop fordi de også bruges til at generere dele af HTML-koden. Personligt anbefaler jeg, at du vænner dig til at bruge apostroffer overalt, for så behøver du ikke at huske, hvad du omslutter.

Adfærd med anførselstegn
--------------------------

Pas på! Du må ikke helt fjerne anførselstegnene! De har nogle særlige fordele, som kan være nyttige i avanceret PHP-arbejde - men nybegyndere betragter dem som fejl og forstår dem ikke.

```php
$x = 10; // indstille en variabel
echo "Hodnota proměnné je: $x, přesně."; // og en liste
```

Inden for anførselstegn kan du også angive værdierne for variabler, eller dollartegnet gør alt efter det til en variabel. Så hvis du ikke ønsker at outputte værdien af en variabel, men dollartegnet, er du nødt til at undslippe.

```php
$price = 25; // pris i dollars
echo "Cena produktu: $price\$"; // udskriver "Produktpris: $25"
```

Fordelen ved anførselstegn er tvivlsom i dette tilfælde, og det kan være bedre at bruge apostroffer og blot linke strengene.

```php
$price = 25;
echo 'Produktpris:' . $price . '$'; // udskriver det samme som i det foregående eksempel
```

> **Note:** Prisen i dollars er korrekt skrevet i formatet `$25`, hvilket ville skabe endnu mere forvirring, fordi notationen `$$$price` faktisk kalder noget, der kaldes en `variabelvariabel` (i variabelnavnet kalder vi navnet på den variabel, der skal kaldes - bare forvirring).

**TIP:** Du er måske interesseret i <a href="/promenna-variabel">variabel variabel</a>.

String betegnelse
--------------------------

Generelt behandles alt i anførselstegn eller apostroffer som en streng. Således:

```php
$x = 5;   // dette er noget andet,
$x = "5"; // end dette.
```

I det første tilfælde gemmes **tallet** 5 i variablen **$x**, i det andet tilfælde gemmes **strengen** "5" i den samme variabel. Heldigvis er dette ligegyldigt i PHP, og du kan arbejde med begge varianter (næsten) på samme måde, fordi PHP automatisk **transformerer** variabler i overensstemmelse med deres indhold. Det anbefales dog generelt ikke at skrive tal i anførselstegn, især ikke i forbindelse med beregningsoperationer, hvor der kan opstå afrundingsfejl.

Nogle gange ønsker jeg at gemme resultatet af en funktion i en variabel:

```php
$pi = 3.14159;
$zaokrouhleni = round($pi); // dette er korrekt
$zaokrouhleni = "round($pi)"; // dette er "forkert" (spørgsmålet er, hvilket output jeg forventer).
```

Fejlen i det andet tilfælde er kun i anførselstegnene, da resultatet af **round()**-funktionen ikke gemmes i variablen **$round**, men i den streng, der kalder denne funktion.
> **TIP:** Værdien `$pi` behøver ikke at blive indtastet direkte i scriptet på denne måde, og vi kan bruge funktionen `pi()`, som er mere præcis til mere komplekse beregninger.

Særlig betydning af at undslippe
--------------------------

> **Varsel:** De følgende eksempler virker kun i anførselstegn, hvis du indrammer dem i apostroffer, opfører de sig som almindelige tegn uden særlig betydning (undtagen `\'`, som undslipper apostrof).
Escaping er til, når jeg ønsker at udskrive et specialtegn inden for anførselstegn eller en apostrof, som kan fortolkes som et sprogudtryk og derfor behandles, selv om programmøren ikke havde til hensigt at gøre det. Vi har allerede vist et eksempel; i dette afsnit beskrives mulige adfærdsmæssige undtagelser.

Det skyldes, at det at undslippe i sig selv nogle gange har en særlig betydning. Eksempel:

```php
echo "Lang tekst, opdelt i to linjer.";
```

Det foregående eksempel udskriver:

```php
Dlouhý text, rozdělený na
dva řádky.
```

Så hvis vi ønsker at skrive en skråstreg, skal vi også undvige den (det er ikke nødvendigt at undvige `n`-tegnet i dette tilfælde, fordi det ville blive opfattet som en ombrydning igen, eller i dette tilfælde skal vi slet ikke undvige det):

```php
echo "Lang tekst, opdelt i to linjer.";
```

Der findes flere specialtegn som dette, f.eks. er `\t` en tabulator. Den fulde liste findes i den officielle dokumentation.

Virkelig brug af specialtegn ved undvigelse
-----------------------------------------------

Til at begynde med vil jeg gerne påpege, at næsten alt kan gøres i PHP på flere forskellige måder, og de eksempler, der er givet her, er blot for at illustrere, hvordan problemet kan gribes an.

Hvis vi f.eks. ønsker at analysere tekst linje for linje, kan vi bruge funktionen <a href="/explode">explode</a>.

```php
$parser = explode("\n", $retezec); // analyserer teksten linje for linje
```

I dette tilfælde giver brugen af specialtegnet `\n` god mening, fordi vi meget effektivt kan sige, at vi ønsker at analysere efter linjeskift.

```php
$parser = explode('
', $retezec);
```

> **Varsel:** På Unix- og Windows-systemer forveksles de tegn, der bruges til at markere slutningen af linjer, med hinanden. Windows bruger f.eks. `CRLF` (et par af `\r\n`-tegn), mens Linux kun bruger `LF` (et enkelt `\n`-tegn). Når du analyserer linje for linje, skal du være opmærksom på dette. Normalt løses problemet ved at normalisere tegnene til kun at være `LF`.

Resumé
-------

Hvis du kan, skal du bruge **apostroffer** overalt.

Det er godt at kende brugen af anførselstegn og kun bruge dem, når det er nødvendigt (eller generelt godt). Hvis du udsender tekst, der kan indeholde anførselstegn, skal du omslutte den med apostroffer (som så opfører sig mere forudsigeligt). Personligt bruger jeg anførselstegn til at udtrykke forskellige specialtegn, som det er unødigt kompliceret at indtaste i apostroffer og ville kræve kompliceret escaping.
