PHP funkce mail()
=================

> id: "6536e2e2-38e1-4496-80ef-6c1c9a0b5be5"
> slugCS: odesilani-emailu
> publicationDate: "2019-09-16 08:51:41"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Funkce `mail()` odešle e-mailovou zprávu prostřednictvím výchozí konfigurace serveru. Pro korektní fungování je potřeba funkci na serveru povolit a nastavit mailový server pro odesílání.

**Funkce slouží pouze pro odesílání. Přijímání zpráv si musíte vyřešit na úrovni mail serveru. Například zprávy pravidelně stahovat protokolem `IMAP` nebo `POP3`.**

> Použití funkce v současné době výrazně nedoporučuji, protože si programátor musí všechno ohlídat sám (například odeslat správné hlavičky, nebo nastavit kódování).
>
> Mnohem lepší je připojení přes <a href="/odesilani-emailu-mail-smtp">SMTP server</a>.

Použití
-------

```php
mail ('jan@barasek.com', 'Předmět', 'Text emailu... ');
```

Prvním parametrem uvedeme adresu příjemce, druhým předmět a třetím text zprávy. Čtvrtý (nepovinný) parametr uvádí doplňující konfiguraci zprávy.
