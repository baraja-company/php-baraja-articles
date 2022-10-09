Udskriv
=======

> id: '23d672c5-b347-43b8-bd08-8ccb7ecca33f'
> slug:
> 	cs: print
> 	da: udskriv
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: b7f7c3c32d9f3c1efd794ca38b7db384

`print` - String output

Beskrivelse
--------------------------

```php
print 'Hej, verden!';
```

`print()` er faktisk ikke en rigtig funktion (det er en sprogkonstruktion), så du behøver ikke at bruge parenteser.

Parametre
--------------------------

- **arg**

Udgangsparameter

Returneringsværdier
--------------------------

Viser altid tallet 1.

Bemærk
--------------------------

Bemærk: Da dette er en **sprogkonstruktion** (ikke en funktion), kan den ikke indlæses i en variabel.

Eksempel
--------------------------

```php
print "Hej, verden";

print "print kan udgive flere linjer tekst.Men pas på HTML-tag'et
fordi det ikke vil blive udskrevet. Det er det, som <a href="/nl2br">Nl2br</a>.";

// Eksempel på forbindelse med variabel
$a = 'php';

print 'Jeg kan godt lide' . $a; // Jeg kan lide php
```

**print** er nøjagtig den samme funktion som **echo**. Hvis du ønsker flere oplysninger, kan du læse artiklen <a href="/echo">echo</a>.
