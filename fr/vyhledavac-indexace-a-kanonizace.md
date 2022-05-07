Algorithme des moteurs de recherche Internet - Indexation et canonisation
=========================================================================

> id: daa97e66-e77f-4741-ade2-905c1cfdbd56
> slug:
> 	cs: vyhledavac-indexace-a-kanonizace
> 	fr: algorithme-des-moteurs-de-recherche-internet---indexation-et-canonisation
> 
> publicationDate: '2016-09-11 11:00:00'
> mainCategoryId: '367f936c-073f-44bd-b399-30738e93137a'
> sourceContentHash: d73a30f413e4b8b77dbceda60b3cf173

Dans la leçon d'aujourd'hui, nous allons étudier l'indexation et la canonisation des documents sur Internet.

Indexation
--------

Le processus d'indexation est effectué par un composant appelé l'indexeur. Il s'agit d'un programme spécialement conçu qui transforme les données téléchargées (les données que le Crawler a téléchargées) en un type de données spécial pour la recherche - les barils.

Le problème de l'indexation est qu'il n'est pas possible de parcourir "intelligemment" les documents, mais la lecture séquentielle (lecture du texte entier du début à la fin) est inévitable. Il s'agit donc d'une discipline exigeante et les moteurs de recherche utilisent les serveurs les plus puissants pour cette activité. Aucune autre tâche du processus de recherche n'est aussi exigeante que l'indexation, où le texte brut devient un index.

Prenez, par exemple, une page sur les chats, téléchargée à partir de Wikipedia. L'indexeur reçoit le texte complet de la page et doit supprimer les éléments inutiles (par exemple, les menus de contrôle de l'utilisateur, les publicités, les pieds de page, ...) et analyser la page pour obtenir le texte brut. Par exemple, le texte pourrait être :

> Le chat domestique (Felis silvestris f. catus) est une forme domestiquée du chat sauvage qui est le compagnon de l'homme depuis des milliers d'années. Comme son parent sauvage, il appartient à la sous-famille des petits chats, dont il est un représentant typique. Il possède un corps souple et musclé, parfaitement adapté à la chasse, des griffes et des dents acérées, une vue, une ouïe et un odorat excellents.

Extrait de [Wikipedia] (http://cs.wikipedia.org/wiki/Ko%C4%8Dka_dom%C3%A1c%C3%AD).

Le moteur de recherche analyse maintenant les mots individuels du texte et les inscrit dans des barils individuels. Cependant, il ne peut pas écrire le baril directement (principalement parce que chaque baril existe en plusieurs exemplaires et aussi parce que cela interférerait avec la recherche), il crée donc une liste d'exigences pour ce à quoi le futur baril devrait ressembler et les stocke dans une mémoire temporaire. Dans notre cas, une telle liste pourrait ressembler à ceci (certains mots sans importance peuvent être ignorés ou marqués comme mots vides) :

| ID du document | Mot | Position | Type / Type de document
|--------------|-------|--------|-----------|
| 1 | Chat | 1 | Normal
| 1 | Domestique| 2 | Normal |
| 1 | Felis | 3 | Normal |
| 1 | Est | 7 | Mot d'arrêt
| 1 | Domestique| 8 | Normal

Une telle liste est énorme (beaucoup plus grande que les textes originaux), mais la recherche est toujours plus rapide car nous ne devons pas lire tous les textes séquentiellement, mais pouvons effectuer une recherche par index dans chaque colonne et les mots d'une page sont triés en rangées les uns après les autres.

Une fois qu'un certain temps s'est écoulé (ou qu'un certain nombre de documents ont été complétés), l'indexeur arrête de travailler à la construction de cette liste de requêtes (pour un futur baril) et recommence à lire les données et à reconstruire les barils individuels (cette liste inclut d'anciens enregistrements qui sont dans un baril déjà fonctionnel). Si de nouveaux documents sont ajoutés à partir d'adresses connues, ce processus les mettra à jour, tandis que pour les nouveaux documents, il les inclura.

De cette façon, l'indexeur parcourt à nouveau la liste et crée lentement de nouveaux barils qui contiennent tous les éléments (ils auront la même apparence que dans l'exemple des chapitres précédents) et lorsque tous les barils seront terminés, ils seront envoyés aux différents serveurs de recherche. La mise à jour des barils sur les serveurs prend du temps (principalement en raison de l'énorme quantité de données déplacées), donc pendant cette opération les serveurs ne sont pas disponibles et les données sont mises à jour sur différents serveurs à différents moments. Il en résulte, par exemple, que certains utilisateurs peuvent obtenir des résultats différents parce que chaque utilisateur effectue sa recherche sur un serveur de distribution différent (en raison de la décomposition de la charge). Une fois la mise à jour terminée, tout revient à la normale et tous les utilisateurs retrouvent tous les documents de la même manière.

Le processus d'indexation est important pour tous les moteurs de recherche, et celui qui le fait le plus souvent et le plus soigneusement est celui qui a la vision la plus actualisée de l'Internet. Google effectue cette opération toutes les quelques heures, Seznam une fois par semaine (et il dispose d'un million de fois moins de données).

Canonisation des documents
--------------------

Lors de la conception initiale du moteur de recherche en texte intégral, la canonisation n'était pas nécessaire, car l'Internet était un média qui créait constamment de nouveaux contenus. Au fil du temps, cependant, la duplication (c'est-à-dire le même contenu apparaissant à plusieurs URL différentes) s'est produite, et les moteurs de recherche doivent s'y adapter. Un exemple typique est Wikipédia, qui compte de nombreux articles. Certains auteurs d'autres pages reprennent ces textes (en partie ou même complètement) et créent ainsi des doublons. Dans la plupart des cas, cependant, cela n'a pas d'importance, car la page source a un rang (qualité du lien) beaucoup plus élevé que la page plagiée, mais il peut arriver que cela dégrade l'original au détriment du pirate.

Les moteurs de recherche ont dû s'adapter à ce vice et le terme "canonisation" a été inventé, qui peut être compris comme la "suppression" de certaines pages de l'index. Par ailleurs, les index sont plus petits et le moteur de recherche n'a pas besoin d'explorer le même contenu en permanence.

Les doublons sont divisés en interne en 2 grandes catégories par chaque moteur de recherche :

Duplicatas naturels
-------------------

Ceux-ci sont créés par le comportement naturel de l'Internet et par ses caractéristiques.

Par exemple, l'URL `http://mathematicator.cz` aura probablement le même contenu que l'URL `http://www.mathematicator.cz` ou `http://mathematicator.cz/index.php` car c'est le comportement standard du serveur Apache (et d'Internet en général).

Si une duplication naturelle est trouvée, le moteur de recherche crée un "ensemble canonique", c'est-à-dire un groupe de pages parmi lesquelles le moteur de recherche sélectionne une représentante qui se distingue dans la recherche. Si un lien mène à une page de l'ensemble canonique, son rang sera automatiquement transmis au représentant principal.

C'est souvent une bonne idée d'aider le moteur de recherche à créer cet ensemble et de configurer correctement la redirection sur le site, ce qui amènera le moteur de recherche à mieux regarder le site et le représentant principal sera mieux sélectionné.

Les doublons conduisant au plagiat
----------------------------

Le plagiat est un problème sur l'internet aujourd'hui. L'existence du plagiat en soi n'a pas beaucoup d'importance, mais les utilisateurs sont particulièrement gênés par ce problème car ils trouvent toujours les mêmes résultats pour la même requête. Avez-vous aussi déjà trouvé plusieurs pages avec un texte identique pour une requête ? C'est exactement le comportement que les moteurs de recherche essaient d'empêcher.

Le plus gros problème est de déterminer quelle page est la source originale - et de le faire à la machine. Là encore, les moteurs de recherche regroupent toutes les pages similaires dans un ensemble canonique et sélectionnent un représentant principal dans cet ensemble. Si les sources proviennent de domaines différents, on ne peut pas considérer la situation comme une duplication naturelle (et choisir n'importe quel candidat), mais il faut évaluer toutes les pages de manière qualitative et sélectionner la meilleure de manière objective - et idéalement la source originale du contenu.

Les moteurs de recherche prennent souvent leurs décisions sur la base du rang de l'ensemble du domaine et de la force du réseau de liens vers un document donné, mais même cette approche n'est pas très fiable. Le deuxième facteur est aussi généralement le moment de la création (indexation) du document. Chaque moteur de recherche garde donc une trace des pages qui génèrent fréquemment du nouveau contenu et visite fréquemment ces pages, de sorte qu'idéalement il remarque immédiatement la nouvelle page et ne choisit donc pas une autre page comme proxy.

Une description détaillée des méthodes de fonctionnement de cette sélection dépasse le cadre de cet article et pourrait faire l'objet d'un livre entier.
