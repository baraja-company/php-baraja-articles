Cambiar la propiedad de los commits en Git
==========================================

> id: '4dcba225-a543-4ca3-a771-87d73e36fbb6'
> slug:
> 	cs: git-zmena-vlastnictvi-commitu
> 	es: cambiar-la-propiedad-de-los-commits-en-git
> 
> publicationDate: '2022-03-08 09:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: ea39b8fad4a7275887356e7abcbea307

Al migrar repositorios entre organizaciones, a menudo sucede que necesitamos sobrescribir los propietarios de las confirmaciones. El motivo puede ser la transferencia de commits de una cuenta a otra, por ejemplo, debido a un cambio en la dirección de correo electrónico del usuario.

Por ejemplo, necesitaba transferir todos los commits de mi antigua cuenta de correo electrónico en List a mi segunda cuenta de Gmail. El segundo caso en el que podría solicitar un cambio de este tipo es cuando accidentalmente hago un commit bajo un correo electrónico privado, pero una empresa en particular quiere commits bajo su dominio.

Afortunadamente, hay un comando para resolver este problema, que puedo llamar en el maestro del proyecto para sobrescribir todo el historial:

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

Una vez ejecutado el comando, los cambios deben ser enviados al maestro utilizando el comando `git push -f`.

> **Atención:**
>
> Tras la ejecución del comando, se sobrescribe todo el historial de confirmaciones y se modifican los hashes. Se trata de una ruptura de la CB que sólo debería producirse en raras ocasiones. Si se comete un error al sobrescribir confirmaciones, el historial no se puede restaurar. Al mismo tiempo, debe eliminar o sobrescribir todas las ramas, de lo contrario habrá un conflicto sobre todos los commits modificados, que se escribirán doblemente (commit original y nuevo) cuando se resuelvan.
