Autoloading classes in PHP
==========================

> id: f6cd5762-261f-4153-b27b-075dd8b5ed13
> slug:
> 	cs: autoloading-trid
> 	en: autoloading-classes-in-php
> 
> publicationDate: '2020-02-09 10:00:29'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '441a1d4107bb32e8cfe4dbb926c2decd'

I'm sure you know this, when programming PHP scripts we split the code into many files and to have all the parts available we load them with a series of `include`, `require` or preferably `require_once` calls, which guarantees loading just once.

In the code it looks like this:

```php
require_once 'Router.php';
require_once 'Page.php';
require_once 'Paginator.php';
```

The main disadvantage of this approach is that the programmer has to constantly make sure that everything is always loaded. However, if he loads a lot, he loses performance unnecessarily and reaches the disk many times. So the manual solution has only problems.

Native autoloading
-------------------

Fortunately, there is native support in PHP for so-called **Class Autoloading**, which is logic in the code that loads a class file only when it is first needed (typically when an instance is first created).

A simple implementation might then look like this:

```php
spl_autoload_register(function (string $className): void {
    include 'src/' . $className . '.php';
});

$obj = new MyClass1();
$obj2 = new MyClass2();
```

When creating an instance of `MyClass1`, the `spl_autoload_register` function reads the `MyClass1.php` file from the `src` directory. This implementation assumes that each class is in a separate file called by the class or interface name.

More complex cases of autoloading
-------------------------------

In a real application, many unpleasant situations can arise that complicate autoload, for example:

- The class or interface does not exist at all
- The file has already been loaded once
- There are multiple classes or interfaces in the same file
- The path to the class or interface does not match the name
- The location of the class or interface changes over time

However, we don't need to program our own solution for all this, but can use the existing solution once designed.

If you are using <a href="https://getcomposer.org/doc/01-basic-usage.md">Composer</a>, you are probably using its native autoloading as well. This is because when you install any package, Composer automatically generates a `class map`, which is an overview of the classes and their physical location.

Then at the beginning of the code (typically in `index.php`) you just use:

```php
require __DIR__ . '/vendor/autoload.php';
```

However, autoloading is only generated once when the `composer dump` command is called, so autoloading needs to be regenerated every time the application changes.

RobotLoader - an elegant solution for development
----------------------------------------

For local development, I really like the `nette/robot-loader` package, which automatically traverses the directory structure and caches the class locations. So if we load a class, it first looks in the cache and only if it doesn't exist, it automatically reindexes the project. Therefore, the programmer doesn't have to keep track of where any file or class is located at all and just programs.

Installation via Composer:

```
composer require nette/robot-loader
```

The basic explanation of the functionality is described in the <a href="https://doc.nette.org/cs/3.0/robotloader">documentation</a>:

> Similar to how Google's robot crawls and indexes web pages, RobotLoader crawls all PHP scripts and notes which classes, interfaces and traits it found in them. It then caches the results of its research and uses them in the next request. So you just need to specify which directories to browse and where to cache.

It is then extremely easy to use:

```php
$loader = new Nette\Loaders\RobotLoader;

// add the directories to be indexed by RobotLoader
$loader->addDirectory(__DIR__ . '/app');
$loader->addDirectory(__DIR__ . '/libs');

// set caching to disk in the 'temp' directory
$loader->setTempDirectory(__DIR__ . '/temp');
$loader->register(); // start RobotLoader
```

Set `$loader->setAutoRefresh(true or false)` to specify whether RobotLoader should reindex files when it encounters a new class. This should be disabled on production servers.

There, now you never have to deal with autoloading again.

Combined solution
------------------

I use a combined solution when developing a real project.

The way it works in real life is that I load the installed packages via Composer autoload (which is very efficient) and this solves the loading of all classes in the `vendor` directory.

The code for the specific project is then placed in the `app` directory, where I handle autoloading just a few classes via RobotLoader. The important thing is to always keep the concrete application as small as possible and use ready-made packages as much as possible, it promotes reusability a lot.

The structure of the project looks like this:

```
/app
    Bootstrap.php <-- configuration
    /model
        UserForm.php <-- project classes
        RegisterFactory.php
        ...
/vendor
    ... <-- libraries
/www
    index.php <-- initialization
```

Testing autoloading
------------------------

Sometimes it may happen that not every file will always load and you will find problems.

For debugging, I recommend the <a href="/get-list-of-all-loaded-files">get_included_files()</a> function.
