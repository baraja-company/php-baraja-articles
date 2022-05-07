Fonction PHP Explode - diviser une chaîne de caractères par un séparateur
=========================================================================

> id: '4dc7cec6-0e96-4a6b-aee8-32c8817ba11e'
> slug:
> 	cs: explode
> 	fr: fonction-php-explode---diviser-une-chaine-de-caracteres-par-un-separateur
> 
> perex: Rozdělení řetězce na více částí podle oddělovače.
> publicationDate: '2019-11-26 11:39:36'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '66725772be4aae0df8af399e7ce2ba07'

Explode est utilisé pour diviser facilement une chaîne de caractères par un séparateur.

- Elle renvoie les résultats individuels sous la forme d'un tableau numéroté à partir de zéro,
- vous ne pouvez pas insérer un tableau, seule la chaîne de caractères est saisie,
- ne peut pas changer le délimiteur pendant l'analyse syntaxique, ne peut pas sélectionner plusieurs délimiteurs.

| PHP 4 et versions ultérieures
|---------------|-----------------|
| Brève description | Division d'une chaîne de caractères en un tableau par séparateur.
| Exigences | Aucune
| Note : Il n'est pas possible d'insérer un tableau, seulement une chaîne.

Nous avons souvent besoin de diviser une chaîne de caractères selon une règle simple. Par exemple, une liste de chiffres séparés par des virgules.

La fonction explode() est idéale pour cela. Elle prend le séparateur (qui sépare la chaîne de caractères) comme premier paramètre et les données elles-mêmes comme second paramètre :

```php
$cisla = '3, 1, 4, 1, 5, 9';
$parser = explode(',', $cisla);

foreach ($parser as $cislo) {
	echo $cislo . '<br>';
	// Ici nous pouvons traiter les nombres plus loin
}
```

Mais que se passe-t-il si les chiffres sont séparés par des virgules, mais qu'il y a des espaces autour d'eux ?

La solution consiste à analyser la plus petite sous-chaîne commune, puis à supprimer les caractères indésirables qui l'entourent (espaces et autres espaces blancs) :

```php
$cisla = '3, 1,4, 1 , 5 ,9';
$parser = explode(',', $cisla);

foreach ($parser as $cislo) {
     echo trim($cislo) . '<br>';
     // Ici nous pouvons traiter les nombres plus loin
}
```

Dans ce cas, la fonction `trim()` supprime élégamment les espaces blancs autour des caractères (espaces, tabulations, sauts de ligne, ...), ne donnant que des données propres.

Limite, limitant le nombre d'entités analysées dans un tableau.
--------------------------

> ASTUCE : Pour de nombreux exemples, explode() ne convient pas et il est bien mieux d'utiliser des expressions régulières.

Cependant, il arrive souvent que nous ne voulions analyser les données que jusqu'à une certaine distance, et le troisième paramètre (facultatif) limite peut être utilisé à cette fin.

Par exemple, dans le cas de données structurées séparées par deux points, nous voulons récupérer le contenu après le premier point et ignorer les autres points.
Exemple :

```php
$cas = 'format : "j.n.Y - H:i"';
```

Si nous devions analyser la chaîne seulement comme :

```php
$parser = explode(':', $cas);
```

Nous obtiendrions ces 3 sous-chaînes (dans d'autres cas, il pourrait y en avoir beaucoup plus) :

```php
'format'
'"j.n.Y - H'
'i"'
```

Par conséquent, nous fixons une limite au nombre d'éléments à obtenir (et éventuellement tous les autres éléments seront ajoutés à la fin du dernier élément) :

```php
$parser = explode(':', $cas, 2);

// reviennent :
echo $parser[0]; // format
echo $parser[1]; // "j.n.Y - H:i"
```

> **Note:** Les guillemets non désirés peuvent être supprimés de la chaîne de caractères, par exemple, en utilisant la fonction `trim()` :

```php
echo trim($parser[1], '"'); // le second paramètre spécifie la carte des caractères à supprimer
```

Entre, obtenir une chaîne entre deux chaînes
--------------------------

Souvent, nous avons besoin de récupérer une chaîne de caractères qui est délimitée par deux autres chaînes de caractères. Il n'existe pas de fonction pour cela directement en PHP, mais nous pouvons l'écrire nous-mêmes :

```php
function between(string $left, string $right, string $data): string
{
   $l = explode($left, $data);
   $r = explode($right, $l[1]);

   return $r[0];
}
```

Expressions régulières
--------------------------

Une division et un travail beaucoup plus élégants avec les chaînes de caractères peuvent être réalisés en utilisant <a href="/regex">les expressions régulières</a>, dont je parle sur une page séparée.

Fonctions similaires
--------------------------

- <a href="/function-implode">Implode()</a> - concaténer un tableau en une chaîne de caractères

Exemple d'analyse de l'adresse IP
--------------------------

```php
$ip = '10.0.0.138';
$parser = explode('.', $ip);
echo $parser[0]; // imprime "10"
echo $parser[1]; // imprime "0" ;
```

La variable `$ip` contient une chaîne d'entrée qui est analysée selon le délimiteur `.`, le retour est un tableau. L'analyse syntaxique se poursuit jusqu'à la fin de la chaîne, sauf si une limite est spécifiée.

Paramètres
--------------------------

| # | Type | Description
|---|--------|------|
| 1 | string | Chaîne de séparation.
| 2 | string | Chaîne de caractères analysée.
| 3 | int | limite d'analyse. Il s'agit d'un paramètre facultatif. Exemple :

```php
$text = 'PHP est mon langage préféré !';
$parser = explode('', $text, 1);

echo $parser[0]; // imprime le premier mot
echo $parser[1]; // imprime le reste du texte
echo $parser[2]; // ne produit rien car une limite a été fixée !
```

Valeurs de retour
--------------------------

La valeur de retour est un tableau contenant une chaîne de caractères analysée.

Les indices sont numérotés de zéro à `X`, sauf si une limite est spécifiée.

Différences par rapport aux versions précédentes
--------------------------

| Version de PHP | Description |
|-----------|-------|
| 5.1.0 | Ajout de la prise en charge de la limite négative sur les passes.
| 4.0.1 | Ajout d'un paramètre optionnel **limite**.

Conseils et notes
--------------------------

Lorsqu'on utilise une **limite** négative, le nombre d'éléments à partir de la fin de la chaîne est donné.

Exemple :

```php
$str = 'un|deux|trois|quatre';

// limite positive
print_r(explode('|', $str, 2));

// limite négative (depuis PHP 5.1)
print_r(explode('|', $str, -1));
```

Les champs suivants seront retournés :

```php
Array
(
    [0] => one
    [1] => two|three|four
)

Array
(
    [0] => one
    [1] => two
    [2] => three
)
```
