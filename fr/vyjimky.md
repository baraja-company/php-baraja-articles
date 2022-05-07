Les exceptions et leur capture en PHP
=====================================

> id: '61b0f178-bb1c-4166-9e8a-af49de2e2a8c'
> slug:
> 	cs: vyjimky
> 	fr: les-exceptions-et-leur-capture-en-php
> 
> publicationDate: '2020-02-16 22:18:18'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: d843fe11a092943db429cb8a28384a31

Les exceptions sont les outils de la programmation orientée objet, qui offre un moyen élégant de lancer et de traiter les erreurs d'application.

Une exception est d'abord lancée (`thrown`), traitée (`try`), et attrapée (`catch`). Seul le lancer est obligatoire.

Philosophie de la génération d'exceptions
-------------------------

Avant l'apparition des exceptions, la gestion des erreurs en programmation était très compliquée car nous devions nous fier aux valeurs de retour des fonctions, les attraper à notre manière et nous comporter en conséquence.

En fait, les fonctions elles-mêmes n'appliquent pas la gestion des erreurs, ce qui peut entraîner des problèmes fatals, mais David a écrit à ce sujet dans <a href="https://phpfashion.com/programatori-chyby-neignoruji">Les programmeurs n'ignorent pas les erreurs</a>.

Un exemple de traitement des erreurs oublié :

```php
// déplacement de disque en disque
copy('c:/oldfile', 'd:/newfile');
unlink('c:/oldfile');
// si la première opération échoue, le fichier est irrémédiablement supprimé.
```

En effet, la façon correcte de traiter la sortie de la fonction `copy()` est de ne pas continuer et de lancer une erreur. Dans le cas des bonnes vieilles fonctions, cela pourrait ressembler à ceci :

```php
function backup(): bool
{
   if (copy('c:/oldfile', 'd:/newfile')) {
      return unlink('c:/oldfile');
   }

   return false;
}
```

Notre fonction `backup()` retournera `true` seulement si la fonction `copy()` n'a pas échoué et que la fonction `unlink()` a retourné `true`. Sinon, il retournera `false`.

Mais est-ce désormais sans danger pour l'application ? Ce n'est pas le cas ! Parce que maintenant nous devons traiter la sortie de la fonction `backup()` au moment où nous l'appelons, et si elle échoue, nous ne saurons même pas pourquoi. En bref, il retournera `false` et nous devrons détecter l'erreur nous-mêmes d'une manière ou d'une autre. Je suppose que dans ce cas, il est bon de voir que les programmeurs abandonnent souvent la gestion des erreurs, ou oublient simplement de gérer quelque chose et l'application lance des erreurs difficiles à détecter à cause de cela.

La solution à ce problème est d'utiliser des exceptions qui forcent le traitement, et si elles ne sont pas traitées, l'application se plante complètement et nous trouvons toujours pourquoi.

Définition de base des exceptions
--------------------------

En PHP, une exception est un type spécial d'interface implémenté par la classe native `Exception` que nous allons utiliser.

Si le traitement d'une partie du programme échoue, nous lançons simplement une exception décrivant le problème :

```php
if (copy('c:/oldfile', 'd:/newfile') === false) {
   throw new \Exception('Impossible de copier le fichier "oldfile".');
}
```

Lancer une exception se fait avec le mot-clé `throw`, suivi de la création d'une instance de la classe avec l'exception. Nous pouvons également obtenir une instance d'une autre manière (par exemple, en la passant à partir d'une variable), et la simple création d'une instance d'exception n'entraîne pas sa levée.

Le premier argument du constructeur de la classe ``Exception` accepte le texte de l'exception, qui doit expliquer de manière concise ce qui vient de se passer. Il est de bonne pratique d'inclure également des informations sur l'opération effectuée et une référence aux données. Par exemple, si la copie d'un fichier a échoué, il est bon de transmettre le nom du fichier. Si l'exécution de la requête SQL échoue, nous passons à nouveau la requête en cours d'exécution. Cela nous aidera beaucoup plus tard lors de la gestion des erreurs, car nous pourrons voir exactement quel est le problème.

Traitement des exceptions
-----------------

Par exemple, prenons une fonction `backup()` qui sauvegarde des données et peut lancer une paire d'erreurs :

```php
function backup(): void
{
   if (copy('c:/oldfile', 'd:/newfile')) {
      if (unlink('c:/oldfile') === false) {
         throw new \Exception('Impossible de supprimer l'ancien fichier.');
      }
   }

   throw new \Exception('Impossible de copier les fichiers de sauvegarde.');
}
```

Notez que la fonction ne renvoie aucune sortie, et que nous spécifions le type `void` dans la définition. La fonction n'a pas besoin de retourner quoi que ce soit, car le succès est considéré comme un état où aucune erreur n'est déclenchée et nous n'avons pas besoin de traiter un scénario positif.

Si nous devions utiliser la fonction dans une application sans traitement, par exemple, comme suit :

```php
echo 'Sauvegarde des fichiers...';
backup();
echo 'Sauvegarde terminée.';
```

C'est la façon normale de procéder. Toutefois, si une erreur se produit, le script se termine automatiquement et affiche le texte de l'exception sur la sortie. Il est important de noter qu'il ne continuera pas à exécuter le code et nous savons qu'aucune corruption de données ne se produira.

Si nous voulons continuer l'exécution, nous devons **nettoyer** l'erreur, ce que nous faisons en utilisant les constructions `try` et `catch` :

```php
echo 'Sauvegarde des fichiers...';
try {
   backup();
} catch (\Exception $e) {
   echo 'La sauvegarde a échoué :' . $e->getMessage();
}
echo 'Sauvegarde terminée.';
```

Si une exception est levée, le code de la zone `catch()` (qui accepte l'exception si elle correspond à son type de données) est appelé et le code interne est exécuté.

Nous obtenons toujours une instance de la classe d'exception, qui peut être utilisée, par exemple, pour afficher un message d'erreur, qui est géré par la méthode `getMessage()`. Il est également utile de connaître la méthode `getFile()`, qui renvoie le chemin du disque vers le fichier contenant l'erreur, `getCode()`, qui renvoie le code d'état de l'erreur, et `getLine()`, qui renvoie le numéro de ligne où l'exception a été levée.

Exceptions préparées
------------------------

En plus de l'exception de base ``Exception`, PHP inclut d'autres types d'exception prédéfinis qui sont adaptés à différents cas d'utilisation.

| Type de données | Explication |
|------------|-----------|
| `LogicException` | Erreur logique, prévisible à la conception du programme |
| `BadFunctionCallException` | Erreur d'appel de fonction ; fonction non trouvée ; appel non autorisé |
| `BadMethodCallException` | Erreur d'appel de méthode |
| `InvalidArgumentException` | Mauvais argument (invalide) passé à une fonction ou une méthode |
| `OutOfRangeException` | Valeur hors de l'intervalle du tableau ou de la collection |
| `LengthException` | La valeur dépasse la longueur autorisée |
| `DomainException` | La valeur n'entre pas dans le domaine ou l'intervalle requis |
| `RuntimeException` | Erreur uniquement détectable au moment de l'exécution (par exemple, échec de l'écriture dans un fichier) |
| `OverflowException` | Dépassement de tampon ou d'opération arithmétique, souvent causé par le traitement de plus de données que prévu |
| `UnderflowException` | Dépassement de capacité d'un tampon ou d'une opération arithmétique, moins de données ont été transmises que prévu |
| `OutOfBoundsException` | Index hors de la plage du tableau ou de la collection |
| `RangeException` | La valeur n'est pas dans la fourchette demandée |
| `UnexpectedValueException` | Valeur inattendue (par exemple, la valeur de retour d'une fonction) |

Les exceptions `LogicException` et `RuntimeException` doivent être évitées par une conception correcte du programme. Personnellement, je ne les utilise que pour des situations exceptionnelles, comme l'échec de l'écriture dans un fichier et la communication avec un service externe.

Je recommande de ne pas attraper les `RuntimeException` du tout et de laisser l'application échouer. Il s'agit généralement d'un problème grave qui doit être signalé dès que possible.
