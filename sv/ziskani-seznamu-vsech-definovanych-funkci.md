Få en lista över alla definierade funktioner
============================================

> id: '99ed7887-33c8-44d7-b0cb-ac37b2336f48'
> slug:
> 	cs: ziskani-seznamu-vsech-definovanych-funkci
> 	sv: fa-en-lista-oever-alla-definierade-funktioner
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: f611e5d3-ed7b-4fe9-84ca-9271fc2bd2e3
> sourceContentHash: '9dbffadf6cf6473eedadbc0bf7eccbb4'

Ibland kan det vara användbart att få en lista över alla tillgängliga funktioner i den aktuella miljön. Detta gäller särskilt när vi förvaltar någon annans server och behöver orientera oss.

Listan över funktioner kan erhållas genom att anropa funktionen `get_defined_functions()`, som returnerar data i form av en array:

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

Listan över funktioner är uppdelad i två stora listor.

- De `interna` funktionerna är de som definieras av PHP självt och de installerade tilläggsfunktionerna.
- Användarfunktioner är de funktioner som definieras av användarkoden själv. Detta är funktioner som vi har skrivit in i källkoden eller som ingår i de installerade biblioteken.

Den här listan kan användas för att felsöka ett program.
