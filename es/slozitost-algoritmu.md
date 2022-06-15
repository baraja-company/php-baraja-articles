Complejidad de los algoritmos
=============================

> id: f0baa25a-4cff-4d3d-b758-5e03f4a8c69c
> slug:
> 	- null
> 	es: complejidad-de-los-algoritmos
> 
> cs: slozitost-algoritmu
> publicationDate: '2021-08-03 20:40:00'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: '6269d38b9a2a8d75ec01b569af8b371c'

Cada algoritmo tiene su propia complejidad, que puede expresarse en notación matemática. Este resumen muestra la complejidad típica de los algoritmos según el tamaño de los datos de entrada (es decir, el número de elementos con los que trabajan) y qué tipos de algoritmos son adecuados para cada tipo de tarea.

En general, existe un mejor algoritmo especializado para cada tipo de problema. Ningún algoritmo es universalmente mejor, y siempre hay que conocer el contexto.

Notación Big O
--------------

La *notación Big O* se utiliza para clasificar los algoritmos en función de cómo aumentan sus requisitos de tiempo de ejecución o de memoria al aumentar el tamaño de la entrada.

El siguiente gráfico muestra los órdenes de crecimiento más comunes de los algoritmos especificados en notación Big O.

A continuación se presenta una lista de algunas de las notaciones Big O más utilizadas y una comparación de su rendimiento con respecto a diferentes tamaños de datos de entrada.

| Notación Big O Complejidad para 10 elementos Complejidad para 100 elementos Complejidad para 1000 elementos
| -------------- | ---------------------------- | ----------------------------- | ------------------------------- |
| **O(1)** | 1 | 1 | 1 |
| **O(log N)** | 3 | 6 | 9 |
| **O(N)** | 10 | 100 | 1000 |
| **O(N log N)** | 30 | 600 | 9000 |
| **O(N^2)** | 100 | 10000 | 1000000 |
| O(2^N)** | 1024 | 1,26e+29 | 1,07e+301 |
| O(N!)** | 3628800 | 9,3e+157 | 4,02e+2567 |

Complejidad de las operaciones con estructuras de datos
----------------------------------

| Estructura de datos, Acceso, Búsqueda, Inserción, Eliminación, Comentario, etc.
| ----------------------- | :-------: | :-------: | :-------: | :-------: | :-------- |
| **Array** | 1 | n | n | n | | |
| **Stack** | n | n | 1 | 1 | |
| **Cola** | n | n | 1 | | |
| **Lista enlazada** | n | n | 1 | n |
| **Tabla hash** | - | n | n | n | En el caso de una función hash perfecta, la complejidad será O(1) |
| **Árbol de búsqueda binario** | n | n | n | En el caso de un árbol equilibrado, la complejidad será O(log(n)). |
| **B-Tree** | log(n) | log(n) | log(n) | log(n) | log(n)
| **Árbol Rojo-Negro** | log(n) | log(n) | log(n) | log(n)
| **Árbol AVL** | log(n) | log(n) | log(n) | log(n) | log(n)
| **Filtro Bloom** | - | 1 | 1 | - | Al buscar `falso positivos` |

Complejidad de los algoritmos de clasificación
----------------------------

| Nombre del Algoritmo Mejor, Promedio, Peor, Memoria, Estable, Comentario.
| --------------------- | :-------------: | :-----------------: | :-----------------: | :-------: | :-------: | :-------- |
| **Ordenación por burbujas** | n | n<sup>2</sup> | n<sup>2</sup> | 1 | Sí | |
| **Ordenación por inserción** | n | n<sup>2</sup> | n<sup>2</sup> | 1 | | |
| **Ordenación por selección** | n<sup>2</sup> | n<sup>2</sup> | n<sup>2</sup> | 1 | | |
| **Heap sort** | n&nbsp;log(n) | n&nbsp;log(n) | n&nbsp;log(n) | 1 | | No |
| **Merge sort** | n&nbsp;log(n) | n&nbsp;log(n) | n&nbsp;log(n) | n | Sí |
| **Ordenación rápida** | n&nbsp;log(n) | n&nbsp;log(n) | n<sup>2</sup> | log(n) | No | El ordenamiento rápido se realiza normalmente con una complejidad de pila O(log(n). ||
| **Shell sort** | n&nbsp;log(n) | depende de la secuencia | n&nbsp;(log(n))<sup>2</sup> | 1 | | No |
| **Ordenación de conteo** | n + r | n + r | n + r | Sí | r - el mayor número de la matriz |
| **Ordenación de la matriz** | n * k | n * k | n * k | n + k | Sí | k - longitud de la clave más larga |
