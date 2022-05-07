Fin()
=====

> id: '879c7c9a-a497-4080-865d-f15e6c9e8578'
> slug:
> 	cs: end
> 	fr: fin
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '2d097dd1fae4e4f125d879c92bc2cfbe'

- Prise en charge de PHP 4, PHP 5
- Courte description : fixe le pointeur interne d'un tableau à son dernier élément.
- Exigences.

Description
--------------------------

La fonction `end()` place un pointeur de tableau interne sur le dernier élément et retourne sa valeur.

Fonctions similaires
--------------------------

- `courant()`
- `each()`
- `prev()`
- <a href="/reset">reset()</a>
- `next()`

Exemple
--------------------------

```php
echo end([
    'pomme',
    'banane',
    'canneberge',
]); // écrit cranberry
```



```php
$a = [];
$a[1] = 1;
$a[0] = 0;

echo end($a); // imprime 0
```

Paramètres
--------------------------

| # | Type | Description |
| --- | ------- | ----- |
| 1 | `array` | Le nom du tableau avec lequel on doit travailler. |

Valeurs de retour
--------------------------

Retourne la valeur du dernier élément ou `false` pour un tableau vide.

Différences par rapport aux versions précédentes
--------------------------

Aucun
