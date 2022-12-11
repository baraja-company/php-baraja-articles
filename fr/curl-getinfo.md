Obtenir des informations sur les requêtes HTTP via cURL
=======================================================

> id: '310e5143-6b38-44c1-a1bf-de37f94ee17d'
> slug:
> 	- null
> 	fr: obtenir-des-informations-sur-les-requetes-http-via-curl
> 
> cs: curl-getinfo
> publicationDate: '2022-07-06 19:50:00'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '49cb69f5cf2b844cb780f77c1e7d42d5'

La fonction PHP `curl_getinfo()` fournit des informations détaillées sur la requête cURL exécutée. Cet article explique la signification de chaque champ.

Exemple d'utilisation
---------------

Appelez la fonction sur le résultat du contexte de `curl_init()` :

```php
$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, 'https://baraja.cz');
curl_setopt($ch, CURLOPT_HEADER, 0);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 0);
curl_setopt($ch, CURLOPT_NOBODY, 1);
curl_setopt($ch, CURLOPT_FOLLOWLOCATION, 0);
curl_exec($ch);
$info = curl_getinfo($ch);
curl_close($ch);

dump($info);
```

Tableau des valeurs
--------------

La fonction `curl_getinfo()` renvoie un tableau associatif à partir duquel les clés et les valeurs individuelles peuvent être récupérées.

| Clé | Valeur de l'exemple | Explication |
|---------------------------|----------------------------|------------------------------------------------------------------------------------|
| `url` | 'https://baraja.cz/' | URL téléchargée. |
| `content_type` | 'text/html ; charset=utf-8' | Encodage et type de contenu utilisés (revendiqués par le serveur cible). |
| `http_code` | 200 | Code d'état HTTP renvoyé. 200 signifie OK. |
| `header_size` | 462 | Taille de l'en-tête de la requête HTTP en octets. |
| `request_size` | 47 | Taille de la demande. |
| | `filetime` | -1 | Heure du fichier (réclamations du serveur). |
| `ssl_verify_result` | 0 | Vérification SSL. |
| | `redirect_count` | 0 | Nombre de redirections avant d'atteindre le document cible.
| `total_time` | 0.233384 | Temps total passé à attendre une réponse. En secondes.
| `namelookup_time` | 0.021608 | Temps pris pour résoudre le domaine sur les enregistrements DNS. Spécifié en secondes. |
| | `connect_time` | 0.035031 | Temps pour établir une connexion au serveur de destination. Spécifié en secondes. |
| | `pretransfer_time` | 0.187275 | Temps nécessaire pour transférer les données. Spécifié en secondes. |
| `upload_size` | 0.0 | Taille des données téléchargées en octets. |
| `size_download` | 0.0 | Taille des données téléchargées en octets. |
| `speed_download` | 0.0 | Vitesse de téléchargement en octets par seconde. |
| `speed_upload` | 0.0 | Vitesse de téléchargement en octets par seconde. |
| `download_content_length` | 15522.0 | Taille des données téléchargées en octets. |
| `upload_content_length` | -1.0 | Taille des données téléchargées en octets. |
| `starttransfer_time` | 0.233354 | Indique la valeur TTFB (Time To First Byte) en secondes. |
| `redirect_time` | 0.0 | Temps passé à rediriger vers le téléchargement du contenu canonique.
| | `redirect_url` | `''` | URL canonique et destination de la redirection. |
| `primary_ip` | '76.76.21.21' | De quelle IP le contenu a été téléchargé. |
| `certinfo` | array (0) | Plus de détails sur le certificat du site cible. |
| `primary_port` | 443 | Le port réseau utilisé (80 signifie HTTP, 443 signifie HTTPS). |
| `local_ip` | '192.168.0.186' | Adresse IP locale de la machine qui a envoyé la requête. |
| `local_port` | 56568 | Port de la machine locale depuis laquelle la demande a été envoyée. |
| `http_version` | 3 | Version du protocole HTTP. |
| | `protocole` | 2 | Code du protocole utilisé. | |
| `ssl_verifyresult` | 0 | Résultat de la vérification SSL. |
| `scheme` | 'HTTPS' | Protocole au début de l'URL. |
| `appconnect_time_us` | 186220 | Temps nécessaire pour établir la connexion avec le serveur cible. Spécifié en microsecondes. |
| | `connect_time_us` | 35031 | Temps de connexion au serveur de destination. Spécifié en microsecondes. | | |
| | `namelookup_time_us` | 21608 | Temps nécessaire pour réécrire le domaine via les enregistrements DNS. Spécifié en microsecondes. |
| | `pretransfer_time_us` | 187275 | Temps pris pour transférer les données. Spécifié en microsecondes. |
| `redirect_time_us` | 0 | Temps passé à rediriger vers le téléchargement du contenu canonique. Exprimé en microsecondes.
| `starttransfer_time_us` | 233354 | Indique la valeur du temps TTFB (Time To First Byte). En microsecondes. |
| | `total_time_us` | 233384 | Temps total passé à attendre une réponse. Spécifié en microsecondes. |

Certaines clés ne sont pas toujours disponibles. Vérifiez toujours l'existence de la clé et la validité de la valeur avant de lire cette dernière.
