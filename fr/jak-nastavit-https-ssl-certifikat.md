Comment configurer un certificat HTTPS / SSL - guide complet
============================================================

> id: '0346a091-2b2c-494f-b2fb-573796d4fb46'
> slug:
> 	cs: jak-nastavit-https-ssl-certifikat
> 	fr: comment-configurer-un-certificat-https-ssl---guide-complet
> 
> publicationDate: '2019-09-16 09:01:35'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '67b2b3ccce4815204a4bcadce781fa59'

Lors du déploiement du protocole `https` sur des sites clients, j'ai souvent rencontré diverses difficultés qui provenaient d'un manque de compréhension des enjeux et d'une trop grande complexité des concepts.

Dans ce tutoriel, je décris en détail les étapes pour obtenir et déployer un certificat valide sur un serveur web.

**Dans chaque sous-titre, je résume toujours brièvement l'étape pour les utilisateurs avancés, et en bas, je discute des détails pour les débutants.

> **Attention : ** L'ensemble du processus de déploiement d'un certificat peut prendre plus d'une heure et est souvent intermittent (le site peut être indisponible).

Exigences d'entrée
-----------------

Les instructions supposent que nous avons accès à un serveur web terminal fonctionnant sous Linux et utilisant Apache.

Pour Nginx, toute la théorie s'applique de la même manière, mais la liaison du fichier de certificat est différente.

## Connexion au serveur web

Nous nous connectons au serveur via SSH.

- Sous Windows, je recommande le programme **Putty**,
- Sur Mac ou Linux, il suffit d'utiliser le terminal intégré.

Sur Mac ou Linux, appelez la commande :

```htaccess
ssh uživatel@server
```

Par exemple, je veux me connecter à l'utilisateur `root` sur le site `baraja.cz` :

```htaccess
ssh root@baraja.cz
```

Ou à l'utilisateur `jan` vers une adresse IP spécifique :

```htaccess
ssh jan@127.0.0.1
```

Après avoir soumis votre demande, soit la connexion sera établie directement, soit un mot de passe vous sera demandé. Rien ne s'affiche lors de la saisie du mot de passe, il faut donc confirmer le mot de passe avec la touche entrée et attendre que la connexion soit autorisée.

> **Attention : ** Si nous n'avons pas de droits sur une action, nous devons les attribuer. Soit vous passez directement à l'utilisateur `root` avec la commande `sudo su`, soit vous faites précéder la commande que vous voulez exécuter sous root du mot `root` au début, par exemple `root rm <name>` sous root supprimera le fichier `<name>`. Lorsque l'on utilise la commande `sudo`, on peut être périodiquement invité à saisir un mot de passe.

**Détails:**

L'accès SSH est mis en place par l'hébergement particulier où vous avez loué un serveur.

- Dans le cas d'un VPS, vous aurez toujours un accès SSH.
- Dans le cas de l'hébergement, il se peut que vous ne disposiez pas du tout de SSH et que la configuration se fasse différemment (généralement via l'interface web ou en contactant le support).

## Passer de HTTP à HTTPS

Si vous convertissez un site existant de `http` en `https`, vous devez garantir que tout le trafic est redirigé vers le nouveau protocole `https`.

Dans le cas d'Apache, cela peut être facilement réalisé en utilisant une redirection dans le fichier `.htaccess` :

```htaccess
<IfModule mod_rewrite.c>
	RewriteEngine On

	RewriteCond %{HTTPS} !on
	RewriteRule .? https://%{HTTP_HOST}%{REQUEST_URI} [R=301,L]
</IfModule>
```

Cette configuration fera en sorte que toutes les requêtes vers `http` seront redirigées avec le `HTTP code 301` vers `https`. Cette configuration est la configuration par défaut pour le cadre Nette, mais elle s'applique également à tous les autres cas.

**Détails:**

Le fichier `.htaccess` contient la configuration spécifique du serveur web qui affecte chaque requête. Il est généralement placé dans le même répertoire que `index.php`, ou d'autres fichiers accessibles depuis Internet.

Son paramétrage est uniquement valable pour le serveur Apache et peut être désactivé ou restreint pour certains hôtes. Pour des informations plus détaillées, contactez toujours l'hébergeur de votre site.

-----

Il arrive parfois que `.htaccess` ne veuille pas bien coopérer et que la configuration soit extrêmement difficile (par exemple pour de nombreux domaines, chacun se comportant différemment). Dans ce cas, la redirection vers HTTPS peut être gérée directement en PHP :

```php
if (empty($_SERVER['HTTPS']) || $_SERVER['HTTPS'] === 'off') {
	$location = 'https://' . $_SERVER['HTTP_HOST'] . $_SERVER['REQUEST_URI'];
	header('HTTP/1.1 301 Déplacement permanent');
	header('Emplacement :' . preg_replace('/^(https:\/\/(?:www\.)(.*)$/', '$1$2', $location));
	die;
}
```

Mettez le script dans `index.php`. Cependant, je ne recommande pas cette solution.

## Trouver les fichiers avec des hôtes virtuels

Dans le cas d'Apache, nous devons trouver le fichier Virtual hosts.

Ils sont généralement situés dans le chemin `/etc/apache2/sites-available`.

Lister le contenu du répertoire avec la commande `ls -al` et trouver le fichier où le virtuel est configuré pour notre site.

Les VirtualHosts sont généralement situés dans des fichiers avec l'extension `.conf`. La valeur par défaut est souvent dans `000-default.conf`.

**Détails:**

- Le répertoire est ouvert avec la commande `cd`, par exemple `cd /etc/apache2/sites-available`.
- Le contenu du fichier est écrit dans le Terminal à l'aide de la commande `cat <nom>`, ou édité à l'aide des commandes `nano <nom>` ou `vim <nom>`.
- On accède généralement à l'éditeur en utilisant le raccourci clavier `CTRL + X` ou en appuyant deux fois sur `ESC`.
- Les fichiers peuvent être partiellement parcourus dans l'interface graphique avec Midnight commander (commande `mc`), qui dans le cas d'Ubuntu est installé simplement avec `apt-get install mc` ou `sudo apt-get install mc`.

## Configurer un hôte virtuel pour le port 443

Dans l'hôte virtuel, nous devons en préparer un nouveau pour le port `443` (un problème courant peut être le blocage du port 443 sur le pare-feu).

Le contenu du fichier peut ressembler à ceci, par exemple :

```htaccess
<IfModule mod_ssl.c>
     <VirtualHost *:443>
         ServerName baraja.cz

         ServerAdmin jan@barasek.com
         DocumentRoot /var/www/baraja.cz/www
         Header always set Strict-Transport-Security "max-age=63072000; includeSubdomains; preload"
         ErrorLog ${APACHE_LOG_DIR}/baraja.cz.ssl.error.log
         CustomLog ${APACHE_LOG_DIR}/baraja.cz.ssl.access.log combined
         SSLEngine on
         SSLCertificateFile      /etc/ssl/baraja.cz/baraja.cz.crt
         SSLCertificateKeyFile   /etc/ssl/baraja.cz/baraja.cz.key
         SSLCertificateChainFile /etc/ssl/baraja.cz/rapidssl.crt
         SSLProtocol             all -SSLv3
         SSLCipherSuite          ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA:ECDHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA256:DHE-RSA-AES256-SHA:ECDHE-ECDSA-DES-CBC3-SHA:ECDHE-RSA-DES-CBC3-SHA:EDH-RSA-DES-CBC3-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:DES-CBC3-SHA:!DSS
         SSLHonorCipherOrder     on
         SSLCompression          off
         <FilesMatch "\.(cgi|shtml|phtml|php)$">
                 SSLOptions +StdEnvVars
         </FilesMatch>
         <Directory /usr/lib/cgi-bin>
                 SSLOptions +StdEnvVars
         </Directory>
		 BrowserMatch "MSIE [2-6]" nokeepalive ssl-unclean-shutdown downgrade-1.0 force-response-1.0
         # MSIE 7 and newer should be able to use keepalive
         BrowserMatch "MSIE [17-9]" \
         ssl-unclean-shutdown
     </VirtualHost>
</IfModule>
```

Le dossier est la zone la plus importante :

```htaccess
SSLEngine on
SSLCertificateFile      /etc/ssl/baraja.cz/baraja.cz.crt
SSLCertificateKeyFile   /etc/ssl/baraja.cz/baraja.cz.key
SSLCertificateChainFile /etc/ssl/baraja.cz/rapidssl.crt
```

Il est absolument essentiel de comprendre cette configuration. Si vous devez chercher des informations plus détaillées sur Google, utilisez les mots `SSLCertificateFile`, `SSLCertificateKeyFile` et `SSLCertificateChainFile` qui sont communs à tous les serveurs Apache.

**Avertissement:** Le problème SSL est relativement ancien et des noms différents sont souvent utilisés pour la même chose afin de maintenir une compatibilité descendante ! Il est donc important de comprendre le principe.

**Détails:**

Il est important que VirualHost contienne :

- `<IfModule mod_ssl.c>` indique que nous voulons utiliser SSL
- `<VirtualHost *:443>` que toute communication se fera sur le port 443
- `SSLEngine on` que SSL est activé pour ce VirtualHost
- Les fichiers `SSLCertificateFile`, `SSLCertificateKeyFile` et `SSLCertificateChainFile` sont des fichiers contenant des clés spécifiques.

Rien d'autre n'est nécessaire.

## Comprendre le principe de ce que nous allons faire et le fonctionnement du certificat

Dans la configuration finale de VirtualHost, nous n'aurons réellement besoin que de 3 fichiers : `SSLCertificateFile`, `SSLCertificateKeyFile` et `SSLCertificateChainFile`, que nous pouvons placer n'importe où sur le serveur (le nom et l'emplacement n'ont pas d'importance). Il est important de fournir un chemin de travail et que le contenu des fichiers soit valide.

La méthode spécifique d'obtention des certificats peut varier d'une AC à l'autre. L'important est de comprendre le principe et de l'appliquer ensuite à votre cas.

| Fichier | Signification |
|---------------------------|-------------------------------------|
| `SSLCertificateFile` | Ce certificat **est envoyé par l'autorité** |
| `SSLCertificateKeyFile` | **My generated** private key |
| `SSLCertificateChainFile` | J'ai téléchargé du web type **intermédiaire + root** |

## Obtenir la clé privée et demander le certificat

Sur le serveur, nous générons d'abord la clé privée. Le mot **privé** est important, il signifie que personne d'autre que le serveur web ne le connaît. Idéalement, il ne devrait jamais quitter le serveur et devrait être placé dans un endroit sûr. La perte de cette clé signifie une perte de sécurité, car un attaquant sera en mesure de se faire passer pour un serveur spécifique.

Pour générer la clé, utilisez la commande :

```shell
openssl req -new -newkey rsa:2048 -nodes -keyout yourdomain.key -out yourdomain.csr
```

Pour le générer, nous devons avoir le programme `openssl` installé sur le serveur, que nous pouvons obtenir par exemple en exécutant `sudo apt install openssl`.

Il peut y avoir plusieurs types de clés, dans ce cas nous générons une clé RSA de 2048 octets (`rsa:2048`).

La sortie de la commande est constituée de 2 fichiers (que vous nommez en fonction de votre domaine) :

- `votredomaine.key` - c'est la clé privée. Enregistrez le chemin vers cette clé dans l'hôte virtuel d'Apache dans `SSLCertificateKeyFile`.
- `votredomaine.csr` - il s'agit d'une `demande de certificat`, ou d'une demande d'émission d'un certificat.

Le contenu de la demande doit toujours être soumis à l'AC pour approbation. Cela se fait généralement via l'interface web de l'administration du site où les certificats sont vendus. L'approbation de la demande varie en fonction du type de certificat. Elle est le plus souvent effectuée automatiquement par un robot et prend de 5 minutes à 8 heures. Dans le cas des certificats coûteux, où la propriété physique du site et de la société exploitante est également vérifiée, la vérification est effectuée manuellement et peut prendre plusieurs jours.

Si vous êtes pressé, il existe une agence de certification `Let's encrypt` qui autorise les demandes automatiquement avec une validité de 3 mois.

> **Note : ** Dans certains cas, l'AC offre la génération automatique de demandes. Ce n'est pas une pratique recommandée car il connaît la clé privée. Dans ce cas, vous devez toujours obtenir la clé privée de l'agence et la placer dans un fichier sur le serveur de la même manière que si vous génériez la demande.

## Obtention d'une clé pour une autorité de certification/de sécurité spécifique

Dans l'intervalle, en attendant que la demande soit approuvée et que le certificat soit émis, nous allons sécuriser la **clé publique** de l'AC. Dans Apache VirtualHost, cela est représenté par le fichier `SSLCertificateChainFile` (en anglais, cela s'appelle `Intermediate and Root CA`). Cette clé est en principe publique et indique au navigateur web qui est l'AC. Ce fichier est généralement téléchargé sur le site web de l'AC ou nous est transmis par courrier électronique.

Vous devez toujours télécharger la clé correcte en fonction de l'algorithme que vous choisissez. Dans le cas de RapidSSL, par exemple, il peut être téléchargé ici : https://knowledge.digicert.com/generalinformation/INFO1548.html.

## Obtenir le certificat

Sur la base de cette demande, l'autorité de certification nous a délivré un certificat. Dans le cas d'Apache VirtualHost, il est représenté par le fichier `SSLCertificateFile`.

Il est important de noter que le certificat a une validité limitée et que nous devons répéter ce processus (idéalement avant l'expiration). Vous pouvez facilement trouver la date d'expiration dans Chrome en cliquant sur le cadenas vert dans le navigateur et en consultant les détails du certificat, dans le cas où il est communiqué par l'AC.

Après la date d'expiration, le certificat n'est plus valide et le site refusera de se connecter et les utilisateurs recevront un message d'erreur concernant la violation de la sécurité.

## Vérifier et apporter des modifications

L'exactitude des paramètres d'Apache peut être partiellement vérifiée avec la commande `apache2ctl -S`.

Pour que les changements prennent effet, Apache doit être redémarré, par exemple avec la commande :

```shell
sudo service apache2 restart
```

Si aucun message d'erreur n'est émis, nous allons immédiatement vérifier la fonctionnalité via un navigateur web (je recommande Google Chrome, qui montre le plus de détails).

En cas d'erreur, nous appelons la commande `apache2ctl -S`, qui peut montrer le chemin vers les logs, où nous pouvons voir approximativement ce qui ne va pas. Il est important d'effectuer toutes les étapes de ce manuel dans l'ordre correct et de ne pas intervertir les clés.

## Soutien futur

N'oubliez pas de marquer la date d'expiration sur votre calendrier afin de pouvoir renouveler votre certificat à temps. Certaines AC envoient un courriel de notification, mais ce n'est pas toujours fiable.

Vous pouvez effectuer le processus de renouvellement et obtenir un nouveau certificat en parallèle pendant que le certificat actuel est en cours d'exécution, puis le remplacer en même temps.

Pour un renouvellement automatique, je recommande [Certbot] (https://certbot.eff.org), qui peut surveiller automatiquement la validité et renouveler les certificats. Le renouvellement est effectué, par exemple, par un cron qui appelle une commande une fois par mois la nuit pour émettre un nouveau certificat et le déploie immédiatement.

Si vous ne comprenez pas la gestion des certificats, il est préférable d'être hébergé par une société établie qui vous fournira un hébergement fonctionnel, y compris un certificat. Cela vous évitera bien des tracas.
