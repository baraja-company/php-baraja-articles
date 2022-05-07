Konfigurera anslutningen till Baraja-doktrinen
==============================================

> id: fbd0961a-53fe-4713-8526-82e36bd1fb9b
> slug:
> 	cs: konfigurace-spojeni-s-baraja-doctrine
> 	sv: konfigurera-anslutningen-till-baraja-doktrinen
> 
> publicationDate: '2020-09-10 11:38:44'
> mainCategoryId: '818d311a-0f58-4df7-a9a4-da7d21489dd6'
> sourceContentHash: cee6f56731ae88d1de9aac0da49e0874

För att upprätta en anslutning till databasen i [Baraja Doctrine] (https://github.com/baraja-core/doctrine) måste du använda Neon-konfigurationsfilen, som är en vanlig del av Nette-ramverket.

Konfigurationen kan se ut så här:

```neon
baraja.database:
    connection:
        host: localhost
        dbname: my-database
        user: root
        password: ******
```

När DI-behållaren kompileras kontrolleras konfigurationen och ett felmeddelande skickas ut som beskriver det specifika felet.

Inloggningsuppgifterna verifieras på ett säkert sätt när behållaren kompileras och lagras sedan fysiskt i behållaren. Endast den tjänst som tillhandahåller anslutningen till databasen har tillgång till inloggningarna, och de kan inte enkelt hämtas av en extern tjänst eller en obehörig besökare från Tracy-baren.

Bakåtkompatibilitet
----------

Tidigare användes definitioner med hjälp av parametrar, till exempel:

```neon
parameters:
    database:
        primary:
            host: localhost
            ...
```

Den här inställningen är dock markerad som **föråldrad** för att öka säkerheten i programmet. När parametrarna används kan en tjänst (eller till och med en del av programmet) begära inloggningsuppgifter, eller så kan den aktiva Tracy-indikatorn på sidan avslöja dem.
