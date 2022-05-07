Envoi d'e-mails (fonctions mail() et SMTP) en PHP
=================================================

> id: '3051b5ea-9408-4aaa-8209-e3c527ef8ad2'
> slug:
> 	cs: odesilani-emailu-mail-smtp
> 	fr: envoi-d-e-mails-fonctions-mail-et-smtp-en-php
> 
> perex: 'Možnosti odesílání e-mailů v PHP, funkce mail(), SMTP, hlavičky, konfigurace a Nette Mailer.'
> publicationDate: '2019-11-26 12:00:02'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '53dc8075e16053ea9284299987c64952'

En PHP, nous avons deux façons d'envoyer des mails :

- La fonction native `mail()`, qui a pas mal de limitations,
- ou via un serveur SMTP.

La fonction `mail()` doit utiliser le serveur SMTP, ce qui est une façon très simple d'envoyer du courrier par le biais du serveur SMTP.
---------------

L'idée d'utilisation est simple : vous appelez la fonction :

```php
mail('jan@barasek.com', 'Sujet', 'Le texte du message...');
```

Et PHP fera l'envoi lui-même.

En interne, l'envoi fonctionne en lisant la configuration de `php.ini` et en cherchant le serveur SMTP par défaut pour délivrer le courrier. Il faut donc que le serveur web soit d'abord configuré.

Le principal écueil de la fonction `mail()` est que le programmeur doit trouver lui-même toute la logique. Cela implique, par exemple, de supprimer les en-têtes concernant le cryptage, de lier les certificats pour crypter les messages, etc.

En cas d'échec de l'envoi, une valeur `false` est retournée, que nous devons attraper et traiter nous-mêmes. Nous pouvons trouver l'erreur spécifique d'une manière limitée en appelant `error_get_last()`, ainsi par exemple :

```php
if (@mail($to, $subject, $message) === false) {
	throw new \Exception(
		'Impossible d'envoyer du courrier :'
		. (@error_get_last()['message'] ?? '')
	);
}
```

> **TIP:** Notez que nous n'avons pas précisé l'adresse à partir de laquelle nous voulons envoyer le courrier et l'encodage à utiliser.
>
> Tous ces paramètres doivent être transmis via des en-têtes.

Si vous avez toujours besoin d'utiliser la fonction `mail()` (par exemple, à cause de l'hébergement), je recommande d'utiliser le paquet `nette/mail` et le service `SendmailMailer`, qui gère bien l'envoi de courrier.

Serveur SMTP
-----------

SMTP signifie `Simple Mail Transfer Protocol`, ce qui (comme vous le verrez bientôt) est très vrai.

SMTP, contrairement à `mail()`, est un protocole plus avancé avec des options de configuration avancées non seulement du côté de PHP, mais aussi directement sur le serveur de messagerie.

La prise en charge du protocole SMTP sur les hôtes est excellente en 2018.

Le SMTP fonctionne essentiellement par le fait que PHP établit d'abord une connexion avec le serveur SMTP (cela nécessite l'extension `php_openssl.dll` en PHP, que vous avez probablement déjà activée), s'authentifie (vérifie l'exactitude des informations de connexion) pendant la connexion, et ensuite nous pouvons communiquer avec le serveur de la même manière qu'avec une base de données - c'est-à-dire envoyer des requêtes individuelles, mais garder une seule connexion à tout moment. L'un des grands avantages du SMTP est la prise en charge directe du cryptage (connu sous le nom de `TLS`).

Envoi d'e-mails à partir de localhost - une solution simple
--------------------------------------------------

J'ai souvent besoin d'envoyer des courriels à partir de localhost lorsque je teste une nouvelle application.

> **Pour le conseil:**
>
> Sur un Mac, la situation est simple car le serveur MAMP trouve "comme par magie" le compte Apple Mail actuellement connecté et les messages sont toujours envoyés à partir de ce compte.

Toutefois, vous ne pouvez pas toujours compter sur ce comportement et il est bon de mettre en place votre propre solution. Si vous disposez d'une connexion Internet et d'un compte Google, il est très facile d'utiliser un compte Gmail auquel vous pouvez vous connecter directement depuis PHP et envoyer du courrier par ce biais.

Si vous utilisez le paquet `nette/mail`, la configuration est simple :

```neon
mail:
	smtp: true
	host: smtp.gmail.com
	username: janbarasek@gmail.com
	password: *********
	secure: ssl
```

> Le mot de passe n'est pas le mot de passe de connexion de votre compte (ce ne serait pas sûr et vous ne pourriez pas utiliser **l'authentification à deux facteurs**, par exemple).
>
> Vous devez utiliser ce que l'on appelle un "mot de passe d'application", ce qui, d'un point de vue pratique, signifie que vous <a href="https://myaccount.google.com/apppasswords">enregistrez votre application</a> directement dans votre compte Google, auquel est attribué un mot de passe généré de manière aléatoire que vous saisissez dans PHP et qui peut être envoyé.
>
> Les instructions détaillées sont <a href="https://support.google.com/accounts/answer/185833?hl=cs">sur le site Web de Google</a>.

Configuration du courrier sur Wedos
---------------------------

Vous ne pouvez envoyer que 500 courriels par jour avec l'hébergement Wedos, et j'ai eu du mal avec la connexion SMTP pendant un certain temps.

A travers le paquet `nette/mail`, cela se fait comme ceci (une solution viable) :

```neon
mail:
	smtp: true
	host: smtp-*******.wedos.net
	username: jan@barasek.com
	password: ******
	secure: tls
	port: 587
```

Le paramètre `host` est différent pour chaque hébergement et se trouve dans l'email que Wedos envoie lorsque vous enregistrez l'hébergement.

Le `username` représente la boîte aux lettres à partir de laquelle les mails seront envoyés. La boîte aux lettres doit exister. Lorsque l'on envoie du courrier en PHP, il faut également définir l'envoi à la même adresse (dans Nette avec la méthode `->setFrom()`).

Si nous ne remplissons pas la configuration de manière précise et correcte, divers messages d'erreur seront affichés et les courriers ne pourront pas être envoyés.

Lorsque le nombre de messages envoyés est dépassé, une exception est levée pour informer du dépassement de la limite.
