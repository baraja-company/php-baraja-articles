Commentaires documentaires, tchèque ou anglais ?
================================================

> id: e5f1bf13-ab9e-4a70-a95c-77fb50aa4878
> slug:
> 	cs: dokumentacni-komentare
> 	fr: commentaires-documentaires-tcheque-ou-anglais
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1'
> sourceContentHash: a33af5a14338e9895fe87087362bd476

La rédaction de la documentation prend du temps et souvent personne ne la lit après vous, c'est pourquoi il est bon d'écrire des commentaires directement dans le code source. Cependant, une quantité importante de texte encombre inutilement le code, qui risque alors de ne pas tenir sur l'écran en même temps, ce qui réduit encore sa lisibilité.

Par conséquent, tout code doit être **auto-explicatif**, c'est-à-dire qu'en le lisant, on doit comprendre immédiatement ce qu'il fait, et il ne doit faire qu'une seule chose.

Par exemple, si une fonction est nommée `getUserProfileById($id)`, ce qu'elle fait et la valeur de retour que l'on peut en attendre sont parfaitement clairs. C'est pourquoi les commentaires sont souvent utilisés pour décrire uniquement les paramètres d'entrée et de sortie + une courte explication textuelle du principe (si quelque chose de plus complexe se passe). Lorsque le code est destiné à plusieurs personnes, il est poli d'inclure l'auteur et la date de création.

```php
/**
 * Retourne VRAI si la valeur passée est une adresse IPv6 valide.
 * Expression régulière alternative :
 * /^(((?=(?>.*?(::))(?!.+\3)))\3?|([\dA-F]{1,4}(\3|:(?!$)|$)|\2))
   (?4){5}((?4){2}|((2[0-4]|1\d|[1-9])?\d|25[0-5])(\.(?7)){3})\z/i
 * @author Jan Barasek
 */
function isIpV6(string $value): bool
{
   if (str_contains($value, ':')) {
      return (bool) filter_var($value, FILTER_VALIDATE_IP, FILTER_FLAG_IPV6);
   }

   return false;
}
```

Le commentaire indique clairement que la fonction attend une adresse IPv6 en entrée et renvoie un booléen `TRUE` ou `FALSE` en fonction du résultat de la validation.

L'annotation bootstrap est utilisée pour indiquer des valeurs à l'aide de clés normalisées, que de nombreux éditeurs peuvent traiter ultérieurement et, par exemple, indiquer les paramètres lors de l'appel de la fonction ou transmettre automatiquement les dépendances.

Les commentaires directement à l'intérieur du code ne sont utilisés que dans des cas particuliers. Si le code nécessite un commentaire interne, cela indique généralement qu'il doit être décomposé en plusieurs parties plus petites qui traiteront leur propre partie du programme.

Langues
--------------

Il est d'usage de toujours nommer les classes, méthodes, fonctions et variables en anglais (afin de rendre le code lisible pour un grand nombre de programmeurs), les clés sont relativement standardisées avec une syntaxe uniforme, donc la langue n'a pas d'importance là non plus, et pour le texte d'aide cela dépend de l'utilisation du projet. S'il s'agit d'un petit projet pour une équipe stable de personnes, l'anglais n'est pas nécessairement un problème, au contraire, au moins tout le monde comprend parfaitement la description.

Motivation pour utiliser l'annotation
-------------------

Les caractères spéciaux (indicatifs) peuvent être remarqués par d'autres outils que l'éditeur, il est donc utile, par exemple, de placer ses tests (sur quelles entrées j'attends quelle sortie) directement au-dessus de la définition de la fonction, ce qui permet de ne jamais oublier l'endroit où les tests sont écrits et en même temps, chaque programmeur a une idée immédiate de ce que fait la fonction.

Voici un petit exemple de ce à quoi pourrait ressembler une définition de test (il s'agit juste d'un concept de notation, pas d'un outil de test spécifique) :

```php
Dosadí hodnoty a vynutí jejich dump do prohlížeče / konzole:
@test Název I---> $limit => {1-{5-8}}, $city => {Praha|Kladno|Brno|Plzeň} O---> [DUMP]

Vygeneruje náhodný hash o délce 6 znaků a ověří, zda je hlavička odpovědi jakákoli, kromě 201:
@test Název I---> $hash => {a-z0-9|len:6} O---> [HEADER:***|!HEADER:201]

Vygeneruje dvojici čísel a na výstupu ověří jejich součet:
@test Sčítání čísel I---> $a => {1-5}, $b => {7-12} O---> ($return == $a+$b)

Načte náhodný článek s ID mezi 1 až 30, ověří jeho délku nebo neprázdnost:
@test Získání textu článku I---> $id => {1-30} O---> (strlen($return) > 64 || $return != NULL)

Vygeneruje náhodnou IP adresu a ověří, zda je z České republiky:
@test Geolokace I---> $ip => {1-255}.{1-255}.{1-255}.{1-255} O---> ($return['pays'] == 'FR')

Pokusí se načíst článek s vysokým ID a při testování mrkne na modifikátory (filtry):
@test Neexistující článek I---> $id => {1000000+} O---> [HEADER:4**:3**|NOCONTENT]

Vrátí počet rohů jednorožce:
@test Jednorožec I---> $maRohy => TRUE O---> 1

Kolik prstů ukazuji?
@test Prsty I---> $ukazuji => {0-10|NAME:prsty} O---> ($return == {*|NAME:prsty})
```

Ce qui serait généralement écrit, par exemple, selon la règle :

```php
@test <název_testu> I---> $<proměnná> => <hodnota> O---> [<modifikátor>:<hodnota>] (<výraz_platnosti>)
```

Dans les tests, j'ai utilisé un générateur de valeurs aléatoires utilisant des expressions régulières.
Voici quelques exemples :

```php
{<hodnota><směr_nerovnosti>}           {500+}  {250-}   {-200-(-50)}
{<začátek>-<konec>}                    {0-5}   {a-z}   {a-f0-9}
{<Hodnota1>|<Hodnota2>|<Hodnota3>}     {Praha|Kladno|Brno}
{<výraz>|<modifikátor>:<hodnota>}      {a-z0-9|len:6}  {*|len:6}
{*|<modifikátor>:<hodnota>}            {*|randomWord:5}
```
