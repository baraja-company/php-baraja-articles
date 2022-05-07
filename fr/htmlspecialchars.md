Htmlspecialchars
================

> id: '46f04729-3956-4889-bb40-58362cb46b2a'
> slug:
> 	cs: htmlspecialchars
> 	fr: htmlspecialchars
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0743dd02-12d0-4766-ba42-8fd7e9c4ae8a'
> sourceContentHash: '566e10c57527a45f32906beb6c21430f'

Htmlspecialchars() est une fonction permettant de convertir les caractères spéciaux en entités HTML.

Description
-----

```php
$promenna = htmlspecialchars($text);
```

Certains caractères spéciaux ont une signification particulière pour les navigateurs, ils doivent donc être convertis en entités. Cela permet d'assurer la sécurité générale du script et d'éviter que la page ne soit mal rendue.

Il est le plus souvent utilisé pour protéger les formulaires et tout endroit où l'utilisateur insère du texte et risque d'insérer des balises HTML.

| Caractère | Note | Modifications apportées à
|------|-------------------------|-----------
| `&` | esperluette | `&amp;`
| "`"` | guillemet double (change lorsque `ENT_NOQUOTES` est désactivé) | `&quot;``
| `'` | apostrophe (change lorsque `ENT_QUOTES` est activé) | `&#039;``.
| `<` | moins que, parenthèse HTML | `&lt;``
| plus grand que, parenthèse HTML | `&gt;``

Paramètre
--------

**String** à convertir

**flags** Paramètres de comportement différents

**charset** Spécifie le jeu de caractères (encodage). Le jeu de caractères par défaut est `ISO-8859-1`.

Vous pouvez utiliser `ISO-8859-1`, `ISO-8859-15`, `UTF-8`, `cp866`, `CP1251`, `CP1252`, et `KOI8-R`.

> Note : Prise en charge uniquement à partir de PHP 4.3.0 et plus. Tout autre jeu de caractères n'est pas reconnu ni pris en charge.

**double_encode** Lorsque `double_encode` est désactivé, PHP n'encodera pas les entités HTML existantes, la valeur par défaut est de tout convertir.

Valeurs de retour
-----------------

Convertir une chaîne de caractères.

Si la chaîne contient des unités invalides, dans le jeu de caractères donné dans `ENT_IGNORE` (non défini), une chaîne vide est retournée.

Changements dans les versions
----------------

| Version | Note
|-------|---------
| 5.4.0 | Ajout des constantes `ENT_SUBSTITUTE`, `ENT_DISALLOWED`, `ENT_HTML401`, `ENT_XML1`, `ENT_XHTML` et `ENT_HTML5`.
| 5.3.0 | Ajout de la constante `ENT_IGNORE`.
| 5.2.3 | Ajout du paramètre `double_encode`.
| 4.1.0 | Ajout du paramètre `charset`.

Exemple
-------

```php
$new = htmlspecialchars(
	'<a href="test">Test</a>',
	ENT_QUOTES
);

echo $new; // <a href="test">Test</a>
```
