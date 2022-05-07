Échapper les caractères dans une chaîne de caractères en PHP
============================================================

> id: '40f9361e-e286-4b5e-a0c0-1f6cda8106af'
> slug:
> 	cs: escapovani
> 	fr: echapper-les-caracteres-dans-une-chaine-de-caracteres-en-php
> 
> publicationDate: '2019-11-26 11:56:52'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '026284252541bc9c918a87298b9e261c'

L'évasion est utilisée pour écrire des caractères qui ont des significations différentes dans des contextes différents.

Par exemple, nous voulons insérer un autre guillemet dans une chaîne de caractères entourée de guillemets. Comment faire ?

Il y a 2 options :

```php
echo "Les jeans Levi's"; // Combinaison de types de guillemets

echo 'Les jeans de Levi's'; // Échappement de la barre oblique inversée
```

L'échappement est également important lorsque vous écrivez des variables dans un modèle HTML, où le contenu de la chaîne peut se trouver dans un contexte différent et avoir une signification particulière.

Par conséquent, par exemple, lorsque nous listons du code HTML (que nous avons dans une variable), nous devons traiter la liste, sinon le code HTML s'exécutera.

Par exemple :

```php
$message = 'Salut <b>Tommy!</b>';

echo $message; // Faux !

echo htmlspecialchars($message); // Droit :)
```

La question de l'échappement est très complexe et je vous recommande de lire l'article <a href="https://phpfashion.com/escapovani-definitivni-prirucka">Escaping - The Definitive Guide</a> de David Grudel.
