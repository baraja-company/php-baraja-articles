Git でコミットの所有者を変更する
==================

> id: '4dcba225-a543-4ca3-a771-87d73e36fbb6'
> slug:
> 	cs: git-zmena-vlastnictvi-commitu
> 	ja: git-dekomittono-suo-you-zhewo-bian-gengsuru
> 
> publicationDate: '2022-03-08 09:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: ea39b8fad4a7275887356e7abcbea307

組織間でリポジトリを移行する場合、コミットオーナーを上書きする必要が生じることがよくあります。その理由は、ユーザーの電子メールアドレスの変更などにより、あるアカウントから別のアカウントにコミットを転送することが考えられます。

例えば、Listの古いメールアカウントから、2つ目のGmailアカウントにすべてのコミットを転送する必要がありました。私がこのような変更を要求する可能性がある2つ目のケースは、私が誤ってプライベートな電子メールでコミットしてしまったが、特定の会社が自分たちのドメインでコミットすることを望んでいる場合です。

幸い、この問題を解決するコマンドがあり、それをプロジェクトマスターで呼び出すだけで、履歴をすべて上書きすることができるのです。

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

コマンドを実行した後は、`git push -f` コマンドを使用して変更をマスターに反映させる必要があります。

> **警告:**
>
> コマンド実行後、コミット履歴が全て上書きされ、ハッシュが変更されます。稀にしか発生しないはずのBCブレークです。コミットの上書きを間違えた場合、履歴を復元することはできません。同時に、すべてのブランチを削除または上書きする必要があります。そうしないと、変更されたすべてのコミットについて競合が発生し、解決時に二重書き込み（元のコミットと新しいコミット）になってしまいます。
