Le principe de l'encapsulation dans l'EPI
=========================================

> id: '54968a42-b678-4385-91ac-c13ba96c9b34'
> slug:
> 	cs: zapouzdreni
> 	fr: le-principe-de-l-encapsulation-dans-l-epi
> 
> publicationDate: '2020-02-16 21:21:35'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '3fbce6cdcbf5f274312496604569d9c2'

L'un des principaux principes de la POO est le **principe d'encapsulation**, selon lequel les problèmes complexes doivent être décomposés en de nombreux petits problèmes que nous pouvons résoudre indépendamment et simultanément. En même temps, en tant qu'utilisateurs, nous ne nous soucions pas de la manière dont cela se passe et les données (état interne) restent isolées.

Par exemple, si nous devons résoudre le problème de savoir comment retourner le résultat "1,6" à partir d'une requête d'un utilisateur avec l'expression "(5+3)*(2/(7+3))`", il est probable qu'aucun d'entre nous ne puisse écrire une seule fonction ou méthode qui résolve ce problème en une seule fois.

**TIP:** Une solution toute faite à ce type d'exemple se trouve dans l'article <a href="/pokrocila-kalkulacka">Traitement d'une expression mathématique comme une chaîne de caractères</a>, mais préparez-vous au fait que ce n'est pas facile.

L'encapsulation apporte l'abstraction sur les objets
-----------------------------------------

Grâce à l'encapsulation, vous pourrez utiliser les objets "en tant qu'utilisateur", c'est-à-dire appeler leurs méthodes sans vous soucier de leur fonctionnement interne.

Supposons que nous devions calculer le salaire d'un employé et que nous voulions utiliser une classe existante d'un autre programmeur pour le faire. Il nous suffit de connaître les paramètres obligatoires du constructeur et nous pouvons "juste utiliser" la classe :

```php
$mzda = new MzdaZamestnance(
    25000, // salaire brut
    6,     // nombre d'années dans l'entreprise
    10,    // nombre d'années d'expérience
    true   // s'agit-il d'un homme ?
);

echo $mzda->getHruba(); // 25000

echo $mzda->getCista(); // 17800
```

Les paramètres de l'objet sont fictifs et ne correspondent pas de manière réaliste à la façon dont le salaire est calculé. En particulier, ce principe est illustré par le fait que nous **n'avons besoin de connaître que l'interface publique générale** et que nous n'avons même pas besoin de nous occuper de **l'état interne de l'objet**, ni même de **la mise en œuvre interne**, et certainement pas de **la raison pour laquelle il fonctionne comme il le fait**. Nous appelons simplement la méthode `getCista()` et obtenons le gain net.

L'encapsulation est une question de conception
----------------------------

Il est important de noter que **l'encapsulation elle-même n'est pas une caractéristique ou une syntaxe du langage**. Le fait qu'une classe et une application soient encapsulées n'est qu'une question de conception de l'application par le programmeur et de réflexion sur le code.

Pensez toujours à la conception des classes de cette façon :

- KISS (keep it simple), gardez l'interface simple et ne forcez pas l'utilisateur à réfléchir inutilement. Résolvez la logique complexe pour l'utilisateur et il vous en sera reconnaissant.
- L'utilisateur de la classe (un autre programmeur ou vous du futur) n'a pas besoin de connaître la logique interne du tout et les noms des méthodes et leurs paramètres devraient suffire.
- Si j'ai besoin de calculs auxiliaires pour le calcul qui n'ont aucun intérêt pour l'utilisateur et qui sont uniquement techniques, cela n'a aucun sens de créer un getter pour eux et ils devraient seulement être calculés en interne.
- La classe doit satisfaire les propriétés de base de l'algorithme, en particulier le fait qu'il fonctionne en général pour n'importe quelles données.
- Les méthodes accessibles au public doivent être conçues de manière à fournir suffisamment d'informations pour permettre d'étendre facilement l'objet avec de nouvelles fonctionnalités à l'avenir, afin que nous puissions facilement calculer de nouvelles données à partir de ce que nous savons déjà.

Garder les données internes non publiques
-------------------------------

Pour les propriétés et les méthodes qui gèrent la logique interne, il est judicieux de définir la visibilité sur `private`. Le principal avantage est qu'ils ne seront pas appelés de l'extérieur et que l'utilisateur sera obligé d'utiliser l'interface que vous avez conçue, protégeant ainsi les données et l'état interne de l'objet.

Par exemple, prenons un objet représentant un compte bancaire sur lequel nous voulons enregistrer des paiements et traiter le solde actuel :

```php
class BankAccount
{
    private int $sum;

    public function __construct(int $startSum)
    {
        $this->sum = $startSum >= 0 ? $startSum : 0;
    }

    public function getSum(): int
    {
        return $this->sum;
    }

    public function pay(int $price): void
    {
        $newSum = $this->sum - $price;
        if ($newSum < 0) {
            throw new \Exception('Tu n'as pas ce genre d'argent !');
        }

        $this->sum = $newSum;
    }

    public function addMoney(int $money): void
    {
        $this->sum += $money;
    }
}
```

Notez que la classe ne contient qu'une seule propriété `private` `$sum`, qui contient le solde actuel.

Si nous voulons obtenir le solde actuel, il existe une méthode `getSum()` pour cela, mais nous n'avons aucun moyen de modifier la nouvelle valeur du solde. Nous ne pouvons retirer de l'argent qu'en utilisant la méthode `pay()` ou ajouter de l'argent en utilisant la méthode `addMoney()`.

Grâce à ce principe, nous avons toujours la certitude que personne ne peut briser l'objet.

Si l'utilisateur essaie de payer plus d'argent que ce qu'il y a réellement sur le compte, la méthode `pay()` ne le permettra pas car elle effectue un calcul de contrôle avant d'écraser la propriété `$sum`, et si le solde doit être négatif (inférieur à zéro), une exception d'erreur est levée et l'opération s'arrête.

Conclusion
-----

Nous avons démontré le principe de base de l'encapsulation, qui nous permet de mieux penser à l'abstraction des objets et apporte une toute nouvelle perspective.

Une fois que vous aurez bien saisi ce principe, vous verrez que <a href="/proc-use-frameworks">les frameworks commencent à avoir énormément de sens</a>, car ils encapsulent en interne beaucoup d'ingéniosité que vous pouvez simplement utiliser.

La prochaine fois, nous nous pencherons sur <a href="/dedicacy-and-visibility">dedicacy et visibilité</a>.
