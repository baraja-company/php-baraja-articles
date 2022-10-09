Konfigurering af forbindelsen til Baraja-doktrinen
==================================================

> id: fbd0961a-53fe-4713-8526-82e36bd1fb9b
> slug:
> 	cs: konfigurace-spojeni-s-baraja-doctrine
> 	da: konfigurering-af-forbindelsen-til-baraja-doktrinen
> 
> publicationDate: '2020-09-10 11:38:44'
> mainCategoryId: '818d311a-0f58-4df7-a9a4-da7d21489dd6'
> sourceContentHash: cee6f56731ae88d1de9aac0da49e0874

For at oprette en forbindelse til databasen i [Baraja Doctrine](https://github.com/baraja-core/doctrine) skal du bruge Neon-konfigurationsfilen, som er en fælles del af Nette-rammen.

Konfigurationen kan se således ud:

```neon
baraja.database:
    connection:
        host: localhost
        dbname: my-database
        user: root
        password: ******
```

Når DI-containeren kompileres, kontrolleres konfigurationen, og der sendes en fejlmeddelelse med en beskrivelse af den specifikke fejl.

Loginoplysningerne verificeres sikkert, når containeren kompileres, og opbevares derefter fysisk i containeren. Det er kun den tjeneste, der leverer forbindelsen til databasen, der har adgang til logins, og de kan ikke blot hentes af en ekstern tjeneste eller en uautoriseret besøgende fra Tracy-baren.

Bagudkompatibilitet
----------

Tidligere blev der anvendt definitioner ved hjælp af parametre, f.eks:

```neon
parameters:
    database:
        primary:
            host: localhost
            ...
```

Denne indstilling er dog markeret som **deprecated** for at øge programsikkerheden. Ved brug af parametrene kan enhver tjeneste (eller endog en del af programmet) anmode om loginoplysninger, eller den aktive Tracy-linje på siden kan afsløre dem.
