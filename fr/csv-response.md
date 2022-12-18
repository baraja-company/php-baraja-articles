Envoi d'un fichier CSV
======================

> id: '35c4f30c-8638-496b-82a7-1297a598fbdb'
> slug:
> 	cs: csv-response
> 	fr: envoi-d-un-fichier-csv
> 
> publicationDate: '2022-12-18 11:10:00'
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1
> sourceContentHash: '582e41a6e41b568d09be08c482523df8'

Lorsque vous envoyez des fichiers binaires, pensez toujours aux en-têtes HTTP à choisir. Dans le cas de l'envoi d'un fichier CSV (format presque idéal pour les tableaux de texte simples, qui peuvent être traités par Excel), `Content-Type : application/csv`, dans le codage `UTF-8` est utile.

Cependant, dans certaines versions d'Excel, il y a un problème avec l'encodage UTF-8. Pour s'assurer que le bon encodage est détecté, nous devons insérer le `UTF-8 BOM`, qui est un caractère spécial ``xEF\xBB\xBF`` qui indique au client qu'il s'agit d'UTF-8, puisqu'il n'existe dans aucun autre encodage.

Par conséquent, envoyez les en-têtes comme suit :

```php
header('Content-Type : application/csv ; charset=utf-8');
header('Content-Disposition : attachment ; filename=.' . date('d-m-y') . '_file.csv');
header('Pragma : no-cache');
echo "\xEF\xBB\xBF";
```
