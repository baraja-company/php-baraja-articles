Obtenir l'adresse IP de l'utilisateur en PHP
============================================

> id: '1d6d761e-c139-4624-8c32-b0f2c131a831'
> slug:
> 	cs: ip-adresa
> 	fr: obtenir-l-adresse-ip-de-l-utilisateur-en-php
> 
> perex:
> 	- 'Zjištění IP adresy uživatele v PHP, ukládání IP adresy a ban uživatele. Ošetření VPN a uživatele za Proxy nebo NATem.'
> 	- 'Obtenir l''adresse IP de l''utilisateur en PHP, stocker l''adresse IP et bannir l''utilisateur. Enquête sur le VPN et l''utilisateur derrière un Proxy ou un NAT.'
> 
> publicationDate: '2020-02-28 10:30:21'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '284c4642c3fa98a026ce5a9e6625bb16'

En PHP, il est très facile de détecter une adresse IP à un niveau de base :

```php
echo 'Vous savez, votre adresse IP est' . $_SERVER['REMOTE_ADDR'] . '?';
```

> **Avertissement:** Obtenir l'adresse IP comme clé du champ `$_SERVER['REMOTE_ADDR']` n'est possible que si PHP a été appelé depuis le navigateur. En mode CLI (par exemple, exécution à partir du terminal avec cron), l'adresse IP n'est pas disponible (ce qui est logique, puisqu'aucune demande de réseau n'est effectuée).

Découverte fiable des adresses IP
-----------------------------

Après de nombreuses années de développement, j'ai finalement opté pour cette mise en œuvre :

```php
function getIp(): string
{
    if (isset($_SERVER['HTTP_CF_CONNECTING_IP'])) { // Support Cloudflare
        $ip = $_SERVER['HTTP_CF_CONNECTING_IP'];
    } elseif (isset($_SERVER['REMOTE_ADDR']) === true) {
        $ip = $_SERVER['REMOTE_ADDR'];
        if (preg_match('/^(?:127|10)\.0\.0\.[12]?\d{1,2}$/', $ip)) {
            if (isset($_SERVER['HTTP_X_REAL_IP'])) {
                $ip = $_SERVER['HTTP_X_REAL_IP'];
            } elseif (isset($_SERVER['HTTP_X_FORWARDED_FOR'])) {
                $ip = $_SERVER['HTTP_X_FORWARDED_FOR'];
            }
        }
    } else {
        $ip = '127.0.0.1';
    }
    if (in_array($ip, ['::1', '0.0.0.0', 'localhost'], true)) {
        $ip = '127.0.0.1';
    }
    $filter = filter_var($ip, FILTER_VALIDATE_IP, FILTER_FLAG_IPV4);
    if ($filter === false) {
        $ip = '127.0.0.1';
    }

    return $ip;
}
```

Beaucoup mieux, alors :

```php
echo 'Vous savez, votre adresse IP est' . getIp() . '?';
```

Si l'IP peut être détectée directement, ou est seulement IPv6, ou est en mode CLI (par exemple cron), il retourne `127.0.0.1` (localhost).

Les implémentations qui prennent en compte les en-têtes `X-Forwarded-For` et `X-Real-IP` sont extrêmement dangereuses directement en PHP, car les données peuvent facilement être modifiées et un attaquant peut usurper une fausse adresse IP pour, par exemple, consulter l'administration ou activer le mode Debug du site (Nette Tracy). D'un autre côté, nous devons accepter certaines requêtes par procuration (par exemple, lorsque le trafic est acheminé par Cloudflare, ou lorsque Apache et Ngnix sont exécutés sur la même machine, et qu'ils sont appelés localement juste après l'autre).

Dans le cas d'un accès direct de l'utilisateur au serveur, il n'y a qu'une seule solution correcte, et c'est de s'assurer sur Apache (via l'extension `RemoteIP`) et sur Nginx via l'extension `remote_ip` que `X-Forwarded-For` est défini à partir de l'adresse IP réelle du visiteur, et que l'adresse IP ne peut pas être définie avec un en-tête HTTP.

Le champ `$_SERVER['REMOTE_ADDR']` récupère automatiquement la bonne adresse IP (c'est-à-dire l'adresse IP depuis laquelle la requête est arrivée directement à PHP) et nous n'avons pas à nous en occuper.

Accès des utilisateurs via un proxy
----------------------------

Il arrive souvent qu'un utilisateur accède par un proxy. Ensuite, l'adresse IP réelle est stockée dans la variable `$_SERVER['HTTP_X_FORWARDED_FOR']`.

Ce cas peut se produire, par exemple, lorsque le routage sur le serveur est résolu en utilisant la méthode `Ngnix -> Apache -> PHP`, où `Ngnix` sert de reverse proxy avant `Apache`. Dans ce cas, PHP ne voit que l'adresse IP du réseau interne (généralement de la forme `127.0.0.*`).

Par exemple, le service **Cloudflare** peut se comporter de cette manière, et il faut faire attention à savoir si l'on travaille avec l'adresse IP de l'utilisateur réel ou du proxy. Pour moi, le meilleur moyen est d'utiliser la fonction `getIp()` mentionnée au début de cet article. Nous pouvons assurer la détection de Cloudflare en vérifiant l'existence de la clé `$_SERVER['HTTP_CF_CONNECTING_IP']`, qui est automatiquement transmise dans chaque requête proxiée.

Détection des VPN / Proxy
-------------------

Il n'existe pas de détection fiable de l'utilisation de proxy ou de VPN, mais dans un environnement réel, nous pouvons filtrer au moins une partie du trafic.

Il existe plusieurs façons de procéder : Prenez une série d'adresses IP et comparez l'adresse IP d'où provient la demande.

Chez certains fournisseurs de VPN, les listes d'adresses IP sont disponibles officieusement (voir par exemple https://gist.github.com/JamoCA/eedaf4f7cce1cb0aeb5c1039af35f0b7), dans le cas des nœuds de sortie de Tor officiellement (https://blog.torproject.org/changes-tor-exit-list-service, mais les ponts Tor n'y sont pas).

Une autre option consiste à faire une demande en ligne quelque part, ce qui peut à la fois retarder le chargement de la page si le service ne fonctionne pas et également faire "fuir" les adresses IP des visiteurs vers un tiers. À partir de 2023, je déconseille fortement cette approche, car il s'agit davantage de protéger et de manipuler les données des utilisateurs.

Cette interrogation en ligne peut être "naïve" et il suffit de voir qui possède la gamme ou s'il s'agit d'un proxy/VPN (certains services peuvent renvoyer ces informations, mais par défaut, elles ne font pas partie des "informations IP", par exemple celles d'un service whois).

(Le plus) souvent, une sorte d'évaluation de la réputation est utilisée, certaines adresses IP étant plus "sales" que d'autres. Statistiquement, il y a plus de merde provenant de divers proxies, VPN et Tor que d'adresses IP domestiques (sauf peut-être les adresses IP domestiques "infectées"). Une telle évaluation de la réputation est proposée par certaines listes de blocage DNS, voir une liste aléatoire, https://en.m.wikipedia.org/wiki/Comparison_of_DNS_blacklists et la colonne "Listing goal", ou elle est fournie directement par des sociétés comme Cloudflare sous la forme de "gestion des robots", etc.

Cela dépend beaucoup de l'objectif de la détection.

Stockage des adresses IP
------------------

Cela dépend de l'adresse IP dont vous disposez.

- L'adresse IPv4 peut être stockée dans 4 octets, la fonction `ip2long` est utilisée pour cela,
- Cependant, pour une adresse IPv6, nous devons utiliser 16 octets et il n'existe pas de fonction de conversion.

Si votre serveur de base de données ne supporte pas directement un type de données pour l'adresse IP, je recommande de stocker l'adresse IP en tant que `varchar(39)`, où les deux versions tiendront dans une chaîne et seront lisibles par l'homme.

> Lorsque vous stockez l'adresse IP, demandez-vous s'il est judicieux de stocker également le nom de domaine détecté par la fonction `gethostbyaddr`. Vous ne pouvez pas trouver les noms lors de l'inscription et de la recherche, car cela prend beaucoup de temps et ils peuvent changer au fil du temps.

Blocage de l'adresse IP du visiteur
-----------------------------

La solution idéale consiste à créer une liste d'adresses IP bloquées et à comparer cette liste avec l'adresse IP actuelle à chaque demande. Si les adresses correspondent, la demande sera immédiatement arrêtée.

```php
$blackList = [
    'premier-ip',
    'druha-ip',
];

if (\in_array(getIp(), $blackList, true) === true) {
    echo 'Malheureusement, votre adresse IP est bloquée :-(';
    die; // Sortie de la demande
}
```

L'exemple suppose une implémentation de la fonction `getIp()` comme dans l'exemple ci-dessus.

Une solution plus puissante consiste à vérifier l'occurrence de l'indice dans le tableau :

```php
$blackList = [
    'premier-ip' => true,
    'druha-ip' => true,
];

if (isset($blackList[getIp()]) === true) {
    echo 'Malheureusement, votre adresse IP est bloquée :-(';
    die; // Sortie de la demande
}
```

Adresse IP du serveur et nom du serveur
---------------------------------

L'adresse IP du serveur est généralement stockée dans le champ `$_SERVER['SERVER_ADDR']` et son nom peut être obtenu par la construction `gethostbyaddr($_SERVER['SERVER_ADDR'])`.

Cependant, si le concept `Ngnix -> Apache -> PHP` est utilisé et que `Ngnix` joue le rôle de reverse proxy, l'adresse IP réelle du serveur n'est pas affichée.

Dans ce cas, le nom du serveur peut être trouvé dans le champ `$_SERVER['SERVER_NAME']`, ou en utilisant la fonction `php_uname('n')`. [Documentation officielle de la fonction uname](https://www.php.net/manual/en/function.php-uname.php).

Nous pouvons alors utiliser cette astuce pour trouver l'adresse IP publique du serveur : `gethostbyname(php_uname('n'))`.
