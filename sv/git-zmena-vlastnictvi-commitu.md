Ändra äganderätten till en commit i Git
=======================================

> id: '4dcba225-a543-4ca3-a771-87d73e36fbb6'
> slug:
> 	cs: git-zmena-vlastnictvi-commitu
> 	sv: aendra-aeganderaetten-till-en-commit-i-git
> 
> publicationDate: '2022-03-08 09:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: ea39b8fad4a7275887356e7abcbea307

När du migrerar arkiv mellan organisationer händer det ofta att vi behöver skriva över commit-ägare. Anledningen till detta kan vara att överföra ändringar från ett konto till ett annat, t.ex. på grund av en ändring av användarens e-postadress.

Jag behövde till exempel överföra alla ändringar från mitt gamla e-postkonto på List till mitt andra Gmail-konto. Det andra fallet där jag skulle kunna begära en sådan ändring är när jag av misstag gör en överföring under en privat e-postadress, men ett visst företag vill ha överföringar under sin domän.

Som tur är finns det ett kommando som löser det här problemet och som jag kan anropa i projektmastern för att skriva över hela historiken:

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

Efter att kommandot har utförts måste ändringarna skickas till master med kommandot `git push -f`.

> **Varning:**
>
> Efter att kommandot har utförts skrivs hela historiken över och hashen ändras. Detta är ett BC-avbrott som endast bör förekomma i sällsynta fall. Om du gör ett misstag när du skriver över ändringar kan historiken inte återställas. Samtidigt måste du radera eller skriva över alla grenar, annars uppstår en konflikt om alla ändrade commits, som skrivs dubbelt (original och ny commit) när de löses.
