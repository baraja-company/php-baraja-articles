配置Baraja Doctrine连接
===================

> id: fbd0961a-53fe-4713-8526-82e36bd1fb9b
> slug:
> 	cs: konfigurace-spojeni-s-baraja-doctrine
> 	zh: pei-zhibaraja-doctrine-lian-jie
> 
> publicationDate: '2020-09-10 11:38:44'
> mainCategoryId: '818d311a-0f58-4df7-a9a4-da7d21489dd6'
> sourceContentHash: cee6f56731ae88d1de9aac0da49e0874

为了在[Baraja Doctrine](https://github.com/baraja-core/doctrine)内建立与数据库的连接，你需要使用Neon配置文件，它是Nette框架的一个共同部分。

配置可以看起来像这样。

```neon
baraja.database:
    connection:
        host: localhost
        dbname: my-database
        user: root
        password: ******
```

当DI容器被编译时，配置被验证，并抛出一个错误信息，描述具体的错误。

当容器被编译时，登录凭证被安全地验证，然后被物理地存储在容器中。只有提供与数据库连接的服务才能访问这些登录，它们不能简单地被外部服务或流氓访问者从Tracy栏中获得。

向后兼容
----------

在过去，使用参数的定义，例如。

```neon
parameters:
    database:
        primary:
            host: localhost
            ...
```

然而，这个设置被标记为**废弃的**，以提高应用程序的安全性。在使用参数时，任何服务（甚至是应用程序的一部分）都可以要求登录凭证，或者页面上活跃的特雷西栏可以泄露它们。
