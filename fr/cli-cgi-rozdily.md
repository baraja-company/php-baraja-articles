Différences entre CLI et CGI
============================

> id: '79b00e74-b59a-4ae6-8a59-8eecfe2a01ca'
> slug:
> 	cs: cli-cgi-rozdily
> 	fr: differences-entre-cli-et-cgi
> 
> perex: Nejdůležitější rozdíly mezi CLI a CGI. Informace o prostředí.
> publicationDate: '2021-10-15 10:00:00'
> mainCategoryId: '4f1d7d70-c5b0-45f1-b1d2-d03c22aa4154'
> sourceContentHash: '0964358c913ca647cc947773de87ceda'

PHP peut fonctionner dans différents environnements. L'environnement le plus courant est `CGI`, qui s'exécute lorsque PHP traite une requête HTTP. Cependant, il est également possible d'exécuter un script PHP à partir du terminal. Dans ce cas, il s'agit d'une tâche dite CLI (Command-line interface).

Les différences les plus importantes entre CLI et CGI
-------------------------------------

- Contrairement à `CGI SAPI`, `CLI` n'écrit pas d'en-tête sur la sortie par défaut.
- Il y a certaines directives `php.ini` qui sont remplacées dans `CLI SAPI` parce qu'elles n'ont pas de sens dans un environnement shell :
   - `html_errors` : La valeur par défaut de CLI est `FALSE`.
   - `implicit_flush` : la valeur par défaut de l'interface CLI est `TRUE`.
   - `max_execution_time` : la valeur par défaut en CLI est `0` (illimité)
   - `register_argc_argv` : la valeur CLI par défaut est `TRUE`.
- Le script peut prendre des arguments en ligne de commande ! La variable `$argc` vous donne le nombre d'arguments passés à l'application. Et le champ `$argv` vous donne un tableau d'arguments réels
- Il y a 3 nouvelles constantes définies pour l'environnement shell : `STDIN`, `STDOUT`, `STDERR`. Tous sont des gestionnaires de fichiers pour le périphérique shell correspondant. Par exemple, `STDIN` est un gestionnaire de fichier pour `fopen('php://stdin', 'r')`. Vous pouvez donc lire une ligne de `STDIN` comme ceci : `$strLine = trim(fgets(STDIN));`. Le `STDIN` est déjà défini pour vous en utilisant le `PHP CLI`.
- Le PHP CLI ne change pas le répertoire courant en répertoire du script en cours d'exécution. Le répertoire courant du script est le répertoire dans lequel vous exécutez la commande PHP CLI.
- Il y a un certain nombre d'options UTILES disponibles pour le CLI de PHP. Ce qui vous permet d'obtenir des informations précieuses sur votre configuration php, votre script php ou de l'exécuter dans différents modes.
- Dans PHP 5, il y a eu quelques changements dans les noms de fichiers CLI et CGI. Dans PHP 5, la version CGI a été renommée en `php-cgi.exe` (anciennement `php.exe`) et la version CLI est maintenant située dans le répertoire principal (anciennement `cli/php.exe`).
- Un nouveau mode a également été introduit dans PHP 5 : `php-win.exe`. C'est équivalent à la version CLI, sauf que dans `php-win` rien n'est imprimé, et ne fournit donc pas de console (aucune "boîte à dos" n'est affichée à l'écran). Ce comportement est similaire à celui de `PHP GTK`.
