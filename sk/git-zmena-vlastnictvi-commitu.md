Zmena vlastníctva revízie v systéme Git
=======================================

> id: '4dcba225-a543-4ca3-a771-87d73e36fbb6'
> slug:
> 	cs: git-zmena-vlastnictvi-commitu
> 	sk: zmena-vlastnictva-revizie-v-systeme-git
> 
> publicationDate: '2022-03-08 09:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: ea39b8fad4a7275887356e7abcbea307

Pri migrácii repozitárov medzi organizáciami sa často stáva, že potrebujeme prepísať vlastníkov revízií. Dôvodom môže byť prenos revízií z jedného účtu na druhý, napríklad z dôvodu zmeny e-mailovej adresy používateľa.

Potreboval som napríklad preniesť všetky revízie z môjho starého e-mailového konta na Zozname do môjho druhého konta Gmail. Druhým prípadom, kedy môžem požiadať o takúto zmenu, je, keď omylom odovzdám pod súkromným e-mailom, ale konkrétna spoločnosť chce odovzdať pod svojou doménou.

Našťastie existuje príkaz na vyriešenie tohto problému, ktorý môžem jednoducho zavolať v hlavnom projekte a prepísať celú históriu:

```
git filter-branch --env-filter "
if [ \"\$GIT_COMMITTER_EMAIL\" = \"janbarasek@seznam.cz\" ]
potom
    export GIT_COMMITTER_NAME="Jan Barášek\"
    export GIT_COMMITTER_EMAIL="janbarasek@gmail.com\"
fi
if [ \"\$GIT_AUTHOR_EMAIL\" = \"janbarasek@seznam.cz\" ]
potom
    export GIT_AUTHOR_NAME="Jan Barášek\"
    export GIT_AUTHOR_EMAIL="janbarasek@gmail.com\"
fi
" $@ --tag-name-filter cat -- --branches --tags
```

Po vykonaní príkazu je potrebné zmeny spláchnuť do masteru pomocou príkazu `git push -f`.

> **Upozornenie:**
>
> Po vykonaní príkazu sa prepíše celá história revízií a zmenia sa hashe. Ide o prestávku v systéme BC, ktorá by sa mala vyskytovať len zriedkavo. Ak pri prepisovaní revízií urobíte chybu, históriu nie je možné obnoviť. Zároveň musíte odstrániť alebo prepísať všetky vetvy, inak dôjde ku konfliktu všetkých zmenených revízií, ktoré budú pri riešení zapísané dvakrát (pôvodná a nová revízia).
