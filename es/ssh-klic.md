Comunicación mediante SSH y clave RSA2
======================================

> id: '09ec87dd-810d-4618-b76b-fd30f63be30a'
> slug:
> 	cs: ssh-klic
> 	es: comunicacion-mediante-ssh-y-clave-rsa2
> 
> publicationDate: '2022-11-12 15:00:00'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '4427ec2be03799f94a860d04be6a72ff'

SSH es un protocolo de red para la transferencia cifrada de archivos y terminales. SSH se utiliza sobre todo para el control remoto de un servidor web y la transferencia segura de archivos. A diferencia del FTP, ofrece una conexión cifrada nativa. SSH se comunica a través del puerto por defecto `22`. La conexión se inicializa con el nombre de usuario y la dirección del servidor de destino. Para la autenticación se puede utilizar una contraseña (no recomendada) o una clave SSH RSA2 (recomendada).

Obtención (generación) de la clave
--------------------------

Antes de conectarnos al servidor, necesitamos obtener (o generar) nuestra primera clave SSH RSA2. Es importante que sea un algoritmo `RSA2`. Esto se debe a que hay varias teclas y no todas pueden utilizarse.

En Linux, para generarla se utiliza la utilidad `ssh-keygen`, a la que especificamos la complejidad de la clave (4096 en este caso) y el correo electrónico del usuario autorizado:

```shell
ssh-keygen -t rsa -b 4096 -C "jan@barasek.com"
```

Después de ejecutar el comando, se nos pedirá la ruta para almacenar la clave y cualquier `password` (contraseña de autorización). No introduzca nada como ruta (la ubicación predeterminada se elige automáticamente) e introduzca la frase de contraseña opcionalmente (si lo hace, tendrá que introducir la misma contraseña cada vez antes de utilizar la clave).

La clave generada se guarda automáticamente en la ubicación predeterminada `~/.ssh`, es decir, en el directorio `.ssh` del directorio personal del usuario actual.

Generar una clave SSH en Windows
-------------------------------

Desafortunadamente, Windows no tiene una ruta predeterminada para la clave SSH. Por ello, lo ideal es instalar, por ejemplo, la utilidad `Putty` y `PuttyGen` para generar la clave. Elija siempre el algoritmo `RSA2`. De nuevo, se generará un par de claves, así que guárdalas en un lugar seguro. Antes de utilizar la clave SSH en `Putty`, debe seleccionar la ruta de disco desde la que recuperar la clave. En Linux esto no es necesario, hay una ruta de disco por defecto.

Seguridad de las llaves
---------------

Cuando se generan claves, en realidad se generan dos. Una clave pública (la que das a la otra parte para permitir la comunicación) y una clave privada (la que es sólo tuya, nunca se la digas a nadie, y se utiliza para desencriptar la comunicación).

Es esencial que nunca pierdas la clave privada. Perderlo significa romper toda la comunicación.

Generalmente se recomienda generar un par de claves pública/privada único para cada dispositivo y usuario para reducir la probabilidad de fugas. Sin embargo, si quieres transferir la clave entre dispositivos, puedes hacerlo. La clave SSH está en el nivel de contraseña, por lo que cuando se mueve a otra máquina de inmediato la conexión funciona.

Algunos servidores recuerdan con qué dispositivo se comunicaron por última vez a través de SSH. Por lo tanto, es posible que la conexión no funcione después de trasladar la llave al nuevo ordenador. En este caso, debe borrar la caché de claves del servidor.

Autorizar la clave para conectarse al servidor
--------------------------------------

El comando `ssh` se utiliza para conectarse al servidor. Basta con introducir el usuario y el dominio:

```shell
ssh root@baraja.cz
```

A continuación, intentará establecer una conexión SSH. Si dispone de una clave SSH operativa y correctamente configurada, la conexión se realizará automáticamente. En caso contrario, deberá introducir una contraseña.

Si quieres autenticarte contra tu servidor usando una clave SSH en lugar de una contraseña, necesitas transferir tu **clave pública** al servidor.

Basta con mostrarlo con el comando:

```shell
cat ~/.ssh/id_rsa.pub
```

Y copia todo su contenido al servidor de destino en la ubicación `~/.ssh/authorized_keys`. Si tienes más de una llave, cada una en una línea distinta.
