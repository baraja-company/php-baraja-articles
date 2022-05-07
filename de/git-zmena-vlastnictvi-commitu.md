Ändern der Commit-Eigentümerschaft in Git
=========================================

> id: '4dcba225-a543-4ca3-a771-87d73e36fbb6'
> slug:
> 	cs: git-zmena-vlastnictvi-commitu
> 	de: aendern-der-commit-eigentuemerschaft-in-git
> 
> publicationDate: '2022-03-08 09:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: ea39b8fad4a7275887356e7abcbea307

Bei der Migration von Repositories zwischen Organisationen kommt es häufig vor, dass wir Commit-Eigentümer überschreiben müssen. Der Grund dafür kann die Übertragung von Commits von einem Konto auf ein anderes sein, z. B. aufgrund einer Änderung der E-Mail-Adresse des Benutzers.

Ich musste zum Beispiel alle Übertragungen von meinem alten E-Mail-Konto bei List auf mein zweites Gmail-Konto übertragen. Der zweite Fall, in dem ich eine solche Änderung beantragen könnte, ist, wenn ich versehentlich eine Übergabe unter einer privaten E-Mail-Adresse durchführe, ein bestimmtes Unternehmen aber eine Übergabe unter seiner Domäne wünscht.

Glücklicherweise gibt es einen Befehl zur Lösung dieses Problems, den ich einfach im Projektmaster aufrufen kann, um den gesamten Verlauf zu überschreiben:

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

Nachdem der Befehl ausgeführt wurde, müssen die Änderungen mit dem Befehl `git push -f` in den Master übertragen werden.

> **Warnung:**
>
> Nach der Ausführung des Befehls wird der gesamte Übergabeverlauf überschrieben und die Hashes werden geändert. Dies ist eine BC-Pause, die nur selten vorkommen sollte. Wenn Sie beim Überschreiben von Übertragungen einen Fehler machen, kann der Verlauf nicht wiederhergestellt werden. Gleichzeitig müssen Sie alle Zweige löschen oder überschreiben, da sonst ein Konflikt über alle geänderten Commits entsteht, der doppelt geschrieben wird (ursprünglicher und neuer Commit), wenn er gelöst wird.
