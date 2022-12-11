Communication via SSH et clé RSA2
=================================

> id: '09ec87dd-810d-4618-b76b-fd30f63be30a'
> slug:
> 	cs: ssh-klic
> 	fr: communication-via-ssh-et-cle-rsa2
> 
> publicationDate: '2022-11-12 15:00:00'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '4427ec2be03799f94a860d04be6a72ff'

SSH est un protocole réseau pour le transfert crypté de fichiers et de terminaux. SSH est le plus souvent utilisé pour le contrôle à distance d'un serveur web et le transfert sécurisé de fichiers. Contrairement au FTP, il offre une connexion cryptée native. SSH communique sur le port par défaut `22`. La connexion est initialisée avec le nom d'utilisateur et l'adresse du serveur de destination. Un mot de passe (non recommandé) ou une clé SSH RSA2 (recommandée) peuvent être utilisés pour l'authentification.

Obtention (génération) de la clé
--------------------------

Avant de nous connecter au serveur, nous devons obtenir (ou générer) notre première clé SSH RSA2. Il est important que ce soit un algorithme `RSA2`. En effet, il existe un certain nombre de clés, et toutes ne peuvent être utilisées.

Sous Linux, on utilise l'utilitaire `ssh-keygen` pour la générer, auquel on spécifie la complexité de la clé (4096 dans ce cas) et l'email de l'utilisateur autorisé :

```shell
ssh-keygen -t rsa -b 4096 -C "jan@barasek.com"
```

Après avoir exécuté la commande, on nous demandera le chemin pour stocker la clé et un éventuel `password` (mot de passe d'autorisation). Ne saisissez rien comme chemin d'accès (l'emplacement par défaut est automatiquement choisi) et saisissez la phrase de passe de manière facultative (si vous le faites, vous devrez saisir le même mot de passe à chaque fois avant d'utiliser la clé).

La clé générée est automatiquement sauvegardée à l'emplacement par défaut `~/.ssh`, c'est-à-dire dans le répertoire `.ssh` du répertoire personnel de l'utilisateur actuel.

Générer une clé SSH dans Windows
-------------------------------

Malheureusement, Windows n'a pas de chemin par défaut pour la clé SSH. Il est donc idéal d'installer, par exemple, l'utilitaire `Putty` et `PuttyGen` pour générer la clé. Choisissez toujours l'algorithme `RSA2`. Une fois encore, une paire de clés sera générée, alors stockez-les en lieu sûr. Avant d'utiliser la clé SSH dans `Putty`, vous devez sélectionner le chemin du disque à partir duquel récupérer la clé. Sous Linux, ce n'est pas nécessaire, il y a un chemin d'accès au disque par défaut.

Sécurité des clés
---------------

Lorsque les clés sont générées, deux sont effectivement générées. Une clé publique (celle que vous donnez à l'autre partie pour permettre la communication) et une clé privée (celle qui n'appartient qu'à vous, que vous ne dites jamais à personne, et qui est utilisée pour décrypter la communication).

Il est essentiel de ne jamais perdre la clé privée. Le perdre, c'est rompre toute la communication !

Il est généralement recommandé de générer une paire de clés publique/privée unique pour chaque appareil et utilisateur afin de réduire la probabilité d'une fuite. Cependant, si vous voulez transférer la clé entre les appareils, vous pouvez le faire. La clé SSH est au niveau du mot de passe, donc lorsque vous la déplacez vers une autre machine, la connexion fonctionne immédiatement.

Certains serveurs se souviennent du dernier appareil avec lequel ils ont communiqué via SSH. Il est donc possible que la connexion ne fonctionne pas après avoir déplacé la clé sur le nouvel ordinateur. Dans ce cas, vous devez vider le cache des clés sur le serveur.

Autoriser la clé à se connecter au serveur
--------------------------------------

La commande `ssh` est utilisée pour se connecter au serveur. Il suffit d'entrer l'utilisateur et le domaine :

```shell
ssh root@baraja.cz
```

Ensuite, il essaiera d'établir une connexion SSH. Si vous disposez d'une clé SSH fonctionnelle et correctement configurée, la connexion sera établie automatiquement. Sinon, vous devez entrer un mot de passe.

Si vous voulez vous authentifier auprès de votre serveur en utilisant une clé SSH au lieu d'un mot de passe, vous devez transférer votre **clé publique** au serveur.

Il suffit de l'afficher avec la commande :

```shell
cat ~/.ssh/id_rsa.pub
```

Et copiez tout son contenu sur le serveur cible à l'emplacement `~/.ssh/authorized_keys`. Si vous avez plus d'une clé, chacune sur une ligne séparée.
