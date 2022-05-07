PHP funkcia mail()
==================

> id: '6536e2e2-38e1-4496-80ef-6c1c9a0b5be5'
> slug:
> 	cs: odesilani-emailu
> 	sk: php-funkcia-mail
> 
> publicationDate: '2019-09-16 08:51:41'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: ca90a7366754a7e7a0ce43f4252296f8

Funkcia `mail()` odošle e-mailovú správu prostredníctvom predvolenej konfigurácie servera. Pre správne fungovanie je potrebné túto funkciu na serveri povoliť a nastaviť poštový server na odosielanie.

**Funkcia je určená len na odosielanie. Prijímanie správ musíte vyriešiť na úrovni poštového servera. Pravidelne sťahujte správy napríklad pomocou protokolu `IMAP` alebo `POP3`.**

> V súčasnosti dôrazne neodporúčam používať túto funkciu, pretože programátor sa musí o všetko postarať sám (napríklad poslať správne hlavičky alebo nastaviť kódovanie).
>
> Oveľa lepšie je pripojiť sa cez <a href="/send-email-mail-smtp">SMTP server</a>.

Používanie stránky
-------

```php
mail ("jan@barasek.com, "Predmet, "Text e-mailu...);
```

Prvým parametrom je adresa príjemcu, druhým predmet a tretím text správy. Štvrtý (nepovinný) parameter označuje dodatočnú konfiguráciu správy.
