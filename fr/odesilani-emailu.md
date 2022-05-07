Fonction PHP mail()
===================

> id: '6536e2e2-38e1-4496-80ef-6c1c9a0b5be5'
> slug:
> 	cs: odesilani-emailu
> 	fr: fonction-php-mail
> 
> publicationDate: '2019-09-16 08:51:41'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: ca90a7366754a7e7a0ce43f4252296f8

La fonction `mail()` envoie un message électronique via la configuration du serveur par défaut. Pour fonctionner correctement, vous devez activer la fonction sur le serveur et configurer le serveur de messagerie pour l'envoi.

**La fonction est réservée à l'envoi. Vous devez organiser la réception des messages au niveau du serveur de messagerie. Par exemple, téléchargez régulièrement des messages en utilisant le protocole `IMAP` ou `POP3`.**

> Je déconseille fortement l'utilisation de cette fonction pour le moment, car le programmeur doit s'occuper de tout lui-même (par exemple, envoyer les bons en-têtes, ou définir l'encodage).
>
> Le mieux est de se connecter via <a href="/send-email-mail-smtp">serveur SMTP</a>.

Utilisation de
-------

```php
mail ('jan@barasek.com', 'Sujet', 'Texte de l'e-mail...');
```

Le premier paramètre est l'adresse du destinataire, le deuxième l'objet et le troisième le texte du message. Le quatrième paramètre (facultatif) indique la configuration supplémentaire du message.
