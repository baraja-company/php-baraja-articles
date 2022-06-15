Obtener una lista de todas las funciones definidas
==================================================

> id: '99ed7887-33c8-44d7-b0cb-ac37b2336f48'
> slug:
> 	cs: ziskani-seznamu-vsech-definovanych-funkci
> 	es: obtener-una-lista-de-todas-las-funciones-definidas
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: f611e5d3-ed7b-4fe9-84ca-9271fc2bd2e3
> sourceContentHash: '9dbffadf6cf6473eedadbc0bf7eccbb4'

A veces puede ser útil obtener una lista de todas las funciones disponibles en el entorno actual. Esto es especialmente cierto cuando estamos gestionando el servidor de otra persona y necesitamos orientarnos.

La lista de funciones puede obtenerse llamando a la función `get_defined_functions()`, que devuelve datos en forma de matriz:

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

La lista de funciones se divide en dos grandes listas.

- Las funciones `internas` son las definidas por el propio PHP y las extensiones instaladas.
- Las funciones de usuario (`usuario`) son las definidas por el propio código de usuario. Se trata de cualquier función que hayamos escrito en el código fuente, o que esté incluida en las bibliotecas instaladas.

Esta lista puede ser bien utilizada para depurar una aplicación.
