File_put_contents
=================

> id: '7ec781c6-e3db-484d-8a49-0b93ab8252b1'
> slug:
> 	cs: file-put-contents
> 	da: file-put-contents
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '78211a3696ebba56d559045f8a0ab9e4'

Funktionen **file_put_contents** er velegnet til automatisk at skrive til en fil. Alternativt kan du også bruge <a href="/fopen">fopen()</a>, hvilket jeg ikke anbefaler til begyndere.

Eksempel
--------------------------

```php
$file = 'file.txt';
$content = 'Indhold, der skal gemmes i en fil.';

file_put_contents($file, $content);
```

file_put_contents har 2 parametre:

- `filename` hvor der skal skrives,
- `Indholdet af den fil`, vi vil skrive.

> Bemærk: `file_put_contents()` overskriver filen med det seneste indhold.

Pas på overskrivning
--------------------------

Hvis du gemmer via file_put_contents, skal du være opmærksom på at overskrive dataene. Funktionen sletter alt det nuværende indhold og erstatter det med det nye indhold. Så hvis du blot vil tilføje teksten, kan du enten tilføje den i begyndelsen eller slutningen ved hjælp af dit eget script:

```php
$file = 'file.txt';
$content = 'Nyt indhold.';

$oldContent = file_get_contents($file);
file_put_contents($file, $content . $oldContent);
```

Så først åbnes filen, derefter skrives det nye indhold, og derefter skrives det oprindelige indhold...

Hvis vi ønsker at tilføje det gamle indhold før det nye, skal vi blot ændre scriptet en smule:

```php
$file = 'file.txt';
$content = Nový obsah.';

$oldContent = file_get_contents($soubor);
file_put_contents($file, $oldContent . $content);
```
