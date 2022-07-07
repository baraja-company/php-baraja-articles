在Git中改变提交所有权
============

> id: '4dcba225-a543-4ca3-a771-87d73e36fbb6'
> slug:
> 	cs: git-zmena-vlastnictvi-commitu
> 	zh: zaigit-zhong-gai-bian-ti-jiao-suo-you-quan
> 
> publicationDate: '2022-03-08 09:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: ea39b8fad4a7275887356e7abcbea307

在不同组织之间迁移存储库时，经常会发生我们需要覆盖提交所有者的情况。其原因可能是将提交从一个账户转移到另一个账户，例如，因为用户的电子邮件地址发生了变化。

例如，我需要将我在List上的旧电子邮件账户的所有提交内容转移到我的第二个Gmail账户。第二种情况是，我可能会要求进行这样的修改，即我不小心在一个私人邮箱下提交，但某家公司希望在他们的域名下提交。

幸运的是，有一个命令可以解决这个问题，我可以直接在项目主程序中调用这个命令来覆盖整个历史。

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

命令执行后，需要用`git push -f`命令将修改的内容冲到主站。

> **警告：**
>
> 命令执行后，整个提交历史被覆盖，哈希值被改变。这是BC省的一个突破，只应很少发生。如果你在覆盖提交时犯了错误，就不能恢复历史。同时，你必须删除或覆盖所有分支，否则所有更改的提交都会产生冲突，解决时将会被重复写入（原始提交和新提交）。
