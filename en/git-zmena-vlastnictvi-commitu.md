Changing commit ownership in Git
================================

> id: '4dcba225-a543-4ca3-a771-87d73e36fbb6'
> slug:
> 	cs: git-zmena-vlastnictvi-commitu
> 	en: changing-commit-ownership-in-git
> 
> publicationDate: '2022-03-08 09:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: ea39b8fad4a7275887356e7abcbea307

When migrating repositories between organizations, it often happens that we need to overwrite commit owners. The reason for this can be to transfer commits from one account to another, for example, because of a change in the user's email address.

For example, I needed to transfer all commits from my old email account on List to my second Gmail account. The second case where I might request such a change is when I accidentally commit under a private email, but a particular company wants commits under their domain.

Fortunately, there is a command to solve this problem, which I can just call in the project master to overwrite the entire history:

```
git filter-branch --env-filter "
if [ \"\$GIT_COMMITTER_EMAIL\" = \"janbarasek@seznam.cz\" ]
then
    export GIT_COMMITTER_NAME="Jan Barášek\"
    export GIT_COMMITTER_EMAIL="janbarasek@gmail.com\"
fi
if [ \"\$GIT_AUTHOR_EMAIL\" = \"janbarasek@seznam.cz\" ]
then
    export GIT_AUTHOR_NAME="Jan Barášek\"
    export GIT_AUTHOR_EMAIL="janbarasek@gmail.com\"
fi
" $@ --tag-name-filter cat -- --branches --tags
```

After the command is executed, the changes need to be flushed to the master with `git push -f`.

> **Warning:**
>
> After the command is executed, the entire commit history is overwritten and the hashes are changed. This is a BC break that should only occur rarely. If you make a mistake when overwriting commits, the history cannot be restored. At the same time, you must delete or overwrite all branches, otherwise there will be a conflict over all changed commits, which will be double-written (original and new commit) when resolved.
