Cambiare la proprietà dei commit in Git
=======================================

> id: '4dcba225-a543-4ca3-a771-87d73e36fbb6'
> slug:
> 	cs: git-zmena-vlastnictvi-commitu
> 	it: cambiare-la-proprieta-dei-commit-in-git
> 
> publicationDate: '2022-03-08 09:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: ea39b8fad4a7275887356e7abcbea307

Quando si migrano i repository tra organizzazioni, capita spesso di dover sovrascrivere i proprietari dei commit. La ragione di questo può essere il trasferimento di commit da un account ad un altro, per esempio, a causa di un cambiamento nell'indirizzo email dell'utente.

Per esempio, avevo bisogno di trasferire tutti i commit dal mio vecchio account di posta elettronica su List al mio secondo account Gmail. Il secondo caso in cui potrei richiedere un tale cambiamento è quando accidentalmente faccio un commit sotto un'email privata, ma una particolare azienda vuole i commit sotto il suo dominio.

Fortunatamente, c'è un comando per risolvere questo problema, che posso semplicemente chiamare nel master del progetto per sovrascrivere l'intera storia:

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

Dopo che il comando è stato eseguito, le modifiche devono essere scaricate sul master usando il comando `git push -f`.

> **Attenzione:**
>
> Dopo l'esecuzione del comando, l'intera cronologia dei commit viene sovrascritta e gli hash vengono cambiati. Questa è una rottura del BC che dovrebbe verificarsi solo in rari casi. Se fate un errore quando sovrascrivete i commit, la storia non può essere ripristinata. Allo stesso tempo, dovete cancellare o sovrascrivere tutti i rami, altrimenti ci sarà un conflitto su tutti i commit cambiati, che saranno scritti due volte (commit originale e nuovo) quando verranno risolti.
