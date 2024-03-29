通过SSH和RSA2密钥进行通信
================

> id: '09ec87dd-810d-4618-b76b-fd30f63be30a'
> slug:
> 	cs: ssh-klic
> 	zh: tong-guossh-hersa2mi-yao-jin-xing-tong-xin
> 
> publicationDate: '2022-11-12 15:00:00'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '4427ec2be03799f94a860d04be6a72ff'

SSH是一个用于加密文件和终端传输的网络协议。SSH最常用于网络服务器的远程控制和安全文件传输。与FTP不同，它提供了一个本地加密连接。SSH通过默认端口`22`进行通信。连接是用目的服务器的用户名和地址初始化的。可以使用密码（不推荐）或SSH RSA2密钥（推荐）进行认证。

获取（生成）钥匙
--------------------------

在我们连接到服务器之前，我们需要获得（或生成）我们的第一个SSH RSA2密钥。重要的是，它是一种`RSA2`算法。这是因为有许多键，而不是所有的键都能被使用。

在Linux中，`ssh-keygen`工具被用来生成它，我们指定密钥的复杂性（在本例中为4096）和授权用户的电子邮件。

```shell
ssh-keygen -t rsa -b 4096 -C "jan@barasek.com"
```

运行命令后，我们将被要求提供存储密钥的路径和任何`password`（授权密码）。不要输入任何东西作为路径（会自动选择默认位置），可选择输入口令（如果你这样做，你需要在使用钥匙之前每次都输入这个相同的密码）。

生成的密钥会自动保存到默认位置`~/.ssh`，也就是当前用户主目录下的`.ssh`目录。

在Windows中生成一个SSH密钥
-------------------------------

不幸的是，Windows没有一个默认的SSH密钥的路径。因此，理想的做法是安装，例如，`Putty`工具和`PuttyGen`来生成密钥。始终选择`RSA2`算法。同样，将产生一对钥匙，所以要把它们安全地存放在某个地方。在`Putty`中使用SSH密钥之前，你必须选择磁盘路径，以便从那里获取密钥。在Linux中这是不需要的，有一个默认的磁盘路径。

关键安全
---------------

生成钥匙时，实际上生成了两个。一个公钥（你给对方的公钥，允许通信）和一个私钥（是你一个人的，永远不要告诉别人，用来解密通信）。

至关重要的是，你永远不要丢失私钥。失去它就意味着打破了整个沟通!

一般建议为每个设备和用户生成一个独特的公钥/私钥对，以减少泄漏的可能性。然而，如果你想在设备之间转移钥匙，你可以。SSH密钥是在密码级别的，所以当你把它移到另一台机器上时，连接马上就能正常工作。

有些服务器会记住它们最后通过SSH与哪个设备进行通信。因此，有可能在把钥匙移到新电脑上后，连接就不工作了。在这种情况下，你需要清除服务器上的密钥缓存。

授权密钥连接到服务器
--------------------------------------

`ssh`命令用于连接到服务器。只需输入用户和域名。

```shell
ssh root@baraja.cz
```

然后它将尝试建立一个SSH连接。如果你有一个有效的和正确配置的SSH密钥，连接将自动建立。如果没有，你必须输入一个密码。

如果你想用SSH密钥而不是密码对你的服务器进行认证，你需要把你的**公钥转移到服务器上。

只需用命令显示它。

```shell
cat ~/.ssh/id_rsa.pub
```

并将其全部内容复制到目标服务器的`~/.ssh/authorized_keys`位置。如果你有一个以上的钥匙，每个都在一个单独的行上。
