Changer la propriété d'un commit dans Git
=========================================

> id: '4dcba225-a543-4ca3-a771-87d73e36fbb6'
> slug:
> 	cs: git-zmena-vlastnictvi-commitu
> 	fr: changer-la-propriete-d-un-commit-dans-git
> 
> publicationDate: '2022-03-08 09:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: ea39b8fad4a7275887356e7abcbea307

Lors de la migration de référentiels entre organisations, il arrive souvent que nous devions écraser les propriétaires de commit. La raison peut en être le transfert de commits d'un compte à un autre, par exemple, en raison d'un changement d'adresse électronique de l'utilisateur.

Par exemple, j'avais besoin de transférer tous les commits de mon ancien compte de messagerie sur List vers mon deuxième compte Gmail. Le deuxième cas où je pourrais demander un tel changement est celui où je commets accidentellement sous une adresse électronique privée, mais où une entreprise particulière veut que les commits soient sous son domaine.

Heureusement, il existe une commande pour résoudre ce problème, que je peux simplement appeler dans le maître du projet pour écraser tout l'historique :

```txt
git filter-branch --env-filter "
if [ \"\$GIT_COMMITTER_EMAIL\" = \"janbarasek@seznam.cz\" ]
then
    export GIT_COMMITTER_NAME=\"Jan Barášek\"
    export GIT_COMMITTER_EMAIL=\"janbarasek@gmail.com\"
fi
if [ \"\$GIT_AUTHOR_EMAIL\" = \"janbarasek@seznam.cz\" ]
then
    export GIT_AUTHOR_NAME=\"Jan Barášek\"
    export GIT_AUTHOR_EMAIL=\"janbarasek@gmail.com\"
fi
" $@ --tag-name-filter cat -- --branches --tags
```

Après l'exécution de la commande, les changements doivent être transférés vers le master en utilisant la commande `git push -f`.

> **Attention:**
>
> Après l'exécution de la commande, l'ensemble de l'historique des livraisons est écrasé et les hachages sont modifiés. Il s'agit d'une rupture de la CB qui ne devrait se produire que rarement. Si vous faites une erreur en écrasant des commits, l'historique ne peut pas être restauré. En même temps, vous devez supprimer ou écraser toutes les branches, sinon il y aura un conflit sur tous les commits modifiés, qui sera doublement écrit (original et nouveau commit) lors de la résolution.
