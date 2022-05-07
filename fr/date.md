Fonction PHP date(), date et heure
==================================

> id: '9b0ec1c7-3431-4d7d-9bcc-6093285f14f1'
> slug:
> 	cs: date
> 	fr: fonction-php-date-date-et-heure
> 
> perex: 'Zjištění data a času, aktuální datum, formátování data a času a převod tvaru.'
> publicationDate: '2019-09-11 10:14:16'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: d0323ba88fcdba84ef45439991301f6c

La fonction `date()` est un outil permettant de travailler avec la date et l'heure. Il est utilisé dans deux cas :

- **Recherche de l'état actuel**, c'est-à-dire de la date, de l'heure, ... et l'éditer dans un format spécifique,
- **Convertir** une date spécifique dans un autre format (par exemple, le numéro du mois en nom, le format de notation de l'année, le système de 12 et 24 heures, ...).

Echantillon
------

```html
Já jsem speciální stránka. Vím, že právě je
<?php
    echo date('H:i'); // Hodina:minuta
?>
```

> AVERTISSEMENT : PHP n'imprime pas votre heure, mais l'heure du serveur. Par conséquent, il se peut que vous obteniez une heure différente de celle qui est réglée sur votre ordinateur.

Syntaxe d'entrée
--------------------------

La fonction est appelée de la manière habituelle et les demandes individuelles sont saisies comme arguments de la fonction.

```php
echo date('marques de formatage', atribut konkrétního času);
```

Les marques de formatage indiquent dans quel format la date sera imprimée. Les marques peuvent inclure des espaces, des points, des deux-points, des tirets, des traits d'union et d'autres caractères qui ne sont pas eux-mêmes des marques de formatage (si vous voulez utiliser une marque de formatage, vous devez <a href="/carriage-notes">l'échapper</a>). Vous trouverez ci-dessous un aperçu de chaque étiquette.

Le deuxième attribut (facultatif) indique la date ou l'heure saisie manuellement, qui sera convertie et éditée dans le format correspondant au premier paramètre. Il doit être spécifié en tant que *timestamp* (peut être obtenu via la balise de formatage "U").

Exemple :

```php
echo date('d. m. Y', 1405856605); // avant le 20. 07. 2014
```

Tableau des marques de formatage autorisées
--------------------------

| Caractère | Description
|------|---------------------
| `Y` | Année à quatre chiffres (par exemple 1998)
| Année sous la forme d'un nombre à deux chiffres (par exemple, 98).
| `M` | Abréviation anglaise du nom du mois (par exemple Jan)
| `m` | Numéro du mois (01-12)
| `F` | Nom du mois en anglais (par exemple, janvier)
| `D` | Abréviation anglaise du jour de la semaine (ex. : Fri)
| `l` | Nom anglais du jour de la semaine (par exemple, vendredi)
| `N` | Numéro du jour de la semaine (1 - lundi, 7 - dimanche)
| `w` | Numéro du jour de la semaine (0 - dimanche, 1 - lundi, 6 - samedi)
| `d` | Jour du mois (01-31)
| `j` | Numéro du jour du mois (1-31)
| `z` | Jour de l'année (001-365)
| `H` | Heure (00-23)
| `h` | Heure (01-12)
| `i` | Minute (00-59)
| `s` | Second (00-59)
| `U` | *Timestamp:* Nombre de secondes depuis le début du temps (depuis le 1er janvier 1970)
| `S` | Terminaison anglaise du numéro ordinal du jour du mois
| `A` | Indicateur AM/PM
| `a` | Indicateur de matin/après-midi (am/pm)
| `P` | Différence par rapport à l'heure de Greenwich (GMT) avec un séparateur entre les heures et les minutes (ajouté en PHP 5.1.3), par exemple : `+02:00`
| `g` | Heure au format 12 heures (1-12)
| `G` | Heure au format 24 heures (0-23)

Formatage de l'heure pour le plan du site
---------------------------------

Très souvent, vous devez formater l'heure du fichier `sitemap.xml`, qui contient la date et l'heure de la dernière modification de la balise `<lastmod>`.

Depuis PHP 5.1.3, la syntaxe suivante peut être utilisée pour faire cela :

```php
date('Y-m-d\TH:i:sP');
```

La date et l'heure seront affichées à la seconde près et comprendront également des informations sur le fuseau horaire où se trouve le serveur (le fuseau horaire est déterminé par le système d'exploitation du serveur).

Obtenir les noms tchèques des jours et des mois
----------------------------------

Il n'est pas possible d'obtenir les noms tchèques des jours et des mois en PHP de manière normale, nous devons donc écrire ces valeurs nous-mêmes. Le meilleur moyen est de stocker les entrées dans un tableau et de les récupérer en utilisant un appel d'index.

```php
$mesice = [
    1 => 'Janvier', 'Février', 'Mars', 'Avril', 'Mai',
    'Juin', 'Juillet', 'Août', 'Septembre', 'Octobre',
    'Novembre', 'Décembre'
];

$dny = ['Dimanche', 'Lundi', 'Mardi', 'Mercredi', 'Jeudi', 'Vendredi', 'Samedi'];
```

Cet exemple est un exemple simplifié de <a href="https://php.vrana.cz">**Jakub Vrana**</a> de l'article <a href="https://php.vrana.cz/ceske-nazvy-mesicu-a-dnu-v-tydnu.php">Noms tchèques des mois et des jours de la semaine</a>.

Traduction des dates de l'anglais à l'anglais
--------------------------------------

Nous avons souvent une date au format "Vendredi 13 septembre" et nous voulons la traduire en anglais, mais comment faire ? Le meilleur moyen est d'appeler une fonction, par exemple `datumcesky()`, à laquelle nous passons la date anglaise et qui la traduit.

```php
function datumCesky(string $date): string
{
    $men = [
        'Janvier', 'Février', 'Mars', 'Avril', 'Mai',
        'Juin', 'Juillet', 'Août', 'Septembre', 'Octobre',
        'Novembre', 'Décembre'
    ];

    $mcz = [
        'Janvier', 'Février', 'Mars', 'Avril', 'Mai',
        'Juin', 'Juillet', 'Août', 'Septembre', 'Octobre',
        'Novembre', 'Décembre'
    ];

    $date = str_replace($men, $mcz, $date);

    $den = [
        'Lundi', 'Mardi', 'Mercredi', 'Jeudi',
        'Vendredi', 'Samedi', 'Dimanche'
    ];

    $dcz = [
        'Lundi', 'Mardi', 'Mercredi', 'Jeudi',
        'Vendredi', 'Samedi', 'Dimanche'
    ];

    return str_replace($den, $dcz, $date);
}
```

Exemple d'utilisation :

```php
echo datumCesky('Vendredi 13 septembre'); // Vendredi 13 décembre
```

Cette fonction n'est certainement pas idéale pour la traduction, car elle ne fait que remplacer les mots anglais par des mots tchèques, mais elle peut être suffisante pour de nombreux déploiements. Pour les traductions plus avancées, vous devez toujours garantir la syntaxe exacte pour traduire dans un style uniforme, par exemple `Vendredi, 13 décembre`.

Trouver le premier jour du mois
-----------------------------

Le premier jour d'avril 2018 était un dimanche, mais comment le savoir facilement ?

Depuis PHP 5.1.0, il existe une solution simple :

```php
echo date('N', strtotime('2018-04-01')); // 1 (lundi), 7 (dimanche)
```

Dans les anciennes versions, nous devons implémenter cette fonction nous-mêmes :

```php
/**
 * @auteur Jan Barášek
 */
function getFirstDayPosition(?int $year = null, ?int $month = null): int
{
    $day = (int) date('w', strtotime($year . '-' . $month . '-1')) - 1;

    return $day < 0 ? 7 : $day + 1;
}
```

Comme le modificateur `w` renvoie la sortie au format US, je modifie encore le numéro du jour par un simple calcul. La fonction renvoie un nombre entier compris entre 1 (lundi) et 7 (dimanche).

Décalage horaire / conversion du formatage et validation de la date
--------------------------------------------------

Souvent, nous devons sauter d'un temps relatif (disons +5 jours), ou extraire une date de la saisie de texte d'un utilisateur pour la rendre valide. Pour ce faire, la fonction <a href="https://www.php.net/manual/en/function.strtotime.php">strtotime()</a> permet la syntaxe suivante :

```php
echo strtotime('maintenant');
echo strtotime('10 septembre 2000');
echo strtotime('+1 jour');
echo strtotime('+1 semaine');
echo strtotime('+1 semaine 2 jours 4 heures 2 secondes');
echo strtotime('jeudi prochain');
echo strtotime('lundi dernier');
```

La sortie de la fonction est **l'horodatage** *(temps universel)* de l'heure spécifiée sous forme d'un nombre entier.

> En général, je recommande de déployer cette fonction sur tous les formulaires où nous travaillons avec le temps de l'utilisateur d'une manière ou d'une autre. Ce n'est certainement pas une bonne idée de forcer l'utilisateur à écrire la date dans un format particulier, mais il faut toujours créer automatiquement un tel format pour que l'utilisateur puisse écrire ce qu'il veut. Car souvent, le texte, par exemple, est copié de quelque part et c'est trop de travail de le reformater manuellement alors que cela peut être fait automatiquement.

Nombre de secondes depuis le début du temps
--------------------------

Depuis le 1er janvier 1970, chaque seconde est ajoutée à une pour donner un énorme nombre entier qui indique le temps qui s'est écoulé depuis cette date. Cette fonction est particulièrement utile pour calculer simplement les différences de temps. C'est une solution assez fiable, mais elle comporte des risques (le problème de 2038).

**Propriétés générales de cette notation:**
- Il s'agit d'un nombre entier, donc il est facile de travailler avec,
- Il s'agit d'un système décimal, ce qui le rend facile à lire pour les humains (sauf qu'il est impossible de connaître rapidement l'heure exacte),
- Il ne peut pas être utilisé pour représenter la période antérieure au 1er janvier 1970, car il ne peut pas être négatif *(certaines nouvelles versions de l'algorithme implémentent des temps négatifs, mais vous ne pouvez pas toujours vous y fier)*,
- En 2038, il cessera de fonctionner sur les ordinateurs 32 bits et provoquera le plantage du programme (en raison d'un dépassement de pile et de la taille maximale des entiers).

Le problème de 2038
--------------------------

<h2 id="universalTime" style="background : #222 ; margin-top : 32px ; padding : 32px ; color : white ; text-align : center ; border-radius : 3px ;">Je ne lis pas...</h1>

<script>
	setInterval(function() {
		window.document.getElementById('universalTime').innerHTML = Math.floor(Date.now() / 1000) ;
	}, 250) ;
</script>

> La valeur actuelle de l'heure est affichée au-dessus de ce texte, qui est incrémenté de +1 toutes les secondes. La valeur actuelle est chargée par javascript, elle n'est donc pas toujours exacte (elle indique l'heure système de votre ordinateur).

Les ordinateurs stockent les nombres entiers sous forme binaire, et s'il s'agit d'un nombre entier, il comporte souvent 32 bits (32 uns et zéros). Le plus grand nombre possible de 32 bits est : `011 111 111 111 111 111 111 111 111 11`.

Cependant, si nous ajoutons +1 à cette constante chaque seconde, nous obtiendrons un jour un tel nombre *(ce sera le 19 janvier 2038 à 03:14:07)*. Cependant, lorsque nous essaierons d'ajouter un autre 1, la plage de bits ne sera plus suffisante (le nombre serait plus grand que ce qui peut être stocké sur 32 bits), il y aura donc un **débordement de la pile**, c'est-à-dire qu'un 1 sera ajouté au début du nombre, ce qui, sous forme binaire, signifie un **nombre négatif**, (ce qui fera probablement planter le programme), nous amenant quelque part en l'an 1901 (ugh !).

Il existe deux solutions à ce problème :

- L'extension de la gamme des nombres entiers de 32 à 64 bits, ce qui nous donne un intervalle de plusieurs milliers d'années.
- Utilisez le type de données ``DateTime`` (couramment disponible en PHP), qui fournit une approche orientée objet de la gestion des dates, est bien compatible avec la base de données, et offre une gamme d'années allant de `0001` à `9999`, ce qui est suffisant pour les besoins de la plupart des applications du monde réel.

Personnellement, je préconise d'utiliser le type de données ``DateTime`` et de ne pas utiliser du tout le stockage des entiers.

Fuseaux horaires différents
-----------------

PHP est intelligent, il essaie donc toujours d'utiliser le meilleur fuseau horaire actuel où se trouve le serveur. Cependant, il peut arriver que le fuseau horaire soit mal spécifié, ou que nous programmions une application globale à laquelle des utilisateurs du monde entier accèdent et que nous devions donc modifier manuellement le fuseau horaire.

Cela peut être fait globalement pour l'ensemble du script PHP en appelant une fonction :

```php
date_default_timezone_set('UTC');
```

Occasionnellement, vous pouvez aussi rencontrer cette solution (détecter que le serveur a un fuseau horaire autre que UTC) :

```php
$defaultTimeZone = 'UTC';

if (date_default_timezone_get() != $defaultTimeZone) {
    date_default_timezone_set($defaultTimeZone);
}
```

Modifier la date et l'heure
--------------------------

Ce n'est pas PHP lui-même qui est responsable de l'obtention de la date et de l'heure actuelles, mais le système d'exploitation sur lequel il fonctionne. Ainsi, si vous devez modifier manuellement l'heure, faites-le à cet endroit.
