Analyse syntaxique et traitement des données en PHP
===================================================

> id: '7dfc991d-a7ee-4e2b-8159-16cef1d27483'
> slug:
> 	cs: parsovani-a-zpracovani-dat
> 	fr: analyse-syntaxique-et-traitement-des-donnees-en-php
> 
> publicationDate: '2017-10-15 09:54:02'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '33947488a0005f2977e6283c532a08d7'

> Cet article traite des méthodes de traitement des données en PHP, mais il n'est pas encore terminé.

Donc, temporairement, juste une rapide esquisse des possibilités :

- **Crawl caractère par caractère** est une méthode très ancienne qui rend le code désordonné, mais toutes les autres méthodes le font en interne.
- <a href="/explode">Explode</a>, division d'une chaîne par un délimiteur
- <a href="/regex">**Les expressions régulières**</a> sont la meilleure façon de traiter les chaînes de caractères simples.
- **Tokenizer**, divise une chaîne complexe en morceaux (tokens) selon des expressions régulières, par exemple, voici comment PHP est traité
