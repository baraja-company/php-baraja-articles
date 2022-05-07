PHP-funktion mail()
===================

> id: '6536e2e2-38e1-4496-80ef-6c1c9a0b5be5'
> slug:
> 	cs: odesilani-emailu
> 	sv: php-funktion-mail
> 
> publicationDate: '2019-09-16 08:51:41'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: ca90a7366754a7e7a0ce43f4252296f8

Funktionen `mail()` skickar ett e-postmeddelande via standardserverkonfigurationen. För att det ska fungera korrekt måste du aktivera funktionen på servern och konfigurera e-postservern för sändning.

**Funktionen är endast avsedd för sändning. Du måste ordna mottagandet av meddelanden på e-postservernivå. Du kan t.ex. ladda ner meddelanden regelbundet med hjälp av `IMAP` eller `POP3`-protokollet.**

> Jag avråder starkt från att använda funktionen för tillfället, eftersom programmeraren måste ta hand om allt själv (t.ex. skicka korrekta rubriker eller ställa in kodningen).
>
> Det är mycket bättre att ansluta via <a href="/send-email-mail-smtp">SMTP-server</a>.

Användning av
-------

```php
mail ('jan@barasek.com', 'Ämne', 'E-posttext...');
```

Den första parametern är mottagarens adress, den andra är ämnet och den tredje meddelandetexten. Den fjärde (valfria) parametern anger den ytterligare konfigurationen av meddelandet.
