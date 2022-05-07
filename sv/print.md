Skriv ut
========

> id: '23d672c5-b347-43b8-bd08-8ccb7ecca33f'
> slug:
> 	cs: print
> 	sv: skriv-ut
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: b7f7c3c32d9f3c1efd794ca38b7db384

`print` - Strängutgång

Beskrivning
--------------------------

```php
print 'Hej, världen!';
```

`print()` är egentligen ingen riktig funktion (det är en språklig konstruktion), så du behöver inte använda parenteser.

Parametrar
--------------------------

- **arg**

Utgångsparameter

Returvärden
--------------------------

Återger alltid siffran 1.

Obs
--------------------------

Observera: Eftersom detta är en **språkskonstruktion** (inte en funktion) kan den inte laddas in i en variabel.

Exempel
--------------------------

```php
print "Hej, världen";

print "print kan skriva ut flera rader text.Men se upp för HTML-tagget
eftersom det inte kan skrivas ut. Det är vad <a href="/nl2br">Nl2br</a>.";

// Exempel på anslutning med variabel
$a = 'php';

print 'Jag gillar' . $a; // Jag gillar php
```

**print** är exakt samma funktion som **echo**. Om du vill ha mer information kan du läsa artikeln <a href="/echo">echo</a>.
