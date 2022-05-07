Configuration de la connexion de la Doctrine Baraja
===================================================

> id: fbd0961a-53fe-4713-8526-82e36bd1fb9b
> slug:
> 	cs: konfigurace-spojeni-s-baraja-doctrine
> 	fr: configuration-de-la-connexion-de-la-doctrine-baraja
> 
> publicationDate: '2020-09-10 11:38:44'
> mainCategoryId: '818d311a-0f58-4df7-a9a4-da7d21489dd6'
> sourceContentHash: cee6f56731ae88d1de9aac0da49e0874

Pour établir une connexion à la base de données dans [Doctrine Baraja] (https://github.com/baraja-core/doctrine), vous devez utiliser le fichier de configuration Neon, qui est une partie commune du cadre Nette.

La configuration peut ressembler à ceci :

```neon
baraja.database:
    connection:
        host: localhost
        dbname: my-database
        user: root
        password: ******
```

Lorsque le conteneur DI est compilé, la configuration est vérifiée et un message d'erreur est envoyé décrivant l'erreur spécifique.

Les identifiants de connexion sont vérifiés de manière sécurisée lors de la compilation du conteneur, puis stockés physiquement dans le conteneur. Seul le service fournissant la connexion à la base de données a alors accès aux identifiants, et ceux-ci ne peuvent pas être simplement obtenus par un service externe ou un visiteur malveillant du bar Tracy.

Rétrocompatibilité
----------

Dans le passé, on utilisait des définitions utilisant des paramètres, par exemple :

```neon
parameters:
    database:
        primary:
            host: localhost
            ...
```

Cependant, ce paramètre est marqué comme **déprécié** afin d'augmenter la sécurité des applications. Lors de l'utilisation des paramètres, n'importe quel service (ou même une partie de l'application) pourrait demander des identifiants de connexion, ou la barre Tracy active sur la page pourrait les révéler.
