Comment fonctionne Captcha (image descriptive)
==============================================

> id: '9cf191e4-a49b-407b-b16e-21de7224ac43'
> slug:
> 	cs: captcha
> 	fr: comment-fonctionne-captcha-image-descriptive
> 
> perex: 'Captcha je druh obrázku, který ověří, že uživatel není robot a chrání webovou aplikaci. Možnosti implementace v PHP.'
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: '80357b2e7f0047bb488922f2ab7b4336'

Le Captcha est actuellement l'un des moyens les plus courants de protéger les formats libres. Il a été créé à l'origine non pas pour protéger la sécurité des données, mais pour se protéger contre le spam et pour reconnaître qu'il s'agit d'un humain.

Cependant, comme il est généré par une machine, il n'est pas toujours parfait, mais le taux de réussite d'un test captcha avoisine les 99 % et seulement 1 % des images peuvent être déchiffrées par un robot spammeur bien fait.

Comment cela fonctionne
--------------------------

Il existe d'autres options, par exemple j'utilise cette solution :

- Le serveur reçoit l'information que l'utilisateur a demandé une page de formulaire et commence à la générer.
- Dans le futur champ de test captcha, il met une image avec une adresse secrète qui ne dit rien (comme un nombre aléatoire, une chaîne de caractères, ... peu importe).
- L'image est générée et stockée à cette adresse. En même temps, la transcription correcte est écrite quelque part (peut-être dans une session ou une base de données).
- Lorsque l'utilisateur soumet le formulaire, il est vérifié si la transcription correspond à ce qu'il a saisi. Si c'est le cas, le formulaire est traité. Sinon, une re-description d'une autre image est demandée.
- Après des tentatives réussies ou non, l'image captcha temporaire est supprimée (elle ne sera plus jamais utilisée). Si le formulaire n'est pas soumis dans un certain délai, il sera également supprimé (expire).

Transcription correcte
--------------------------

Si le test captcha peut être résolu, l'utilisateur est probablement humain. Cependant, il est bon de penser aux utilisateurs qui ne peuvent pas résoudre la tâche, notamment les aveugles. C'est une bonne solution pour combiner plusieurs tests possibles (notamment le voice prefetching). Cependant, la reconnaissance vocale par une machine est actuellement beaucoup plus efficace que la lecture d'une image. Cette solution n'est donc pas toujours idéale.

Image captcha simple personnalisée
--------------------------

Très souvent, il suffit de générer une image vierge de certaines dimensions et d'y saisir quelques caractères de manière lisible sans autre forme de procès. Sérieusement ! La plupart des robots spammeurs sont stupides et ne peuvent pas attaquer les formulaires génériques avec ce type de protection, même s'il s'agit d'un texte parfaitement lisible qu'un meilleur OCR peut transcrire parfaitement.

L'image résultante peut ressembler à ceci :

```txt
<img src="captcha.php" alt="ukázková captcha">
```

Essayez de rafraîchir la page plusieurs fois et vous verrez que le code change aléatoirement à chaque fois. Pour les besoins de la démonstration, il est simplement généré sans être sauvegardé, il est donc supprimé immédiatement après son chargement.

J'ai résolu le code source en utilisant la bibliothèque PHPGD, qui est disponible sur pratiquement toutes les installations et hébergements PHP :

```php
Header("Type de contenu : image/png");
$obr = ImageCreate(100, 35);
$pozadi = ImageColorAllocate ($obr, 219, 28, 49); //définition de la couleur d'arrière-plan
$bila = ImageColorAllocate ($obr, 255, 255, 255); //définition de la couleur blanche pour le texte
$styl = array ($pozadi);
ImageSetStyle ($obr, $styl);
  $nahodne_cislo = rand(11111,99999); //tirer un numéro aléatoire de 5 caractères
  imagestring($obr, 5, 25, 10, $nahodne_cislo, $bila); //fonction pour dessiner du texte (dans ce cas un nombre)
ImagePNG($obr); //générer l'image en mémoire et effectuer le rendu
ImageDestroy($obr); //supprime l'image de la mémoire (elle ne sera plus nécessaire, car elle est générée une fois)
```

Le rendu de l'image n'est alors qu'une question de HTML :

```html
<img src="captcha.php">
```

Notez que ce script n'est pas fonctionnel en soi sans modification supplémentaire. Il sert uniquement de démonstration pour générer une image simple.

Résumé
--------------------------

Les tests Captcha sont assez fiables, mais ils sont ennuyeux et prennent du temps. Parfois, elles ne sont pas lisibles. Il est donc intéressant de donner à l'utilisateur la possibilité de charger une autre image à l'aide de javascript, afin d'éviter de recharger toute la page et de devoir tout remplir à nouveau.

Rappelez-vous : Ce qui est une tâche triviale pour un humain peut ne jamais être une solution réalisable pour une machine. C'est pourquoi même ce captcha primitif au texte parfaitement lisible peut protéger contre plus de la moitié des spams.
