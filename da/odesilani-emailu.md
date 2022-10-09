PHP-funktion mail()
===================

> id: '6536e2e2-38e1-4496-80ef-6c1c9a0b5be5'
> slug:
> 	cs: odesilani-emailu
> 	da: php-funktion-mail
> 
> publicationDate: '2019-09-16 08:51:41'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: ca90a7366754a7e7a0ce43f4252296f8

Funktionen `mail()` sender en e-mail-meddelelse via standardserverkonfigurationen. For at det skal fungere korrekt, skal du aktivere funktionen på serveren og konfigurere mailserveren til at sende.

**Funktionen er kun til afsendelse. Du er nødt til at sortere modtagelse af meddelelser på mailserverniveau. Du kan f.eks. downloade meddelelser regelmæssigt ved hjælp af `IMAP` eller `POP3`-protokollen.**

> Jeg fraråder stærkt brugen af funktionen i øjeblikket, fordi programmøren selv skal tage sig af alting (f.eks. sende de korrekte headers eller indstille kodningen).
>
> Det er meget bedre at oprette forbindelse via <a href="/send-email-mail-smtp">SMTP-server</a>.

Brug af
-------

```php
mail ('jan@barasek.com', 'Emneord', 'E-mail tekst...');
```

Den første parameter er modtagerens adresse, den anden er emnet og den tredje er meddelelsesteksten. Den fjerde (valgfrie) parameter angiver den yderligere konfiguration af meddelelsen.
