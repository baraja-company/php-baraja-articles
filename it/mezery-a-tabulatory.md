Indentare il codice usando spazi e tabulazioni
==============================================

> id: '116f19ed-3753-498d-bb9e-e0f93b88c347'
> slug:
> 	cs: mezery-a-tabulatory
> 	it: indentare-il-codice-usando-spazi-e-tabulazioni
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1'
> sourceContentHash: c3af36d7df67cf36b940c9702cb71549

Per mantenere il codice facile da leggere per gli altri programmatori e mantenerlo elegante, dobbiamo imparare a formattarlo in modo uniforme. Questo articolo discute l'uso degli spazi e delle tabulazioni.

Sono meglio gli spazi o le tabulazioni per indentare il codice? Questo è spesso un argomento di discussione senza fine, se stai cercando una risposta rapida e inequivocabile, la maggior parte dei buoni programmatori preferisce usare le tabulazioni, ma cerchiamo di spezzarlo bene.

Spazi
----------------------

Ogni programmatore ed editor usa una quantità diversa di spazi per l'indentazione (ma più spesso 4), il che porta ad un codice incoerente che può essere più difficile da leggere quando si legge il codice di qualcun altro. Inoltre, sono necessari più caratteri per l'indentazione (che aumenta la sua dimensione dei dati).

Tuttavia, gli spazi hanno un vantaggio quando si rende il codice in un browser web (dove l'entità HTML `&nbsp;` è usata per l'indentazione), quindi è un formato relativamente facile da trasportare che guadagna un vantaggio solo come metodo di rendering stabile e affidabile (4 spazi appariranno sempre come 4 spazi).

Tabulatori
----------------------

Sono qualsiasi larghezza che il programmatore imposta nell'editor (se l'editor può farlo), quindi se ti piace una particolare indentazione, nessun problema - possiamo guardare lo stesso codice con diverse larghezze di tabulazione. Allo stesso tempo, è un carattere molto economico che non ha bisogno di essere ripetuto così spesso come i soli spazi.

Quando si rende il codice con tabulazioni in una pagina HTML, è consuetudine sostituire le tabulazioni con spazi fissi per assicurare la corretta visualizzazione in tutti i browser:

```php
$code = '<?php
    $a = 5+3;
    $b = 4;
    se ($a > $b) {
        echo $a . " > " . $b;
    } else {
        echo $b . " <= " . $a;
    }
?>';

echo str_replace("\t", '&nbsp;', $code);
```
