Codes d'état HTTP
=================

> id: '97800bb6-004c-4bdb-b126-f87e77cc5120'
> slug:
> 	cs: stavove-http-kody
> 	fr: codes-d-etat-http
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '6c1da4a7233c1eba531e9336daae5acb'

Dans la communication HTTP, des "codes d'état" sont transmis, qui sont des informations sur la façon dont le transfert a été effectué. Je suis sûr que vous savez qu'un code `200` signifie un succès et qu'un code `404` signifie une page inexistante.

Les codes de statut sont divisés en plusieurs groupes en fonction de leur préfixe.

1xx Informatif
--------------

| Code | Signification |
|-------|--------|
| `100` | Continuer |
| `101` | Protocole de commutation |

2xx Succès
----------

| Code | Signification |
|-------|--------|
| `200` | OK (tout va bien) |
| `201` | Créé |
| `202` | Accepté |
| `203` | Information non autorisée |
| `204` | Pas de contenu |
| `205` | Restaurer le contenu |
| `206` | Contenu partiel |

Redirection 3xx
----------------

| Code | Signification |
|-------|--------|
| 300` | Choix multiple |
| `301` | Redirection permanente |
| `302` | Trouvé |
| `303` | Voir plus |
| `304` | Aucun changement |
| `305` | Utiliser un proxy |
| `306` | **Vieux mais réservé pour une utilisation future** |
| `307` | Redirection temporaire |

4xx Erreur du client (utilisateur)
-----------------------------

| Code | Signification |
|-------|--------|
| `400` | Mauvaise demande |
| `401` | Connexion non autorisée |
| `402` | Paiement demandé |
| `403` | Désactivé |
| `404` | Non trouvé |
| `405` | Méthode non autorisée |
| `406` | Inacceptable | ...
| `407` | Authentification du proxy requise |
| `408` | La demande a expiré |
| `409` | Conflit de réseau |
| `410` | Données disparues |
| `411` | La longueur demandée ne correspond pas |
| `412` | Échec de l'hypothèse |
| `413` | La demande d'entité est trop importante |
| `414` | Request-URI is too long |
| `415` | Unsupported media type |
| `416` | La portée demandée n'est pas satisfaisante |
| `417` | Échec de l'attente |

5xx Erreur de serveur
--------------

| Code | Signification |
|-------|--------|
| `500` | Erreur de serveur interne |
| `501` | Non implémenté |
| `502` | Mauvaise passerelle |
| `503` | Service indisponible |
| `504` | Délai d'attente de la passerelle expiré |
| `505` | Version HTTP non supportée |
| `509` | Limite de bande passante dépassée |
