Communication via SSH and RSA2 key
==================================

> id: '09ec87dd-810d-4618-b76b-fd30f63be30a'
> slug:
> 	cs: ssh-klic
> 	en: communication-via-ssh-and-rsa2-key
> 
> publicationDate: '2022-11-12 15:00:00'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '4427ec2be03799f94a860d04be6a72ff'

SSH is a network protocol for encrypted file and Terminal transfer. SSH is most commonly used for remote control of a web server and secure file transfer. Unlike FTP, it offers a native encrypted connection. SSH communicates over the default port `22`. The connection is initialized with the username and address of the destination server. A password (not recommended) or an SSH RSA2 key (recommended) can be used for authentication.

Obtaining (generating) the key
--------------------------

Before we connect to the server, we need to obtain (or generate) our first SSH RSA2 key. It is important that it is an `RSA2` algorithm. This is because there are a number of keys, and not all of them can be used.

In Linux, the `ssh-keygen` utility is used to generate it, to which we specify the complexity of the key (4096 in this case) and the email of the authorized user:

```shell
ssh-keygen -t rsa -b 4096 -C "jan@barasek.com"
```

After running the command, we will be asked for the path to store the key and any `password` (authorization password). Do not enter anything as the path (the default location is automatically chosen) and enter the passphrase optionally (if you do, you will need to enter the same password each time before using the key).

The generated key is automatically saved to the default location `~/.ssh`, i.e. to the `.ssh` directory in the current user's home directory.

Generating an SSH key in Windows
-------------------------------

Unfortunately, Windows does not have a default path for the SSH key. It is therefore ideal to install, for example, the `Putty` utility and `PuttyGen` to generate the key. Always choose the `RSA2` algorithm. Again, a pair of keys will be generated, so store them safely somewhere. Before using the SSH key in `Putty`, you must select the disk path from which to retrieve the key. In Linux this is not needed, there is a default disk path.

Key security
---------------

When keys are generated, two are actually generated. One public key (the one you give to the other party to allow communication) and a private key (the one is yours alone, never tell anyone, and is used to decrypt communication).

It is essential that you never lose the private key. Losing it means breaking the whole communication!

It is generally recommended to generate a unique public/private key pair for each device and user to reduce the likelihood of a leak. However, if you want to transfer the key between devices, you can. The SSH key is at the password level, so when you move it to another machine right away the connection works.

Some servers remember which device they last communicated with via SSH. Therefore, it is possible that the connection will not work after moving the key to the new computer. In this case, you need to clear the key cache on the server.

Authorizing the key to connect to the server
--------------------------------------

The `ssh` command is used to connect to the server. Simply enter the user and domain:

```shell
ssh root@baraja.cz
```

Then it will try to establish an SSH connection. If you have a working and correctly configured SSH key, the connection will be made automatically. If not, you must enter a password.

If you want to authenticate against your server using an SSH key instead of a password, you need to transfer your **public key** to the server.

Simply display it with the command:

```shell
cat ~/.ssh/id_rsa.pub
```

And copy its entire contents to the target server at location `~/.ssh/authorized_keys`. If you have more than one key, each on a separate line.
