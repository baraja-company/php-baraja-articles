Formulaires HTML - partie dans le navigateur
============================================

> id: cb0015c7-b7b6-41ac-8263-4068960e16b3
> slug:
> 	cs: formulare
> 	fr: formulaires-html---partie-dans-le-navigateur
> 
> perex: 'Zpracování formulářů v PHP, zejména možnosti odeslání získaných dat na server metodou GET a POST.'
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: a23332c0-a233-4093-abd7-85b1b00a383b
> sourceContentHash: '8d2a0df0667b3da5d6b19e6c25ad2517'

Avant de pouvoir traiter les données utilisateur sur le serveur via PHP, nous devons d'abord les obtenir. Cela se fait dans le navigateur via des formulaires HTML qui définissent les éléments de base pour recevoir les données. Le but de cet article n'est pas de présenter toutes les possibilités des formulaires, mais seulement les possibilités de base pour accepter les données et comprendre le principe.

Source du formulaire HTML de base
-----------------------------

```html
<form action="script.php" method="get">

<!-- Zde bude celý obsah formuláře -->

</form>
```

Chaque formulaire commence par la balise HTML `<form>` et se termine par la balise `</form>`. Tous les champs de formulaire placés entre ces balises seront soumis.

Ensuite, vous devez définir où envoyer le formulaire avec l'attribut `action` (nom du script), et quelle méthode utiliser avec l'attribut `method` (GET ou POST). Si la méthode et la destination ne sont pas spécifiées, le formulaire s'envoie par défaut par la méthode GET.

Champs de formulaire de base
-------------------------

Le champ le plus utilisé est utilisé pour obtenir le texte (chaîne de caractères). Chaque champ a son propre type et son propre nom par lequel il peut être reconnu après la soumission.

Champs de texte communs
------------------

Plus important encore, j'ai besoin d'un champ de texte brut :

```html
<input type="text" name="food">
```

<input type="text" name="food">

Champ du mot de passe
---------------------

```html
<input type="password" name="heslo">
```

<input type="password" name="password">

Case à cocher
--------

Il est utilisé pour vérifier le booléen (`TRUE` et `FALSE`) :

```html
<input type="checkbox" name="vop" checked="checked">
```

<label>
	<input type="checkbox" name="vop" checked="checked"> Acceptez-vous les conditions ?
</label>

Bouton radio pour sélectionner plusieurs options
------------------------------------

```html
<input type="radio" name="language" value="cz" checked> Čeština
<input type="radio" name="language" value="sk"> Slovenština
<input type="radio" name="language" value="en"> Angličtina
```

Il vous permet de choisir parmi plusieurs options. L'option sélectionnée envoie sa valeur. Par défaut, il est bon de sélectionner un champ avec l'attribut `checked="checked"` :

<label>
	<input type="radio" name="language" value="cz" checked="checked"> Tchèque
</label><br>
<label>
	<input type="radio" name="language" value="en"> Slovaque
</label><br>
<label>
	<input type="radio" name="language" value="en"> Anglais
</label>

Grand champ de texte
------------------

Créé pour la saisie de texte sur plusieurs lignes. Il est également utilisé pour entrer :

- `cols` ~ nombre de colonnes
- `rows` ~ nombre de lignes

```html
<textarea name="article" cols="40" rows="6">
Ahoj lidi!
</textarea>
```

<textarea name="article" cols="40" rows="6">
Hé les gars !
</textarea>

Boîte de sélection
---------

Présente un moyen pratique de sélectionner parmi de nombreuses données.

```html
<select name="gender">
	<option value="man">Muž</option>
	<option value="woman">Žena</option>
</select>
```

Après avoir soumis le formulaire, la valeur dans `value` est envoyée.

<select name="gender">
	<option value="man">Mâle</option>
	<option value="woman">Femme</option>
</select>

Bouton d'envoi
---------------------

Le formulaire peut comporter un nombre illimité de boutons d'envoi. Ils sont faciles à saisir :

```html
<input type="submit" value="Odeslat">
```

Lorsqu'il est cliqué, il prend toutes les données des champs du formulaire et les envoie au script de paramétrage :

<input type="submit" value="Submit">

Traitement des données sur le serveur
-------------------------

Ensuite, il faut envoyer les données au serveur et les traiter sur place, ce qui est abordé dans <a href="/processing-formula-in-php">l'article suivant</a>.
