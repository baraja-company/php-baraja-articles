Uzyskaj listę wszystkich zdefiniowanych funkcji
===============================================

> id: '99ed7887-33c8-44d7-b0cb-ac37b2336f48'
> slug:
> 	cs: ziskani-seznamu-vsech-definovanych-funkci
> 	pl: uzyskaj-liste-wszystkich-zdefiniowanych-funkcji
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: f611e5d3-ed7b-4fe9-84ca-9271fc2bd2e3
> sourceContentHash: '9dbffadf6cf6473eedadbc0bf7eccbb4'

Czasami przydatne może być uzyskanie listy wszystkich funkcji dostępnych w bieżącym środowisku. Dzieje się tak zwłaszcza wtedy, gdy zarządzamy cudzym serwerem i musimy się zorientować w sytuacji.

Listę funkcji można uzyskać, wywołując funkcję `get_defined_functions()`, która zwraca dane w postaci tablicy:

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

Lista funkcji jest podzielona na dwie duże listy.

- Funkcje `wewnętrzne` to funkcje zdefiniowane przez sam PHP i zainstalowane rozszerzenia.
- Funkcje użytkownika (`user`) to funkcje zdefiniowane przez sam kod użytkownika. Są to funkcje, które zostały zapisane w kodzie źródłowym lub zawarte w zainstalowanych bibliotekach.

Listę tę można wykorzystać do usuwania błędów w aplikacji.
