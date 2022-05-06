Get a list of all defined functions
===================================

> id: '99ed7887-33c8-44d7-b0cb-ac37b2336f48'
> slug:
> 	cs: ziskani-seznamu-vsech-definovanych-funkci
> 	en: get-a-list-of-all-defined-functions
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: f611e5d3-ed7b-4fe9-84ca-9271fc2bd2e3
> sourceContentHash: '9dbffadf6cf6473eedadbc0bf7eccbb4'

Sometimes it can be useful to get a list of all available features on the current environment. This is especially the case when we are managing someone else's server and need to get our bearings.

The list of functions can be obtained by calling the `get_defined_functions()` function, which returns data in the form of an array:

```
[
   internal => [
      ...,
   ],
   user => [
      ...,
   ]
]
```

The list of functions is in fact divided into two large lists.

- The `internal` functions are those defined by PHP itself and the installed extensions.
- User (`user`) functions are those defined by the user code itself. These are any functions that we have written into the source code, or that are included in the installed libraries.

This list can be well used to debug an application.
