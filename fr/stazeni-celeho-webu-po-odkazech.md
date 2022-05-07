Téléchargement de l'ensemble du site par liens en PHP
=====================================================

> id: dd4abb8e-8f9b-4867-b98f-ff1c859d387a
> slug:
> 	cs: stazeni-celeho-webu-po-odkazech
> 	fr: telechargement-de-l-ensemble-du-site-par-liens-en-php
> 
> publicationDate: '2019-11-06 17:41:30'
> mainCategoryId: f46a0d80-fbe4-4be8-a5e4-04a8d29b0afc
> sourceContentHash: e70451decff5c7bdf19bca8181c0dd5e

Il m'arrive assez souvent de télécharger toutes les pages d'un site ou d'un domaine, car j'effectue ensuite diverses mesures avec les résultats, ou j'utilise les pages pour une recherche en texte intégral.

Une solution possible est d'utiliser l'outil prêt à l'emploi [Xenu](http://home.snafu.de/tilman/xenulink.html), qui est très difficile à installer sur un serveur web (c'est un programme Windows), ou [Wget](https://www.gnu.org/software/wget/), qui n'est pas supporté partout et crée une autre dépendance inutile.

Si la tâche consiste simplement à faire une copie de la page pour la consulter ultérieurement, le programme [HTTrack] (https://www.httrack.com/) est très utile, et c'est celui que j'apprécie le plus. Seulement, lorsque l'on recherche des URL paramétrées, on peut perdre en précision dans certains cas.

J'ai donc commencé à chercher un outil qui puisse indexer automatiquement toutes les pages directement en PHP avec une configuration avancée. Finalement, cela est devenu un projet opensource.

WebCrawler de Baraja
-----------------

C'est précisément pour répondre à ces besoins que j'ai mis en place mon propre paquet Composer [WebCrawler] (https://github.com/baraja-core/webcrawler), qui peut gérer le processus d'indexation des pages de manière élégante et autonome, et si je rencontre un nouveau cas, je l'améliore.

Il est installé avec la commande Composer :

```shell
composer require baraja-core/webcrawler
```

Et il est facile à utiliser. Il suffit de créer une instance et d'appeler la méthode `crawl()` :

```php
$crawler = new \Baraja\WebCrawler\Crawler;

$result = $crawler->crawl('https://example.com');
```

Dans la variable `$result`, le résultat complet sera disponible comme une instance de l'entité `CrawledResult`, que je recommande d'étudier car elle contient beaucoup d'informations intéressantes sur l'ensemble du site.

Paramètres des chenilles
------------------

Souvent, nous devons limiter le téléchargement des pages d'une manière ou d'une autre, car sinon nous téléchargerions probablement tout l'internet.

Pour ce faire, on utilise l'entité `Config`, à laquelle on transmet la configuration sous forme de tableau (clé-valeur) et qui est ensuite transmise au Crawler par le constructeur.

Par exemple :

```php
$crawler = new \Baraja\WebCrawler\Crawler(
    new \Baraja\WebCrawler\Config([
        // clé => valeur
    ])
);
```

Options de réglage :

| Touche | Valeur par défaut | Valeurs possibles |
|-------------------------|---------------|-----------------|
| `followExternalLinks` | `false` | `Bool` : Rester seulement dans le même domaine ? Peut-il également indexer les liens externes ?
| `sleepBetweenRequests` | `1000` | `Int` : Temps d'attente entre le téléchargement de chaque page en millisecondes. |
| `maxHttpRequests` | `1000000` | `Int` : Combien d'URLs maximums téléchargées ?
| `maxCrawlTimeInSeconds` | `30` | `Int` : Durée maximale du téléchargement en secondes ?
| `allowedUrls` | `['.+']` | `String[]` : Tableau des formats d'URL autorisés sous forme d'expressions régulières. |
| `forbiddenUrls` | `['']` | `String[]` : Tableau de formats d'URL interdits sous forme d'expressions régulières. |

L'expression régulière doit correspondre à l'intégralité de l'URL exactement sous forme de chaîne.
