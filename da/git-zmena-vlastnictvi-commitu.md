Ændring af ejerskab af commit i Git
===================================

> id: '4dcba225-a543-4ca3-a771-87d73e36fbb6'
> slug:
> 	cs: git-zmena-vlastnictvi-commitu
> 	da: aendring-af-ejerskab-af-commit-i-git
> 
> publicationDate: '2022-03-08 09:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: ea39b8fad4a7275887356e7abcbea307

Når vi migrerer repositorier mellem organisationer, sker det ofte, at vi har brug for at overskrive commit-ejere. Årsagen til dette kan være overførsel af kommits fra en konto til en anden, f.eks. på grund af en ændring i brugerens e-mailadresse.

Jeg havde f.eks. brug for at overføre alle kommits fra min gamle mailkonto på List til min anden Gmail-konto. Det andet tilfælde, hvor jeg kan anmode om en sådan ændring, er, når jeg ved et uheld laver en commit under en privat e-mail, men en bestemt virksomhed ønsker at lave commits under deres domæne.

Heldigvis findes der en kommando til at løse dette problem, som jeg bare kan kalde i projektmasteren for at overskrive hele historikken:

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

Efter at kommandoen er udført, skal ændringerne flushes til master med kommandoen `git push -f`.

> **Varsling:**
>
> Når kommandoen er udført, overskrives hele commit-historikken, og hashes ændres. Dette er et BC-brud, som kun bør forekomme sjældent. Hvis du begår en fejl, når du overskriver commits, kan historikken ikke gendannes. Samtidig skal du slette eller overskrive alle grene, ellers vil der være en konflikt om alle ændrede commits, som vil blive dobbeltskrevet (det oprindelige og det nye commit), når de bliver løst.
