Informations sur la configuration de PHP et du serveur (phpinfo(), php.ini)
===========================================================================

> id: '6f4a78c1-ca06-4619-9a28-a716a65a12a8'
> slug:
> 	cs: info
> 	fr: informations-sur-la-configuration-de-php-et-du-serveur-phpinfo-php.ini
> 
> perex: 'PHP info, informace o nastavení a konfiguraci webového serveru. Změna konfigurace přes php.ini'
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '4f1d7d70-c5b0-45f1-b1d2-d03c22aa4154'
> sourceContentHash: '9ad0961571b298fb1f30ea3e67b2973e'

Nous avons souvent besoin de trouver autant d'informations que possible sur le serveur, la fonction native `phpinfo()` est idéale pour cela :

```php
phpinfo();

die;	// après avoir écrit la configuration, quittez le script
```

Cela permet de voir facilement la version installée, les extensions, les bibliothèques et bien plus encore.

> Voir la fin de cet article pour des informations sur la configuration et la modification des paramètres.

Recherche d'une section de configuration spécifique
-------------------------------------

Il est parfois utile de ne répertorier que des informations spécifiques. Nous pouvons donc définir le premier paramètre pour spécifier exactement ce qui nous intéresse :

```php
phpinfo(INFO_MODULES);
```

Des constantes prédéfinies sont utilisées pour le réglage :

| Nom de la constante | Valeur | Description
|-------------------|-----------|------
| INFO_GENERAL | 1 | Configuration générale, emplacement du php.ini, date de la dernière mise à jour, serveur Web, informations système et plus encore.
| INFO_CREDITS | 2 | Crédits PHP, pour en savoir plus voir `phpcredits()`.
| INFO_CONFIGURATION| 4 | Directives sur l'emplacement et les paramètres actuels. De plus amples informations seront fournies par la fonction `ini_get()`.
| INFO_MODULES | 8 | Informations sur les modules installés. Pour plus d'informations, consultez la fonction `get_loaded_extensions()`.
| INFO_ENVIRONMENT | 16 | Informations sur la variable `Environnement`, disponible sous la forme `$_ENV`.
| INFO_VARIABLES | 32 | Un aperçu des paramètres des variables superglobales, connues sous le nom de `EGPCS` (Environment, GET, POST, Cookie, Server).
| INFO_LICENSE | 64 | Informations sur la licence d'utilisation de PHP et autres conditions d'utilisation.
| INFO_ALL | -1 | Afficher toutes les informations (valeur par défaut)

Variable superglobale `$_SERVER`.
---------------------------------

Nous pouvons obtenir un grand nombre d'informations sur les paramètres du serveur directement pendant que le script est en cours d'exécution (par exemple, l'e-mail adressé au webmaster, l'adresse IP actuelle du visiteur ou l'URL actuellement appelée).

Il est facile de répertorier toutes les valeurs existantes :

```php
foreach ($_SERVER as $key => $value) {
    echo $key . ':' . $value . '<br>';
}
```

> **Avertissement:** Tous les index ne doivent pas nécessairement exister (par exemple, si le script exécute cron en mode CLI, l'index avec l'URL de la page ou l'adresse IP de la requête n'existera pas).

Lecture des directives de configuration spécifiques
-----------------------------------------

Une grande partie de la configuration est stockée dans le fichier `php.ini` et n'est pas accessible directement par PHP de façon normale. Par exemple, la taille maximale du fichier à télécharger.

Pour lire directement la configuration, utilisez la fonction `ini_get()` (note : cette fonction peut ne pas être activée sur tous les serveurs, ceci est particulièrement vrai pour les hôtes).

Par exemple, si nous voulons connaître la taille maximale des fichiers que nous pouvons télécharger, nous devons écrire notre propre implémentation :

```php
/**
 * @auteur Jan Barášek
 */
public static function getMaxUploadFileSize(): int
{
    $maxUpload = min(
        ini_get('taille_maximum_post'),
        ini_get('taille_maximum_des_fichiers')
    );

    if (strncmp($maxUpload, 'M', 1) === 0) {
        return (int) str_replace('M', '', $maxUpload);
    }

    return (int) $maxUpload;
}
```

Renvoie la valeur maximale de `upload_max_filesize` et `post_max_size` en Mo.

Configuration du serveur et modification des paramètres
-------------------------------------

Les paramètres eux-mêmes sont stockés dans le fichier `php.ini`. Son emplacement peut être facilement trouvé en utilisant la fonction `phpinfo()` ou en appelant la commande `php --ini`.

```shell
> php --ini

Configuration File (php.ini) Path: /etc/php/7.1/cli
Loaded Configuration File:         /etc/php/7.1/cli/php.ini
Scan for additional .ini files in: /etc/php/7.1/cli/conf.d
Additional .ini files parsed:      /etc/php/7.1/cli/conf.d/10-mysqlnd.ini,
/etc/php/7.1/cli/conf.d/10-opcache.ini,
/etc/php/7.1/cli/conf.d/10-pdo.ini,
/etc/php/7.1/cli/conf.d/20-calendar.ini,
/etc/php/7.1/cli/conf.d/20-ctype.ini,
/etc/php/7.1/cli/conf.d/20-exif.ini,
/etc/php/7.1/cli/conf.d/20-fileinfo.ini,
/etc/php/7.1/cli/conf.d/20-ftp.ini,
/etc/php/7.1/cli/conf.d/20-gd.ini,
/etc/php/7.1/cli/conf.d/20-gettext.ini
```

Sinon, le chemin peut être analysé intelligemment (fonctionne sur les systèmes Linux) :

```shell
php -r "phpinfo();" | grep php.ini
```

Il va revenir :

```shell
Configuration File (php.ini) Path => /etc/php/7.1/cli
Loaded Configuration File => /etc/php/7.1/cli/php.ini
```

> **Important:** Habituellement, la configuration est divisée en plusieurs fichiers en fonction de l'environnement et des paquets, où `php.ini` est globalement valide pour tous, alors que par exemple la configuration `CLI` n'est valide que pour le mode CLI, c'est-à-dire pour appeler un cron ou une commande depuis le Terminal.

Configuration de la taille limite des fichiers à télécharger
----------------------------------------------

Un exemple de propriété qui est souvent configurée directement dans `php.ini` est la taille maximale du fichier téléchargé (la valeur par défaut est généralement `2 Mo`, ce qui est déjà faible en 2018).

Dans le fichier de configuration, ceci est écrit comme suit, par exemple :

```shell
; Maximum allowed size for uploaded files.
upload_max_filesize = 40M

; Must be greater than or equal to upload_max_filesize
post_max_size = 40M
```

*Le point central signifie un commentaire, suivi de directives de configuration spécifiques.*
