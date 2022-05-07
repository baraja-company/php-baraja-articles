Zmiana własności commitów w Gicie
=================================

> id: '4dcba225-a543-4ca3-a771-87d73e36fbb6'
> slug:
> 	cs: git-zmena-vlastnictvi-commitu
> 	pl: zmiana-wlasnosci-commitow-w-gicie
> 
> publicationDate: '2022-03-08 09:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: ea39b8fad4a7275887356e7abcbea307

Podczas migracji repozytoriów pomiędzy organizacjami często zdarza się, że musimy nadpisać właścicieli commitów. Powodem tego może być przeniesienie commitów z jednego konta na drugie, na przykład z powodu zmiany adresu e-mail użytkownika.

Na przykład musiałem przenieść wszystkie commity z mojego starego konta pocztowego na Liście na moje drugie konto Gmail. Drugim przypadkiem, w którym mógłbym poprosić o taką zmianę, jest sytuacja, w której przypadkowo dokonuję commitów na prywatny adres e-mail, a dana firma chce, aby commitować pod ich domeną.

Na szczęście istnieje polecenie rozwiązujące ten problem, które mogę wywołać w głównym projekcie, aby nadpisać całą historię:

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

Po wykonaniu polecenia, zmiany należy przepłukać na master za pomocą polecenia `git push -f`.

> **Ostrzeżenie:**
>
> Po wykonaniu tego polecenia cała historia commitów zostaje nadpisana, a hashe zmienione. Jest to przerwa w BC, która powinna występować rzadko. Jeśli popełnisz błąd podczas nadpisywania commitów, nie będzie można przywrócić historii. Jednocześnie należy usunąć lub nadpisać wszystkie gałęzie, w przeciwnym razie powstanie konflikt wszystkich zmienionych commitów, który po rozwiązaniu zostanie podwójnie zapisany (oryginalny i nowy commit).
