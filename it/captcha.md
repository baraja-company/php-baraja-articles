Come funziona il Captcha (immagine descrittiva)
===============================================

> id: '9cf191e4-a49b-407b-b16e-21de7224ac43'
> slug:
> 	cs: captcha
> 	it: come-funziona-il-captcha-immagine-descrittiva
> 
> perex:
> 	- 'Captcha je druh obrázku, který ověří, že uživatel není robot a chrání webovou aplikaci. Možnosti implementace v PHP.'
> 	- Un captcha è un tipo di immagine che verifica che l'utente non sia un robot e protegge l'applicazione web. Opzioni di implementazione PHP.
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: '80357b2e7f0047bb488922f2ab7b4336'

Il captcha è attualmente uno dei modi più comuni per proteggere i formati liberi. È stato originariamente creato non per proteggere la sicurezza dei dati, ma per proteggere dallo spam e per riconoscere che è un umano.

Tuttavia, è generato dalla macchina, quindi non è sempre perfetto, ma il tasso di successo di un test captcha è da qualche parte intorno al 99% e solo l'1% delle immagini può essere decifrato da un robot spam ben fatto.

Come funziona
--------------------------

Ci sono più opzioni, per esempio io uso questa soluzione:

- Il server riceve l'informazione che l'utente ha richiesto una pagina di modulo e inizia a generarla.
- Nel futuro campo di prova captcha, mette un'immagine con un indirizzo segreto che non dice nulla (come un numero casuale, una stringa di caratteri, ... qualsiasi cosa).
- L'immagine viene generata e memorizzata a quell'indirizzo. Allo stesso tempo, la trascrizione corretta viene scritta da qualche parte (forse in una sessione o in un database).
- Quando l'utente invia il modulo, viene controllato se la trascrizione corrisponde a ciò che ha inserito. Se lo fa, il modulo viene elaborato. In caso contrario, viene richiesta una nuova descrizione di un'altra immagine.
- Dopo entrambi i tentativi, riusciti o meno, l'immagine captcha temporanea viene cancellata (per non essere mai più utilizzata). Se il modulo non viene inviato entro un certo tempo, verrà cancellato anch'esso (scade).

Trascrizione corretta
--------------------------

Se il test captcha può essere risolto, l'utente è probabilmente umano. Tuttavia, è bene pensare agli utenti che non possono risolvere il compito, specialmente gli utenti non vedenti. È una buona soluzione per combinare più test possibili (specialmente il prefetching vocale). Tuttavia, il riconoscimento vocale da parte di una macchina è attualmente molto più efficiente della lettura da un'immagine. Pertanto, questa soluzione non è sempre ideale.

Immagine captcha semplice personalizzata
--------------------------

Spesso è sufficiente generare un'immagine vuota di certe dimensioni e inserirvi alcuni caratteri in modo leggibile senza ulteriori modifiche. Seriamente! La maggior parte dei bot di spam sono stupidi e non possono attaccare moduli generici con questo tipo di protezione, anche se è un testo perfettamente leggibile che un OCR migliore può trascrivere perfettamente.

L'immagine risultante può assomigliare a questa:

```txt
<img src="captcha.php" alt="ukázková captcha">
```

Prova ad aggiornare la pagina alcune volte e vedrai che il codice cambia casualmente ogni volta. A scopo dimostrativo, è appena generato senza salvare, quindi sarà rimosso immediatamente dopo averlo caricato.

Ho risolto il codice sorgente usando la libreria PHPGD, che è disponibile praticamente su ogni installazione PHP e hosting:

```php
Header("Tipo di contenuto: image/png");
$obr = ImageCreate(100, 35);
$pozadi = ImageColorAllocate ($obr, 219, 28, 49); //definizione del colore di sfondo
$bila = ImageColorAllocate ($obr, 255, 255, 255); //definizione del colore bianco per il testo
$styl = array ($pozadi);
ImageSetStyle ($obr, $styl);
  $nahodne_cislo = rand(11111,99999); //disegnare un numero casuale di 5 caratteri
  imagestring($obr, 5, 25, 10, $nahodne_cislo, $bila); /funzione per disegnare il testo (in questo caso un numero)
ImagePNG($obr); //generazione dell'immagine in memoria e rendering
ImageDestroy($obr); //cancellare l'immagine dalla memoria (non sarà più necessaria, perché è stata generata una volta)
```

Il rendering dell'immagine è poi solo una questione di HTML:

```html
<img src="captcha.php">
```

Notate che questo script non è funzionale da solo senza ulteriori modifiche. Serve solo come dimostrazione per generare una semplice immagine.

Riassunto
--------------------------

I test Captcha sono abbastanza affidabili, ma fastidiosi e richiedono tempo. A volte non sono leggibili, quindi è bello dare all'utente la possibilità di caricare un'immagine diversa usando il javascript in modo che l'intera pagina non debba essere ricaricata e tutto debba essere riempito di nuovo.

Ricorda: Ciò che è un compito banale per un umano potrebbe non essere mai una soluzione realizzabile per una macchina. Ecco perché anche questo primitivo captcha con testo perfettamente leggibile può proteggere contro più della metà dello spam.
