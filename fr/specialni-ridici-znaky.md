Caractères de contrôle spéciaux en PHP
======================================

> id: '888e6ee4-2ff3-4f0d-8f5a-4741e62e11d0'
> slug:
> 	cs: specialni-ridici-znaky
> 	fr: caracteres-de-controle-speciaux-en-php
> 
> publicationDate: '2021-11-24 14:05:00'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '2711968227a253040a0a6ed1464aa447'

Les chaînes de caractères PHP peuvent contenir des caractères de contrôle spéciaux qui ont une signification différente dans un contexte particulier et ne se comportent pas nécessairement comme des caractères ordinaires.

Nombre d'entre elles vous seront déjà familières de manière intuitive. Certains sont réservés à des utilisations spéciales et d'autres sont réservés aux caractères du clavier, par exemple.

Écriture de caractères spéciaux
-----------------------

Les caractères spéciaux sont écrits entre guillemets.

C'est donc très simple :

```php
$message = "Bonjour le monde.";
```

Le code précédent contient un saut de ligne entre `Hello` et `world`.

Tableau des caractères spéciaux
-------------------------

Si la chaîne de caractères est entourée de guillemets doubles ("), PHP interprétera les séquences d'échappement suivantes comme des caractères spéciaux :

| Séquence | Signification |
|----------|--------|
`\n` | saut de ligne (`LF` ou `0x0A (10)` en ASCII) | |
| `\r` | retour de chariot (`CR` ou `0x0D (13)` en ASCII) |
| ``t`` | tabulation horizontale (`HT` ou `0x09 (9)`` en ASCII) |
| ``v` | tabulation verticale (``VT` ou `0x0B (11)` en ASCII) | |
| `\e` | échappement (`ESC` ou `0x1B (27)` en ASCII) |
| | `\f` | saut de page (`FF` ou `0x0C (12)` en ASCII) |
| `\\\\` | backslash |
| `\$` | signe de dollar |
| `\"` | double-quote |
| [0-7]{1,3}` | La séquence de caractères correspondant à une expression régulière est un caractère en notation octale qui déborde silencieusement sur un octet. (par exemple, `"\400" === "\000"`) |
| `\x[0-9A-Fa-f]{1,2}` | La séquence de caractères correspondant à une expression régulière est un caractère en notation hexadécimale. | |
| `\u{[0-9A-Fa-f]+}` | la séquence de caractères correspondant à l'expression régulière est un point de code Unicode, qui sera affiché dans la chaîne sous la forme d'une représentation UTF-8 de ce point de code.

Comme pour les chaînes de caractères entre guillemets, une barre oblique inversée sera produite lors de l'échappement de tout autre caractère.

Lorsque vous délimitez des chaînes de caractères avec des guillemets, n'oubliez pas que les variables contenues seront développées (les valeurs des variables seront écrites directement dans la chaîne de caractères). Ce comportement peut être extrêmement dangereux.
