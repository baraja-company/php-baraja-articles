Ricevere dati con il metodo POST
================================

> id: c2f273fa-7730-4521-82d0-1b3096269fac
> slug:
> 	cs: metoda-post
> 	it: ricevere-dati-con-il-metodo-post
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '51a30cb759cdbb184ee5bb0bf3070af1'

L'invio di dati tramite POST è molto diverso da <a href="/metodo-get">GET</a>, è più sicuro, il testo può essere più lungo e il suo valore non può essere determinato se non da un modulo o un'intestazione (che non si ottiene per errore).

Fonte
--------------------------

Source non è molto diverso dal metodo <a href="/method-get">GET</a>. È più o meno la stessa cosa, tranne che i parametri non sono visualizzati nell'URL, solo il nome del file è visibile.

```php
echo $_POST['Articolo'] ?? '';
```

Caratteristiche, vantaggi e svantaggi
--------------------------

- I parametri non possono essere collegati, ma deve essere presentato un modulo
- Non può essere indicizzato (collegato al punto precedente)
- Più sicuro di <a href="/metodo-get">GET</a> (i dati sono inviati in modo nascosto, i valori non sono visualizzati nella storia)
- Da non confondere con SSL, il POST non è criptato
