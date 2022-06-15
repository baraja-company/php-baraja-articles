Configuración de la conexión de la Doctrina Baraja
==================================================

> id: fbd0961a-53fe-4713-8526-82e36bd1fb9b
> slug:
> 	cs: konfigurace-spojeni-s-baraja-doctrine
> 	es: configuracion-de-la-conexion-de-la-doctrina-baraja
> 
> publicationDate: '2020-09-10 11:38:44'
> mainCategoryId: '818d311a-0f58-4df7-a9a4-da7d21489dd6'
> sourceContentHash: cee6f56731ae88d1de9aac0da49e0874

Para establecer una conexión con la base de datos dentro de [Doctrina Baraja](https://github.com/baraja-core/doctrine) es necesario utilizar el archivo de configuración Neon, que es una parte común del marco de trabajo Nette.

La configuración puede ser así:

```neon
baraja.database:
    connection:
        host: localhost
        dbname: my-database
        user: root
        password: ******
```

Cuando se compila el Contenedor DI, se verifica la configuración y se lanza un mensaje de error que describe el error específico.

Las credenciales de acceso se verifican de forma segura cuando se compila el contenedor y luego se almacenan físicamente en él. Sólo el servicio que proporciona la conexión a la base de datos tiene entonces acceso a los inicios de sesión, y no pueden ser obtenidos simplemente por un servicio externo o por un visitante deshonesto de la barra de Tracy.

Compatibilidad con el pasado
----------

En el pasado, se utilizaban definiciones mediante parámetros, por ejemplo:

```neon
parameters:
    database:
        primary:
            host: localhost
            ...
```

Sin embargo, esta configuración está marcada como **deprecated** para aumentar la seguridad de la aplicación. Al utilizar los parámetros, cualquier servicio (o incluso parte de la aplicación) podría solicitar las credenciales de acceso, o la barra de Tracy activa en la página podría delatarlas.
