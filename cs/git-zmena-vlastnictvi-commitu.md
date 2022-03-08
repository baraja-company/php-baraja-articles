Změna vlastnictví commitů v Gitu
================================

> id: "4dcba225-a543-4ca3-a771-87d73e36fbb6"
> slug:
> 	cs: git-zmena-vlastnictvi-commitu
> 
> publicationDate: "2022-03-08 09:00:00"
> mainCategoryId: "483db7b7-5699-41fb-ba0b-d2b653bacd1f"

Při migraci repositářů mezi organizacemi se často stává, že potřebujeme přepsat vlastníky commitů. Důvodem může být převod commitů z jednoho účtu na druhý například z důvodu změny e-mailové adresy uživatele.

Například jsem potřeboval převést všechny commity ze starého e-mailu na Seznamu na můj druhý účet na Gmailu. Druhý případ, kdy mohu takovou změnu požadovat, je okamžik, kdy omylem commitnu pod soukromým e-mailem, ale konkrétní firma chce commity pod jejich doménou.

Na řešení tohoto problému naštěstí existuje příkaz, který stačí zavolat v masteru projektu a přepsat tím celou historii:

```
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

Po provedení příkazu je potřeba změny flushnout do masteru příkazem `git push -f`.

> **Pozor:**
>
> Po provedení příkazu dojde k přepsání celé historie commitů a změně hashů. Jde o BC break, který by měl nastat jen výjimečně. Pokud uděláte při přepsání commitů chybu, nelze historii obnovit. Zároveň je potřeba smazat nebo přepsat všechny větve, jinak dojde ke konfliktu nad všemi změněnými commity, které se po vyřešení zapíšou zdvojeně (původní a nový commit).
