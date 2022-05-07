Eine Liste aller definierten Funktionen abrufen
===============================================

> id: '99ed7887-33c8-44d7-b0cb-ac37b2336f48'
> slug:
> 	cs: ziskani-seznamu-vsech-definovanych-funkci
> 	de: eine-liste-aller-definierten-funktionen-abrufen
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: f611e5d3-ed7b-4fe9-84ca-9271fc2bd2e3
> sourceContentHash: '9dbffadf6cf6473eedadbc0bf7eccbb4'

Manchmal kann es nützlich sein, eine Liste aller in der aktuellen Umgebung verfügbaren Funktionen zu erhalten. Das ist besonders dann der Fall, wenn wir einen fremden Server verwalten und uns erst einmal orientieren müssen.

Die Liste der Funktionen kann durch den Aufruf der Funktion `get_defined_functions()` erhalten werden, die Daten in Form eines Arrays zurückgibt:

```txt
[
   internal => [
      ...,
   ],
   user => [
      ...,
   ]
]
```

Die Liste der Funktionen ist in zwei große Listen unterteilt.

- Die "internen" Funktionen sind diejenigen, die von PHP selbst und den installierten Erweiterungen definiert werden.
- Benutzerfunktionen (`user`) sind Funktionen, die durch den Benutzercode selbst definiert werden. Dies sind alle Funktionen, die wir in den Quellcode geschrieben haben oder die in den installierten Bibliotheken enthalten sind.

Diese Liste kann gut zum Debuggen einer Anwendung verwendet werden.
