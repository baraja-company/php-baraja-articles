Tableau de contingence en PHP
=============================

> id: '9bdc1004-8f06-48ec-8f56-8707fad5cef7'
> slug:
> 	cs: kontingencni-tabulka
> 	fr: tableau-de-contingence-en-php
> 
> publicationDate: '2019-11-13 22:00:05'
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1
> sourceContentHash: '80fdc1436cd30bc39ffe9c11c3d86c41'

Un tableau de contingence est généralement utilisé pour montrer la relation entre deux phénomènes statistiques. Lors du développement d'une application web, nous aurons souvent besoin de visualiser la relation entre un certain phénomène dans la base de données et une séquence temporelle, généralement dans l'administration.

Par exemple, nous disposons d'un tableau de commandes qui présente des produits individuels et nous sommes intéressés par la manière dont les ventes de certains produits à fort volume sont liées au temps.

Un tableau comme le suivant serait utile à cet effet :

| Datte | Pommes | Fraises | Poires | ...
|---------|--------|--------|--------|
| 2019-05 | 10 | 15 | 18 |
| 2019-04 | 12 | 18 | 11 |
| 2019-03 | 13 | 9 | 21 |
| 2019-02 | 6 | 17 | 10 |
| 2019-01 | 7 | 4 | 6 |

Il n'y a pas de moyen facile de préparer les données dans ce formulaire en PHP, et les récupérer directement dans ce formulaire en SQL n'est pas non plus élégant, car nous devons tenir compte du fait qu'il y a un nombre dynamique de colonnes.

Nous devons donc faire preuve d'intelligence lorsque nous concevons la sortie de cette structure de données.

Sérialisation des données à l'aide de clés
----------------------------

Lorsque je construis un tableau, j'utilise souvent la récupération de tous les enregistrements qui répondent à une condition donnée directement à partir de la base de données, par exemple les données d'intervalle.

Plus précisément :

```sql
SELECT *
FROM `order`
WHERE `inserted_date` <= '2019-05-01'
ORDER BY `inserted_date` DESC
```

La requête récupère toutes les colonnes de la table order (`order`), en filtrant tous les enregistrements depuis le début des âges jusqu'à ``2019-05-01``, en les retournant triés du plus récent au plus ancien.

Avec une simple requête SQL, nous obtenons les données presque instantanément. La deuxième caractéristique intéressante est que les index de la base de données peuvent être utilisés efficacement pour compiler les résultats. Cependant, comme les données se présentent sous la forme d'un tableau simple, nous devons les sérialiser manuellement en une structure de données qui peut être convertie en un tableau de contigs.

Étant donné qu'un tableau de contingence décrit la relation entre deux ou plusieurs facteurs, il est logique d'utiliser une clé multidimensionnelle. Toutefois, comme certaines données peuvent ne pas exister pour toutes les combinaisons, il est préférable de sérialiser la clé en une seule chaîne et de stocker les données sous forme de tableau plat.

Les données peuvent être assemblées en un seul passage de boucle (la variable `$selection` contient la sortie de la base de données) :

```php
$data = [];

foreach ($selection as $row) {
    $date = date('Y-m', $row->insertedDate); // Date année-mois
    foreach ($row->items as $product) { // on passe par les produits
        $key = $date . '_' . $product->id;
        if (isset($data[$key])) {
            $data[$key]++; // existe, nous allons ajouter un autre produit
        } else {
            $data[$key] = 1; // n'existe pas, nous allons commencer le premier produit
        }
    }
}
```

Si nous explorions une structure de données plus simple, une boucle interne ne serait pas nécessaire pour parcourir les produits. Dans ce cas, l'ensemble de la construction des données pourrait être résolu en un seul cycle.

Avec cette approche, nous obtenons ce que l'on appelle un tableau plat de valeurs qui ressemble à une "clé : valeur", stockant des informations bidimensionnelles.

Le résultat est alors, par exemple (en `mai 2019`, un produit avec l'ID `10` a vendu `6` unités) :

```php
$data = [
    '2019-05_10' => 6,
    ...
];
```

Rendre les données dans un tableau - modèles
-------------------------------

Si nous avons les données sous la forme d'un tableau plat, nous pouvons rendre l'ensemble du tableau très facilement. Pour ce faire, il suffit de connaître les champs de tous les produits qui nous intéressent et les champs de toutes les dates pour lesquelles nous voulons tracer le tableau.

```php
$products = [ ... ]; // champ produit : id => nom
$dates = [ ... ]; // par date : date => étiquette

echo '<table>';
foreach ($products as $productId => $productName) {
    echo '<br>';
    foreach ($dates as $date => $dateLabel) {
        echo '<td>';
        echo htmlspecialchars(
            (string) ($data[$date . '_' . $productId] ?? '0')
        );
        echo '<td>';
    }
    echo '</tr>';
}
echo '</tableau>';
```

Notez que lorsque vous parcourez les données, vous recherchez une occurrence spécifique par pliage de clé de chaîne. Cette approche nous permet de contraindre ou d'étendre le tableau rendu de manière arbitraire en fonction des données que nous consultons. Si la donnée n'existe pas, l'opérateur ternaire `??` est évalué et zéro est affiché.

Nous pouvons construire le tableau des produits et des dates disponibles dans le cadre du premier cycle qui prépare les données. À ce stade, nous serons sûrs de ne tracer que des données qui existent réellement. Dans ce cas, il est très important que la sortie de la base de données SQL soit triée par date de création, sinon les lignes peuvent être mélangées lors du rendu final de la table.
