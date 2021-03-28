Konfigurace spojení s Baraja Doctrine
================================

> id: fbd0961a-53fe-4713-8526-82e36bd1fb9b
> slugCS: konfigurace-spojeni-s-baraja-doctrine
> publicationDate: "2020-09-10 11:38:44"
> mainCategoryId: "818d311a-0f58-4df7-a9a4-da7d21489dd6"

Pro navázání spojení s databází v rámci [Baraja Doctrine](https://github.com/baraja-core/doctrine) je potřeba použít konfigurační Neon soubor, který je běžnou součástí Nette framework.

Konfigurace pak může vypadat například takto:

```neon
baraja.database:
    connection:
        host: localhost
        dbname: my-database
        user: root
        password: ******
```

Při kompilaci DI Containeru se správnost konfigurace ověří a případně vyhodí chybová hláška s popisem konkrétní chyby.

Přihlašovací údaje se při kompilaci kontejneru bezpečným způsobem ověří a poté fyzicky uloží do kontejneru. Přístup k přihlášení poté má pouze služba zajišťující spojení s databází a nemůže je jednoduše získat cizí služba nebo nečestný návštěník z Tracy baru.

Zpětná kompatibilita
----------

V minulosti se používala definice pomocí parametrů, tedy například:

```neon
parameters:
    database:
        primary:
            host: localhost
            ...
```

Toto nastavení je ale označeno jako **deprecated** z důvodu zvýšení bezpečnosti aplikace. Při použití parametrů totiž mohla libovolná služba (nebo i část aplikace) vyžádat přihlašovací údaje, případně je mohl prozradit aktivní Tracy bar na stránce.
