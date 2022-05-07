Réception de données par la méthode POST
========================================

> id: c2f273fa-7730-4521-82d0-1b3096269fac
> slug:
> 	cs: metoda-post
> 	fr: reception-de-donnees-par-la-methode-post
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '51a30cb759cdbb184ee5bb0bf3070af1'

L'envoi de données par POST est très différent de <a href="/method-get">GET</a>, il est plus sûr, le texte peut être plus long et sa valeur ne peut pas être déterminée sauf par un formulaire ou un en-tête (que vous n'obtiendrez pas par erreur).

Source :
--------------------------

La source n'est pas si différente de la méthode <a href="/method-get">GET</a>. C'est à peu près la même chose, sauf que les paramètres ne sont pas affichés dans l'URL, seul le nom du fichier est visible.

```php
echo $_POST['Article'] ?? '';
```

Caractéristiques, avantages et inconvénients
--------------------------

- Les paramètres ne peuvent pas être liés, mais un formulaire doit être soumis
- Ne peut pas être indexé (lié au point précédent)
- Plus sûr que <a href="/method-get">GET</a> (les données sont envoyées de manière cachée, les valeurs ne sont pas affichées dans l'historique).
- À ne pas confondre avec SSL, POST n'est pas crypté.
