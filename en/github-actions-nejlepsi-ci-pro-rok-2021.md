GitHub Actions - the best CI for 2021
=====================================

> id: c1fe3b77-a506-4f20-bf1e-90ee82817c7d
> slug:
> 	cs: github-actions-nejlepsi-ci-pro-rok-2021
> 	en: github-actions---the-best-ci-for-2021
> 
> perex: 'Protože už nějaký čas vyvíjíte webové aplikace, určitě jste si všimli, že se pro vás celá řada věcí rutinně opakuje, i když by nemusela. Velmi často jde o technickou správu projektů, verzování souborů, automatická kontrola kódu, zpracování různých dat, nebo třeba deploy na server a bezpečnostní věci kolem toho.'
> publicationDate: '2021-02-07 15:46:39'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: d3d151927a02dde744d1202449c904dc

Since you've been developing web applications for a while now, you've probably noticed that many things are routinely repetitive for you, even though they shouldn't be. Very often it's technical project management, file versioning, automated code review, various data processing, or maybe deploy to the server and security stuff around that.

In consulting in companies, I very often run into the problem where prevention is very much underestimated. The reason is often that developers perceive some things as very challenging and that it will add work for them. But the truth is that you usually only need to set up the whole setup once and then just reap the long term benefits.

What is CI (Continuous Integration)
----------

CI/CD is a tool that can automate routine tasks that have a similar basis and keep repeating in projects. The most common use of CI is for running automated tests, code review, automated deploys (deploying an application to a web server), project management, or perhaps security audits.

Think of CI as a virtual operating system that executes predefined commands to the Terminal or runs specific programs. The output of CI is either a success or a failure, which is displayed directly in the project, but also sent to administrators who can react to it. A good practice is to run CI jobs following a specific event (for example, a commit to the repository, a merge request/pull request received, a tag/version/release, or a cron timeloop run).

How CI specifically works
------------

The basis of CI is a configuration file where we define its behavior and response to events. In the case of GitHub, you just need to create a helper directory `.github/workflows` and create any file with the `.yml` extension inside. GitHub will parse it with each commit and execute it as needed.

Let's imagine we want to define a new configuration file with a specific task. Since this is a task that will be run directly by GitHub or another environment on the virtual machine, we need to define basic things about the environment, including:

- The name of the task (for example, the name of the test)
- Trigger events (which event to trigger the task based on, for example, every time you push to the repository or receive a pull request)
- The definition of the tasks themselves (there can be many tasks running in parallel)

In the case of jobs, we further define:

- Job name
- Running environment (typically the name of the operating system)
- Steps to build the container

The above container is already the first preparation for **complete dockerization of the project**. The basic use of CI is relatively easy and you don't need to understand Docker at all, just know the commands in Terminal.

As part of the specific steps in the task, we need to run the individual commands that will build the installation of the operating system we just installed. So in the case of PHP, it will be important to install just the PHP language (and specify the version), clone the project repository to the runner, install the project dependencies (most often [Composer](/composer)), and run the test itself.

Why use GitHub Actions (CI)?
--------

There's a common superstition in the IT community that Microsoft is the evil corporation that makes low quality stuff. In the case of GitHub Actions, however, this is not true at all, because they have, quite objectively, the best CI platform in the world.

The fundamental advantage of GitHub CI is the simplicity and elegance of the notation. With just a few lines, you can configure your own test environment and manage it automatically, even without prior knowledge. At the same time, I see the speed of processing specific tasks as a huge advantage. For example, a test on codestyle (code formatting) and PhpStan (checking data types, logic and general correctness) can be evaluated by GitHub Actions on a regular project in under 50 seconds, while GitLab, for example, evaluates the same test in almost 8 minutes! Other solutions like Travis are relatively expensive and most developers don't appreciate the benefits of their special features anyway.

Prepared actions
-----

GitHub has a large database of ready-made actions and packages that you can use right away. These are typically operating systems, programming languages, or libraries from the community.

You can find a database of actions directly in the [Marketplace](https://github.com/marketplace?type=actions), for example my favorite action is [Send SMS in case of failure via Twillio](https://github.com/marketplace/actions/twilio-sms).

Finished examples
------

Very many examples [I publish within my own repositories](https://github.com/baraja-core) where you can transparently see what specific configuration I use.

For example, in February 2021 I used this configuration for codestyle checking in a project:

``yaml
name: Coding Style

on:
  Push:
    Branch:
      - master
  pull_request:
    types: [ assigned, open, synchronize, reopened ]
  schedule:
    - cron: '1 * * * * *'

jobs:
  nette_cc:
    name: Nette Code Checker
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: shivammathur/setup-php@v2
        with:
          php-version: 7.4
          coverage: none

      - run: composer create-project nette/code-checker temp/code-checker ^3 --no-progress
      - run: php temp/code-checker/code-checker --strict-types --no-progress

  nette_cs:
    name: Nette Coding Standard
    run-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: shivammathur/setup-php@v2
        with:
          php-version: 8.0
          coverage: none

      - run: composer create-project nette/coding-standard temp/coding-standard ^3 --no-progress --ignore-platform-reqs
      - run: php temp/coding-standard/ecs check src
```

This config file installs the latest Ubuntu (`runs-on: ubuntu-latest`), clones the project repository (`uses: actions/checkout@v2`), installs PHP (`uses: shivammathur/setup-php@v2`) version 7.4 (`php-version: 7.4`), installs the code-checker package, and then runs the task via PHP.

A few more notes for users who are not so experienced:

- CI will start a virtual machine with a complete operating system every time it runs, which can be used to call commands in Terminal just like, for example, your server
- To check the test results, so-called return codes are used as defined by Linux itself. Specifically, when a program returns `0` (zero), it means success and any other number means failure. For example, shell scripts work in the same way
- When we want to write a custom test or more complex logic, we can use PHP, for example, and run that as a CLI task (the `php file.php` command), which will run and can do anything it has rights to do (work with files, communicate over the internet, output to the screen, ...)
- When CI runs, all output is recorded on screen and can be viewed directly in the web browser
- CI is not interactive, even though Terminal can run interactive operations with the user, CI does not support this and must be designed as a task that sometimes starts and sometimes finishes
- A timeout of 1 hour is defined for the CI container to run, if everything is not processed in that time (for example, the program stalls), the entire system will be forcefully terminated and an error will be reported
- GitHub can run CI jobs automatically by cron, however very often it does not run them at the right time, but runs with a delay
- You can also wrap all CI logic in a Docker container and run it locally on all machines or on a server, for example
- If you need to access secret information (e.g. database password), there is an option to define secret variables directly in GitHub and CI has them available at runtime
- It is always a good idea to deploy to a web server via SSH
- The total running time of CI jobs is limited, usually to 2 thousand minutes per month (very often this will be enough for you, I have never used it up because one job runs for a maximum of one minute), or you can increase the time for a fee
- You can also host CI runners on your server and bypass the minute limit, but very likely your server won't be much faster because Microsoft has invested heavily in their Azure where GitHub Actions is hosted and it runs on very powerful servers
- Very often it's handy to install specific packages just for CI (typically PhpStan and various security features), so put them in the `composer.json` section of the `require-dev` file so they don't get installed in a specific project as a mandatory dependency

GitHub apps and bots
-----

Unlike other repository hosts, GitHub supports so-called GitHub Apps and automation bots. This is a large database of ready-made bots that can perform automated operations on your projects (even without CI) and when they encounter something, they will, for example, file an issue or send a pull request with a fix.

I personally use and recommend it to you:

- [Dependabot](https://dependabot.com) - automatic dependency checking in Composer for example, when a new version of the packages you use comes out and is compatible, it automatically sends a pull request
- [Codecov](https://github.com/marketplace/codecov) - checks code coverage with automatic tests and graphs which parts of the code have not been covered yet
- [Snyk](https://github.com/marketplace/snyk) - automatically checks for security vulnerabilities
- [Imgbot](https://github.com/apps/imgbot) - automatic image data size optimization

Many other applications can be found in the [Marketplace](https://github.com/marketplace?type=apps).

More handy tips and tricks
-----

I've grown extremely fond of using automation tasks in GitHub.

I have automated checks set up in all my repositories, so when I submit any commit (or anyone else in the community), I get real-time feedback on whether code quality (codestyle and PhpStan), security, and more have been violated.

I have some tests run automatically every hour. For example, if there is a security vulnerability or CVE, I'll know about it in an hour at the latest, even at the level of specific projects and what exactly happened.

Over time, after testing different configurations, I have come to the conclusion that I consider the following dependencies to Composer as the ideal minimum setup:

- `phpstan/phpstan` - checking the code for correctness and logic
- `tracy/tracy` - high level error reporting and logging
- `phpstan/phpstan-nette` - Phpstan extensions and specialties in Nette
- `spaze/phpstan-disallowed-calls` - a brilliant library by Michal Spacek that handles (un)use of dangerous things in code (from forgotten variable dumps to the ability to run a user string as a shell command)
- `roave/security-advisories` - checks for installation of unsafe versions of packages (if a security vulnerability is found in a particular package or a CVE is released, this package will prevent the installation of the corrupted version)

Ideally, you should have the basic security setup set to auto-run at least every 8 hours and have errors sent to your email. Because whenever a project installation fails or a new vulnerability is discovered, I want to know about it as soon as possible and react immediately.

Remember that everything works automatically, so you can always be sure that even if you use complicated processes to check security and other things, it doesn't cost you any extra effort and you just need a well-executed initial configuration.

Because there is huge potential in prevention and automation!
