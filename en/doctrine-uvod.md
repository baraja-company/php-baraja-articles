Doctrine Series - Introduction
==============================

> id: fbd0461a-53fe-4713-8926-82e31bd1fb9b
> slug:
> 	cs: doctrine-uvod
> 	en: doctrine-series---introduction
> 
> publicationDate: '2021-08-27 11:40:00'
> mainCategoryId: '818d311a-0f58-4df7-a9a4-da7d21489dd6'
> sourceContentHash: '2bc62308fcb66acda5a09ac95b5eb2c2'

Doctrine is an advanced PHP library for object-oriented database work. The main purpose and goal of Doctrine is to describe the database schema using data entities and manipulate the data in a fully object-oriented way.

This paradigm is called ORM (Object-relational mapping), which is [design-pattern](/design-patterns) for converting (wrapping) data stored in a relational database into an object that can be used in an object-oriented language. Thus, to understand and use Doctrine, you must know at least the basics of [Object-Oriented Programming](/oop).

Why learn Doctrine?
------------------------

There are many reasons:

- Doctrine is the most widely used ORM database, used by most of the advanced PHP community.
- It will fundamentally simplify the design of your PHP application
- You provide a consistent way to design, version, transfer and backup your database schema
- You can [get a lot of database tables by downloading the package](https://github.com/baraja-core/shop-product) without having to figure out and configure anything
- Relationships between tables become real physical entities
- Database outputs will not be ordinary untyped arrays, but you will get real physical objects
- You get an easy way to perform many operations simultaneously within a single transaction
- You'll easily increase application security and resilience by simply knowing when what happens, and that it happens safely
- You get an easily testable code and database layer
- You'll discover an entire ecosystem around Doctrine that solves many problems elegantly. You'll often find simple solutions to complex problems that are otherwise almost impossible to solve easily
- You'll learn lots of new things, explore new ideas, and use the database to its full potential
- Get rid of complex SQL queries. Doctrine provides a custom query writing interface (DQL) that is very powerful
- Applications will get faster. You will easily discover room for application optimization, take advantage of lazy loading and find application bottlenecks

The long-held opinion of the author of this article ([Jan Barasek](https://baraja.cz)) is that Doctrine is the best way to work with a PHP database. It simply has no competition.

How to get started?
----------

Before you start using Doctrine fully, you need to prepare a suitable environment. If you are just starting out with PHP or don't have senior knowledge, the best choice is to install the Nette Framework with the [Baraja Doctrine](https://github.com/baraja-core/doctrine) extension package, which automatically integrates full support. First download the package via [Composer](/composer), then set up the DI Extension and Doctrine will start working automatically.

For Doctrine to work correctly, you need to prepare an empty database (Doctrine can work with an existing project, but for the first steps this is inappropriate as it risks overwriting existing data) and configure the connection. Since Doctrine is not just a database library, but provides an advanced database framework, you need to [solve other configuration](/configure-connections-with-baraja-doctrine). Most of the settings are automatically overwritten in that package for Nette, however in the minimum configuration your server must support the `APCu Cache` or `SQLite3` extensions.

If everything was configured correctly, a new DI service `Baraja\Doctrine\EntityManager` will be created in Nette, which you can [inject](https://doc.nette.org/cs/3.1/di-usage) into Presenter:

```php
<?php

namespace App\FrontModule\Presenters;

use Baraja\Doctrine\EntityManager;

final class HomepagePresenter extends BasePresenter
{
	#[Inject]
	public EntityManager $entityManager;
}
```

If you manage to inject the basic EntityManager service, you can start learning and working with Doctrine.

How to proceed?
--------

The following chapters are a combination of a Doctrine technology reference guide, years of experience, design patterns, and ready-made solutions. Together we will go through all the basic elements of Doctrine from defining your own entity, to generating a physical database schema, to working with a versioning tool and production deployment.

I have been using Doctrine for a very long time and have solved thousands of cases in it. We will show tips and tricks on how to use Doctrine to optimize database speed and how to design a database appropriately. You can also use Doctrine for an existing project (if you meet certain conditions) and we'll show you how to do it.

This series of articles was created to help my training and consulting students. If you need to discuss or explain certain topics in more detail, you can email me at jan@barasek.com. Because this is a relatively demanding technology, all questions will be treated as a paid consultation.
