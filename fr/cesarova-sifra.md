Le chiffre de César - son fonctionnement
========================================

> id: '662dd627-c106-406e-ad6a-7ae9a492ad92'
> slug:
> 	cs: cesarova-sifra
> 	fr: le-chiffre-de-cesar---son-fonctionnement
> 
> publicationDate: '2019-08-23 15:43:02'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '892ad0746be469b5cdbb4de72d8e85b9'

**Attention : ** Cet article a été écrit il y a plusieurs années et certaines informations peuvent être dépassées ou incorrectes. Veuillez garder cela à l'esprit lors de votre lecture.

Le chiffrement de César est l'une des fonctions de hachage les plus simples. À l'époque, il était pratiquement incassable, mais à l'ère des ordinateurs modernes, il suffit de quelques dizaines de secondes, voire de quelques minutes, pour le briser. Elle est basée sur une clé, selon laquelle le message est crypté et selon laquelle il peut être à nouveau développé. La clé est donc secrète. Au moment du cryptage, le message peut être vu et ne signifiera rien (juste un fouillis de caractères). La seule façon de casser le chiffrement est de deviner la clé.

La clé
--------------------------

La clé peut être n'importe quel nombre entier ayant moins de chiffres que le message lui-même. En général, 3 chiffres valides sont donnés (il y a donc 999 combinaisons). Chaque chiffre supplémentaire augmente la sécurité. Pour que deux parties puissent communiquer, les deux parties doivent connaître cette clé secrète (elles doivent donc se la transmettre de manière sécurisée). Si la clé est connue d'eux et non de l'autre, le message peut être diffusé, même de manière non sécurisée, sans compromettre son contenu, car l'attaquant potentiel ne connaît pas la procédure pour récupérer le message.

Pour les besoins de la démonstration, j'utiliserai la clé **123**. Normalement, on utilise un autre nombre aléatoire qui est supposé ne pas être si facile à deviner.

Procédure de cryptage
--------------------------

Le principe repose sur l'idée de remplacer les caractères d'un message par d'autres à l'aide d'une clé. J'appelle ça "changer de personnage".

Par exemple, prenons ce message que nous voulons crypter :

```php
TAJNA ZPRAVA
```

Attribuez maintenant un numéro à chaque personnage. Généralement par l'alphabet (son numéro de série). Je vais utiliser l'alphabet anglais, donc cette série de caractères :

```php
A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
```

Si on attribue un numéro à chaque caractère, on obtient quelque chose comme :

```php
20-1-10-14-1   26-16-18-1-22-1
```

Maintenant vient la clé. On prend chaque numéro individuel et on y ajoute la clé. Je surligne en couleur ce que j'ai ajouté à tel ou tel endroit :

`1 2 3`

```php
21 3 13 15 3   3 17 20 4 23 3
```

Notez que lorsque je crypte le caractère **Z**, j'écris le chiffre 3. C'est parce que **Z** est le dernier caractère de l'alphabet, donc lorsqu'il atteint la fin, il compte à nouveau depuis le début de la ligne.

Transmettre le message
--------------------------

Maintenant, nous pouvons transmettre le message comme nous le souhaitons. Parfois même publiquement. Les autres ne verront qu'une série illogique de chiffres, et ne sauront probablement pas qu'il s'agit d'une sorte de chiffrement. La sécurité est également renforcée par la clé elle-même, qui est secrète. Parfois, il convient de transmettre le message sous la forme d'une série de chiffres, parfois il convient de convertir ces chiffres en caractères (toujours en suivant la même série alphabétique), puis de transmettre une séquence de caractères. Cela dépend beaucoup des circonstances. En général, cependant, une séquence numérique est préférable, car peu de gens se douteront qu'il s'agit d'un message codé.

Décryptage chez le destinataire
--------------------------

Le destinataire décrypte selon la même procédure. Il prend chaque caractère individuel et soustrait les chiffres en fonction de la clé, puis reconvertit les valeurs résultantes en caractères en utilisant l'alphabet. C'est juste une procédure de cryptage inverse. L'important est de connaître la **clé** et le **jeu de caractères**, c'est-à-dire l'ordre des caractères.

Craquage et décryptage
--------------------------

La seule procédure possible de craquage consiste à essayer toutes les combinaisons possibles de toutes les clés potentielles. Si nous ne connaissons pas la longueur de la clé, tout le processus devient encore plus compliqué. Mais en général, les ordinateurs d'aujourd'hui peuvent essayer quelque chose comme 100 clés en 1 seconde, donc une clé aléatoire de 3 caractères prend environ une minute à craquer.

Cependant, si la clé est aussi longue ou plus longue que le message original, elle ne peut pas être cassée car chaque caractère individuel a sa propre clé, de sorte que toutes les combinaisons doivent être essayées pour chaque caractère.

Si j'avais un message "**TAKE MESSAGE**", il comporterait 11 caractères (les espaces ne comptent pas). Si je voulais une clé qui soit aussi longue de 11 caractères, et que j'utilise une chaîne de 26 caractères de l'alphabet anglais, alors il y a **11^26** = 1.191817654*10²⁷ combinaisons, l'ordinateur moyen craquerait cette clé en 1.310999419×10²⁶ secondes = 10^20 jours :)
