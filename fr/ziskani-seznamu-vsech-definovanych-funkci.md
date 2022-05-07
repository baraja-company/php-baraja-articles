Obtenir une liste de toutes les fonctions définies
==================================================

> id: '99ed7887-33c8-44d7-b0cb-ac37b2336f48'
> slug:
> 	cs: ziskani-seznamu-vsech-definovanych-funkci
> 	fr: obtenir-une-liste-de-toutes-les-fonctions-definies
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: f611e5d3-ed7b-4fe9-84ca-9271fc2bd2e3
> sourceContentHash: '9dbffadf6cf6473eedadbc0bf7eccbb4'

Parfois, il peut être utile d'obtenir une liste de toutes les fonctionnalités disponibles dans l'environnement actuel. C'est notamment le cas lorsque nous gérons le serveur de quelqu'un d'autre et que nous avons besoin de nous repérer.

La liste des fonctions peut être obtenue en appelant la fonction `get_defined_functions()`, qui renvoie des données sous la forme d'un tableau :

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

La liste des fonctions est divisée en deux grandes listes.

- Les fonctions `internes` sont celles définies par PHP lui-même et les extensions installées.
- Les fonctions utilisateur (`user`) sont celles définies par le code utilisateur lui-même. Il s'agit de toutes les fonctions que nous avons écrites dans le code source, ou qui sont incluses dans les bibliothèques installées.

Cette liste peut être bien utilisée pour déboguer une application.
