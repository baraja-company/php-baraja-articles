Composer - complete overview of advanced features
=================================================

> id: a74d8d59-91ce-4602-ad52-80cf89a647bd
> slug:
> 	cs: composer
> 	en: composer---complete-overview-of-advanced-features
> 
> perex: Composer je pokročilý správce balíků a závislostí pro vaše PHP aplikace. Článek popisuje jeho všechny výhody a možnosti použití.
> publicationDate: '2020-03-10 20:18:19'
> mainCategoryId: '4f1d7d70-c5b0-45f1-b1d2-d03c22aa4154'
> sourceContentHash: '68340d4b4d3c8a6ed143ede176fbf04e'

As you already know, [Composer](https://getcomposer.org/) is a robust package and dependency manager for PHP, through which you can elegantly manage hundreds of projects at once and distribute code once written to all applications simultaneously.

This tutorial serves as a detailed comprehensive developer's guide. We'll cover all the important advanced techniques for working with Composer, as well as explain the technical details and relevant dependencies.

Installing Composer
-------------------

Regardless of the platform, we download from [the official Composer website](https://getcomposer.org/).

Internally, the PHP file `composer-setup.php` is downloaded, which when run in CLI mode can install Composer. It is also important to know that Composer does not work without PHP, so first verify that you have PHP running on your computer (it only needs to be available for Terminal).

On Mac and Linux, Composer works right after installation and you just need to call the `composer -v` command to quickly verify that Composer is installed correctly.

On Linux, the following command can be used to install: `/usr/bin/php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"`

On Windows, it's a good idea to install the [Git Bash for Windows](https://gitforwindows.org/) tool, which allows you to open a specific Terminal that behaves almost like Linux and lets you work in the same environment as on Linux.

The installation for servers is the same as on a local environment. Just make sure you have the correct version of PHP that Composer uses internally.

Available commands
----------------

Composer implements a number of commands.

The usage is: `composer <command>`, for example: `composer update`.

Overview for `1.10.0`:

| Command | Description / Meaning |
|-----------------------|----------------|
| `about` | Displays brief information about Composer. |
| `archive` | Creates an archive with the contents of the selected Composer package. |
| `browse` | Opens the home page of the selected package, author or other related page in a web browser. Often may contain documentation on how to use it. |
| `cc` | Clears the internal Composer cache with versions of packages downloaded in the past. |
| `check-platform-reqs` | Check if the installation requirements are met for the current platform. |
| `clear-cache` | Clear the internal Composer cache. |
| `clearcache` | Clear internal Composer cache. |
| | `config` | Sets a configuration directive.
| `create-project` | Creates a new project based on the selected package and automatically creates a folder to place the project in.
| `depends` | Shows which packages caused the selected package to be installed. |
| `diagnose` | Diagnoses the system to identify common errors. Processing of the output is up to the developer, it is just a listing. |
| `dump-autoload` | Generates a new <a href="/autoloading-trid">autoloader</a>. |
| `dumpautoload` | Generates a new <a href="/autoloading-trid">autoloader</a>. |
| `exec` | Executes binaries and scripts from Vendor. |
| `fund` | Explores how to make/modify your dependencies. |
| `global` | Allows you to run global Composer commands from the `$COMPOSER_HOME` variable. |
| `help` | Prints help for commands. |
| `home` | Opens the home page of a specific package in the browser. |
| `i` | Installs all project dependencies according to the `composer.lock` file, if it exists and is valid. If there is a problem, the information from `composer.json` is used and `composer.lock` is restored to its original state. |
| `info` | Displays information about the currently installed packages in the project. Displays the names of all packages, their current versions and a short description. |
| `init` | Creates a basic `composer.json` function in the current directory. |
| `install` | Installs all project dependencies according to the `composer.lock` file, if it exists and is valid. If a problem occurs, the information from `composer.json` is used and `composer.lock` is reverted to its original state. |
| `licenses` | Displays a list of all packages, their versions and current license. |
| `list` | Displays a list of available commands. |
| | `outdated` | Displays a list of all packages for which there is a newer version available for installation and which meet the dependencies. For each package, the latest compatible version that Composer suggests for installation will be displayed. |
| `prohibits` | Shows which packages and dependencies prevent the installation of the requested package or version. |
| `remove` | Removes the package from the `require` or `require-dev` configuration section. |
| `require` | Adds the requested package to `composer.json` and installs it. If the dependencies cannot be satisfied, it will revert to the original state. |
| `run` | Runs the defined scripts in `composer.json`. |
| `run-script` | Runs the defined scripts in `composer.json`. |
| `search` | Searches packages by keyword or search query. |
| `self-update` | Updates the internal `composer.phar` to the latest version. |
| `selfupdate` | Updates internal `composer.phar` to the latest version. |
| | `show` | Displays detailed information about the currently installed packages. |
| `status` | Displays a summary of local changes made to packages manually, based on a comparison with the source of the package from which it was originally installed. |
| `suggests` | Displays suggestions for packages. Suggestions can include various kinds of actions, such as installing security updates and so on. |
| `update` | Updates the entire project according to the dependencies, so that they are always all satisfied by `composer.json`. If successful, it updates `composer.lock`, where it writes the currently installed versions. |
| `upgrade` | Alias to `update`. |
| `u` | Alias to `update`. |
| `validate` | Checks `composer.json` and `composer.lock` for syntax errors. |
| `why` | Shows which packages caused the currently selected package to be installed, including all dependencies. |
| `why-not` | Displays which packages and versions prevent the installation of the selected package or version. |

Creating and defining a project
----------------------------------

Each project managed by Composer is defined by a `composer.json` file in its root that defines all dependencies. The file can be created either manually for an existing project or automatically when creating a project.

Since everything in Composer is a package, the project itself can be based on a package. So, for example, if you are creating dozens or hundreds of very similar projects, it makes sense to put their basic configuration and structure into a separate package on which to base the installation.

An example of such a package is my [Baraja Sandbox](https://github.com/baraja-core/sandbox), which is based on pure Nette 3.0 and adds a basic dependency to my [Package Manager](https://github.com/baraja-core/package-manager), which I use for all projects and dependency management in the Nette configuration.

The sandbox is then installed simply with the command:

``shell
$ composer create-project baraja/sandbox <your-project-name>
```

Based on the project name, Composer will then automatically create a folder to install the project into (copy the package contents and install the dependencies).

In the `vendor` folder, Composer then manages all the packages (their physical files are there) and generates an autoload of classes, which we ideally put directly into `index.php` as a line:

```php
<?php

require __DIR__ . '/vendor/autoload.php';

// The application code itself
```

Installing other packages and dependencies
-------------------------------------

Within a functional project we can install new packages and add dependencies very easily.

There are 2 ways to do this:

- With the `composer require ...` command,
- By adding a dependency directly to the `composer.json` file in the `require` section, and then using the `composer update` command.

Try installing for example PackageManager: `composer require baraja-core/package-manager`, or [Doctrine](https://github.com/baraja-core/doctrine): `composer require baraja-core/doctrine`.

If the chosen package cannot be installed, specific reasons can be asked and Composer will list the dependencies that prevent this. Often it is enough to fix a dependency on a particular version or remove incompatible code. For detailed information, use the command: `composer why baraja-core/doctrine`.

Updating projects and packages
-----------------------------

A well-designed project is developed so that you can easily download updates over time and always have the latest versions of all packages. The main benefit is that you get all bugs fixed, often performance improvements and lots of new features. In addition, the gradual changeover will make updating less complicated after a long time, as you'll be fixing problems on the fly on a smaller scale and avoiding incompatibilities.

To update all packages and dependencies, use the `composer update` command.

The update process itself may fail in some cases. The reason is usually either a broken dependency or an incompatible package.

For detailed information on why a package cannot be installed, use the command: `composer why-not baraja-core/doctrine`. If we already have the package and only the specific version is not working (it does not want to install), we can also ask for the specific version: `composer why-not baraja-core/doctrine:v1.0.20`.

Within the `composer.json` file, we can also list the dependency on a specific runtime. This is especially useful when we are developing a project with multiple people and want to verify that they have all the extensions installed.

Typically, the version of PHP is checked (must be `7.1.0` or later):

``json
{
   "require": {
      "php":">=7.1.0"
   }
}
```

Possibly other system extensions:

```json
{
   "require": {
      "php":">=7.1.0",
      "ext-json": "*",
      "ext-session": "*",
      "ext-PDO": "*"
   }
}
```

When installing packages or upgrading, these rules are then also taken into account. This helps prevent problems that would show up at runtime. Typically, for example, a payment gateway package needs to communicate with the API, so it must admit dependencies on the `curl` and `json` extensions.

Troubleshooting dependencies
-----------------------------

Often, dependency violations occur because of a bad version of PHP. In this case Composer throws a message for example `your PHP version (7.3.11) overridden by "config.platform.php" version (7.1) does not satisfy that requirement.`.

Very often the error is caused by the settings directly in the project `composer.json`, where the following section is:

``json
"config": {
   "platform": {
      "php": "7.2"
   }
}
```

The change **must be made directly in the file**. In the case of a global project (before installation or with a global dependency), the Composer version can be forced with the command: `composer config -g platform.php 7.2.14` (the `-g` switch means `global`).

In some cases, we want to install the latest package versions and ignore the local environment settings. In this case, we can use the advanced command:

``shell
$ composer update --ignore-platform-reqs
```

**Use the `ignore-platform-reqs` switch at your own risk, it may cause damage to the project!** Do not use if you do not understand the implications.

Manual Composer calls, parameters and memory management
------------------------------------------------------

Composer is actually a PHP script wrapped in a so-called PHAR, which is a compiled version of a PHP application. Knowing this information can be put to good use, for example to better parameterize the call itself.

In really large projects, it sometimes happens that we run out of RAM and need to allocate much more, or increase the processing time of the scripts.

For example, on Windows you can use this command for that:

```shell
$ php -d memory_limit=-1 C:/xampp/htdocs/composer.phar update
```

The `memory_limit=-1` switch tells Composer not to be susceptible to the RAM limit, and to consume as much memory as it needs.

Custom user scripts after Composer action
--------------------------------------------

After running Composer actions, you can invoke automatic execution of user-defined scripts that either perform specific actions on the project or, for example, generate a configuration after deployment. I mainly use this approach on a local server to offer an automatic database configuration tool, generate a database schema, etc.

We register the required scripts in the `scripts` section of `composer.json`:

```json
"scripts": {
   "post-autoload-dump": "baraja\\PackageManager\\\PackageRegistrar::composerPostAutoloadDump"
}
```

In this case, the static method `composerPostAutoloadDump` in the `Baraja\PackageManager\PackageRegistrator` class is automatically called. Composer has this class available because it first performed the generation of the <a href="/autoloading-trid">autoloader classes</a>.

If we just want to run the scripts and not perform unnecessary actions with Composer, the `composer dump` command is very useful, as it just generates a new autoloader (which I recommend to always keep it up to date) and then runs the scripts immediately. If you want to try using scripts, I have prepared a ready-made package [Baraja PackageManager](https://github.com/baraja-core/package-manager) that implements smart scripts and interactive interface for Nette framework.

Versioning Vendor into Git?
------------------------

A question we often discuss with developers is whether to version the contents of the `vendor` folder into Git, or have it re-generated on every installation.

In general, the cleaner solution seems to be to **not version** the content at all and install everything each time. But the reality is that from time to time a developer will stop developing a package, or remove it altogether. The constant downloading of packages also complicates local installs and updates, as well as slowing down deploys, and can sometimes be responsible for brief site outages when new package versions are downloaded incorrectly.

I see Vendor's suspension as a form of "security". If the files are physically in the versioning system, we have at least a rudimentary assurance on the production server that the packages will work and their code is exactly as it is in the locally running installation. In addition, Vendor often takes only units of MB, which is certainly worth the assurance of a working site given the capacities of today's disks.

**Practical note:** For the average site I maintain, `vendor` takes up no more than `30 MB`, which is an acceptable capacity for Git. When cloning a repository with junior, we then don't have to deal with training him on how to get the site up and running, and it just "works right away".

Custom Composer packages
-----------------------

You can create your own packages within Composer, either publicly available (registered at [Packagist](https://packagist.org/)) or private (you must have your own package server, for example [Satis](https://getcomposer.org/doc/articles/handling-private-packages.md)).

The issue of creating, maintaining, developing and versioning packages is very complex and will be the subject of a separate article.

In the meantime, you can read the article [Semantic Versioning](https://semver.org/lang/cs/), which I have translated for you.
