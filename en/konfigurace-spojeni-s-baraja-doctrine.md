Configuring the Baraja Doctrine connection
==========================================

> id: fbd0961a-53fe-4713-8526-82e36bd1fb9b
> slug:
> 	cs: konfigurace-spojeni-s-baraja-doctrine
> 	en: configuring-the-baraja-doctrine-connection
> 
> publicationDate: '2020-09-10 11:38:44'
> mainCategoryId: '818d311a-0f58-4df7-a9a4-da7d21489dd6'
> sourceContentHash: cee6f56731ae88d1de9aac0da49e0874

To establish a connection to the database within [Baraja Doctrine](https://github.com/baraja-core/doctrine) you need to use the Neon configuration file, which is a common part of the Nette framework.

The configuration can look like this:

```neon
baraja.database:
    connection:
        host: localhost
        dbname: my-database
        user: root
        password: ******
```

When the DI Container is compiled, the configuration is checked for correctness and an error message is thrown describing the specific error.

The login credentials are securely verified when compiling the container and then physically stored in the container. Only the service providing the connection to the database then has access to the logins, and they cannot simply be obtained by an external service or a rogue visitor from the Tracy bar.

Backward compatibility
----------

In the past, definitions using parameters were used, for example:

``neon
parameters:
    database:
        primary:
            host: localhost
            ...
```

However, this setting is marked as **deprecated** to increase application security. This is because when using the parameters, any service (or even part of the application) could request login credentials, or the active Tracy bar on the page could give them away.
