Test des connaissances de base de Nette
=======================================

> id: d137c177-503c-4e84-8709-0e65b0ce6060
> slug:
> 	cs: test-nette-zaklady
> 	fr: test-des-connaissances-de-base-de-nette
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '28a2aef7-7490-43a9-add7-80d8a051f8a9'
> sourceContentHash: '66fb122b33aa8ce41158345bb01769ab'

Seuil de réussite : 15 points

*Vous obtenez 1 point pour chaque question à laquelle vous avez répondu correctement. Pour toute question à réponse incorrecte, vous ne recevez rien. Si la réponse n'est que partielle (et qu'il ne serait pas possible de programmer la chose sur cette base), la question est considérée comme incorrecte (il n'est pas possible d'obtenir un demi-point). Si la solution contient un bogue de sécurité, ou une coquille dans le code, ou une coquille dans le code, la réponse est considérée comme incorrecte car elle ne s'exécuterait pas*.

-----------

1 Expliquez la différence entre les boucles `for`, `while` et `foreach`. Pour chacun d'entre eux, donnez un exemple précis de son utilisation qui montre clairement son principal avantage.


2. nous avons une variable dont nous ne savons presque rien (nous ne connaissons que son nom). Comment pouvons-nous voir son contenu ? Par exemple, il s'appelle `$data`.


3. écrivez les commandes suivantes pour travailler avec le dépôt Git :
- Télécharger les dernières modifications depuis le serveur
- marquez le fichier `Statistic.php`.
- étiqueter tous les fichiers du projet
- marque tous les fichiers dans le répertoire `cron`.
- commettre des changements avec le message "My commit".
- envoi du commit au serveur


4. nous avons une chaîne de texte dans la variable. Donnez un exemple de fonction permettant de calculer la somme de contrôle.


5) Écrivez un extrait de code qui crée une action "supprimer" dans "Presenter" qui accepte l'ID de l'élément comme un nombre entier et supprime une ligne de la table "question" selon l'ID spécifié. Après une suppression réussie, il imprimera le message "La question a été supprimée" et redirigera vers l'action `list`.

Sous la question pour un extra point : Si la suppression échoue pour une raison quelconque, elle ne déclenche pas une erreur fatale, mais en informe l'utilisateur par un message (message flash).

6) Lorsque je crée un formulaire Nette, il devient un composant. Qu'est-ce qu'un composant Nette ?

J'ai besoin de créer un simple formulaire Nette pour insérer un enregistrement dans une table `question` qui contient une liste de questions. La structure du tableau est la suivante :

| Colonne | Propriétés |
|-----------|----------------------------------|
| id | int(8), unsigned, auto increment |
| question | varchar(255) |
| is_active | tinyint(1), unsigned, default value : 1 |

Créez les champs de formulaire appropriés pour insérer une nouvelle ligne dans ce tableau. Après l'insertion de l'enregistrement, un FlashMessage doit être déclenché pour informer de la réussite de l'insertion de l'enregistrement + redirection vers l'édition de l'enregistrement (action `edit`).

- Validation que le champ du formulaire a été rempli
- Validation que le texte de la question s'insère dans le varchar selon la structure de la table.
- Validation qu'une question avec ce texte n'existe plus
- Définissez une autre table personnalisée `group` pour contenir les informations sur les groupes. Lors de la création d'une question, il sera alors possible de déterminer à quel groupe appartient la question. Vous devrez mettre en place une session entre les tables (décrivez comment cela se fait et comment elle sera mise en place).
- Quelle macro Latte dois-je utiliser pour rendre le formulaire au modèle (rendu par défaut) ?

8) Ayons un formulaire d'édition dans `Presenter` qui est créé comme un composant. Nous voulons transmettre des valeurs par défaut à partir de ce qui se trouve dans la base de données, c'est-à-dire que nous devons obtenir les données de la table d'une manière pratique.
- Comment allez-vous procéder et de quels éléments du code avons-nous besoin ?
- Comment transmettre au formulaire l'identifiant de l'élément actuellement édité ?
- Comment définir l'élément de formulaire à sa valeur par défaut ?
- Comment vérifier qu'un utilisateur tente de modifier un élément qui n'existe pas dans la base de données, et comment l'en informer de manière appropriée ?

9 Considérez les données suivantes extraites d'une base de données (en utilisant une base de données Nette ordinaire) :

```php
$questions = $this->db->questions()->fetchAll();
```

- Comment lister le texte de toutes les questions sous forme de liste à puces ?
- Comment faire passer les données du tableau au modèle Latte ?
- De quelles macros Latte aurons-nous besoin pour lister les articles ? Donnez une implémentation spécifique pour lister les colonnes `id` et `name` dans le format :

	*1024 : Comment ça va ?
	*1025 : Qu'as-tu mangé au déjeuner aujourd'hui ?

10. donnez un exemple d'au moins 3 champs de formulaire différents qui sont écrits dans le formulaire :

```php
$form->add(tady bude příklad);
```

et pour chacun d'eux, expliquez à quoi il sert et quelle sortie il renvoie (type de données + exemple).


11. nous avons un formulaire Nette soumis.
- Comment faire pour que tous les champs (noms et valeurs) soient envoyés ?
- Donnez un exemple d'édition d'un champ nommé `question`.
- Fournissez une implémentation concrète du code qui parcourra un tableau de valeurs et de clés et retournera une chaîne unique contenant la liste totale des clés et des valeurs, de sorte que nous puissions, par exemple, stocker cette chaîne dans une base de données ou l'envoyer par courrier électronique (le stockage et l'envoi ne sont pas le sujet du devoir et ne seront pas évalués).


12) Pour chaque condition, décidez si le résultat est VRAI ou FAUX :
- `1 > 0`
- `1 == 1`
- `1 == "1"`
- `1 === "1"`
- `1 == true`
- `1 === true`
- `1 === false`
- `1 == "1" && 1=== true`


13) Quels sont les types de données que nous connaissons en PHP ?
- Donnez au moins 5 exemples de noms de types de données
- Pour chaque exemple, énumérez au moins 3 valeurs possibles qui peuvent être stockées dans le type de données (si le type de données ne peut pas stocker autant de valeurs possibles, écrivez-le).
- Pour chaque type de données, donnez un exemple typique de son utilisation (ce qui y est stocké en pratique).
- Quelle est la différence de condition entre `==` (deux égaux) et `===` (trois égaux) ?
- Expliquez l'inconvénient de l'utilisation de `==` dans les conditions et comment `==` résout spécifiquement ce problème (exemple où `==` peut échouer et où `==` sauve la situation).


14) Disposons d'une table de coordination (table des coordinations) qui répertorie toutes les coordinations entre 2 personnes. L'un d'eux organise la coordination et l'autre est un invité. Écrivez une sélection de base de données qui renvoie toutes les lignes avec les coordinations qui me concernent (suis-je l'organisateur de la coordination, ou suis-je l'invité de la coordination). La table a les colonnes `id`, `id_user_organizer` (id de l'organisateur), `id_user_quest` (id de l'invité). Mon ID est stocké de la manière habituelle dans `Presenter`.


15. Groupe de questions sur Latte :
- Qu'est-ce que le Latte ?
- Quelle est la différence entre `variable`, `macro`, `filtre` et `n:attribut` ? Qu'est-ce qui est utilisé où ?
- Comment créer une référence `DashboardPresenter` à une action `default` ?
- En listant un tableau de questions, comment puis-je générer une référence à une édition spécifique (`QuestionPresenter`, action `edit`) d'une question pour passer l'ID de la question actuellement listée ? Écrire un code Latte spécifique.

Symboliquement écrit (exemple en PHP, traduire en Latte) :

```php
foreach ($questions as $question) {
   echo $question->id; // ID de la question
   echo $question->question; // texte de la question
}
```

- Comment écrire un espace HTML fixe ?


16. à quoi servent les services dans Nette ?
- Comment est-il instancié ?
- Que doit faire un service pour être utilisé ?
- Comment charger un service dans Presenter ?
- Prenons par exemple le service `StatisticManager`, qui possède une méthode publique `getStatistics()` qui n'accepte aucun paramètre. Comment puis-je charger ce service dans Presenter et appeler la méthode publique `getStatistics()` dans l'action par défaut et passer le résultat au modèle ?
- Quelle est la différence entre `objet`, `classe`, `service` ?
- Qu'est-ce qu'un " modèle ", une " entité " et un " objet de valeur " ?
- Tous les services ont un gestionnaire principal qui les connaît et peuvent être récupérés dans ce gestionnaire. Quel est le nom de ce responsable ? Comment y enregistrer un nouveau service ?


17. Néon
- Que sont les fichiers Neon ?
- Quels types existent et comment sont-ils classés ?
- Que contiennent les fichiers Neon ? Quelles sont les données qui y sont stockées ?
- Donnez un exemple de la manière d'enregistrer le champ suivant (la recette est en PHP, vous devrez la traduire) afin qu'il puisse être utilisé comme paramètre :

```php
$imageGenerator = [
   "points" => [
      480: [910, 30, 1845, 1150],
      600: [875, 95, 1710, 910],
      768: [975, 130, 1743, 660]
   ]
];
```

- Donnez un exemple d'enregistrement d'un service dans Neon, auquel nous passons le paramètre `imageGenerator` que nous avons enregistré dans la tâche précédente, afin que le service le reçoive dans le constructeur et puisse l'utiliser dans le service (au sens de la configuration). Pour le service, fournissez un exemple d'implémentation du constructeur de sorte que le premier paramètre d'entrée soit traité comme le type de données du tableau.


18. les objets en général
- Que sont les "méthodes", les "propriétés" et les "constantes" ? Quelle est la différence entre eux ?
- Les méthodes et les propriétés ont trois états d'accessibilité de base (`public`, `privé`, `protégé`), expliquez la différence et un exemple spécifique d'utilisation et qui peut voir quoi et quand.
- J'ai une classe `course` dans laquelle il y a une propriété privée `currentCourse` dans laquelle le cours actuel est stocké. Comment faire pour que la propriété soit en lecture seule et non en écriture depuis l'extérieur ?


Lorsque je crée des tables dans une base de données qui sont logiquement liées (par exemple, une table pour un utilisateur et ensuite une table pour ses articles), je dois m'assurer que les données seront liées correctement.
- Qu'est-ce qui garantit que les données des tables sont correctement liées dans la base de données ?
- Qu'est-ce que l'intégrité référentielle et quel est son rôle dans la base de données ?
- Quels types de sessions avons-nous ? Quel est le but de chaque type ?
- Quels paramètres définissons-nous pour les sessions afin de gérer la suppression ou la modification des données de différentes manières ? Donnez 3 exemples et utilisations spécifiques + une description de son fonctionnement.


20) Quel est le but des fabriques (modèle de conception `OOP`) ?
- Donnez un exemple d'utilisation.
- Les services de Nette sont-ils des usines ?
- Quel est le but de l'injection de dépendances ?
- Quelle est la différence entre `DI` et `DIC` ?
