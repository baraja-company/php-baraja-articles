Validation et formatage des numéros de téléphone
================================================

> id: bbfe35c3-3a8c-4fe5-a9bd-5bff0fc234e0
> slug:
> 	cs: telefonni-cisla
> 	fr: validation-et-formatage-des-numeros-de-telephone
> 
> publicationDate: '2021-06-18 09:20:00'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: '224b3857a6bb898d9d322499cd1d3757'

Il n'y a pas de moyen facile de valider et de formater les numéros de téléphone en PHP, j'ai donc écrit une bibliothèque simple qui n'a pas de dépendances, mais qui peut quand même gérer ce rôle.

Le but est de vérifier le format d'un numéro de téléphone, ou de le convertir en une forme canonique de base (qui est toujours valide).

Installation de
---------

Simplement par compositeur :

```txt
$ composer require baraja-core/phone-number
```

Ou téléchargez le paquet [Download on GitHub](https://github.com/baraja-core/phone-number).

Comment utiliser la bibliothèque
----------

Le principe de cet outil est basé sur le formatage et la validation des numéros de téléphone provenant de l'utilisateur, ou de sources sur lesquelles vous n'avez aucun contrôle.

L'utilisation la plus courante consiste à corriger le formatage des numéros de téléphone :

```php
$original = '+420 777123456';
$formatted = \Baraja\PhoneNumber\PhoneNumberFormatter::fix($original);

echo $original . '<br>';
echo $formatted;
```

La fonction corrige le formatage des nombres et renvoie la chaîne `+420 777 123 456`.

Si aucun préfixe n'est spécifié par l'utilisateur, le préfixe `+420` est supposé. Vous pouvez modifier la préférence par défaut à l'aide du deuxième paramètre :

```php
// retours : +421 777 123 456
\Baraja\PhoneNumber\PhoneNumberFormatter::fix('+420 777123456', 421);
```

Le code du téléphone n'est écrasé que si l'utilisateur ne le saisit pas et s'il n'est pas détecté automatiquement.

Formatage des entrées et des sorties
----------------------------

La chaîne d'entrée peut ressembler à (presque) n'importe quoi. L'algorithme intégré peut supprimer automatiquement les caractères non valides (par exemple, certains utilisateurs écrivent une note à côté d'un numéro de téléphone qui sera supprimée automatiquement). Vous n'avez donc pas à vous soucier du formatage de l'entrée, mais la sortie sera toujours cohérente.

La sortie a toujours le même aspect (elle est normalisée au format canonique).

Le format général est le suivant :

```txt
   +420 777 123 456
     |  \_________/
  Prefix     |
      National number
```

Si vous soumettez une entrée non valide (ou une entrée qui ne peut être corrigée automatiquement), une exception sera levée.

Piégeage des erreurs
----------------

Si un nombre ne peut pas être normalisé en toute sécurité à une forme de base, ou s'il n'existe pas, lancez une exception ``InvalidArgumentException`'.

Si vous souhaitez convertir l'exception en booléen, utilisez le validateur d'actif intégré :

```php
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('123'); // faux
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('777123456'); // vrai
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('+420 777123456'); // vrai
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('+420 777 123 456'); // vrai
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('+420 77 712 34 56'); // vrai
```
