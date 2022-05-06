Refactoring a legacy PHP project - how to catch up on technology debt
=====================================================================

> id: '8f37305a-e9dd-4b1e-9f53-04f0fca648f7'
> slug:
> 	cs: refaktoring-legacy-php-projektu
> 	en: refactoring-a-legacy-php-project---how-to-catch-up-on-technology-debt
> 
> publicationDate: '2021-05-11 20:45:00'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: b13673b23a86bc82a88636e4a9464b4b

When consulting with knowledgeable and experienced project owners, I often come across the question of the long-term sustainability of a digital project. Many large projects that exceed 3 years of development start to become internally obsolete and are no longer sustainable - now imagine a team of developers with varying levels of knowledge, experience and most importantly, diligence.

In order to keep your digital project at the TOP level technically, you need:

- **Competent developers and a project manager** who have great communication and everyone knows what they're doing and how it impacts the others,
- **Functional tools and workflow** that the whole team knows and adapts their work to others accordingly,
- **Automated tasks** that provide a daily routine that is so easy to forget it is extremely important.

If you're developing a large project, you simply don't have it easy. It's important to do things right, but it's more important to do the right things.

What is refactoring and why do you need it
---------------------------------------

The concept of refactoring encompasses a set of activities that modify the appearance and internal logic of a program's code without affecting the surrounding environment. You can think of the refactoring process as a Christmas cleanup - it stays the same, but is much better organized, and the unnecessary stuff is thrown away. With refactoring, it's also important to keep in mind that it's not a one-time event, but a long-term process that you have to repeat in regular cycles. If you don't do this, you're in for all sorts of strange mistakes, financial losses, onboarding problems and crying foul.

If you can keep the project stable and up to date, you'll gain some great benefits:

- The project will be **safer**. Patches for bugs and security vulnerabilities are released regularly within the libraries you use. You must address security, it is one of the basic life functions of a healthy project.
- The code and internal architecture will become simpler and better thought out. You'll be better able to find bugs, and many bugs can even be prevented.
- With simplified code, you can also bring in junior developers to <a href="https://www.youtube.com/watch?v=1wZtdIb-Ppk">significantly reduce</a> your overall budget.
- You may find that you're making some things too complicated and overcomplicated - the project will be simplified overall.

In order to stay on track with a large digital project, you have to run. But you want to grow.

How to refactor safely
-------------------------

Every refactoring is a big bet that may not pay off.

I always ensure a stable environment long before I embark on refactoring.

For stability, you especially need **functional error logging**. For simple projects, you just need <a href="https://tracy.nette.org">Tracy</a>, which logs errors to an HTML file. For more advanced projects, tools like <a href="https://sentry.io/welcome/">Sentry</a> or <a href="https://rollbar.com">Rollbar</a> come into play. I personally use Tracy on all projects, other tools depending on the type of project.

About a month before refactoring, I start tracking bugs and create a spreadsheet of known bugs to focus on later. It's always important to know which bugs were introduced by refactoring, and which ones the project already contained in the past. This will then help to better defend the results of the work to the client.

Once I know, thanks to the log, that I'm working on a relatively stable environment, we can get down to the hard work. Other headings include descriptions of methods according to how much risk lurks behind them.

Fixing Coding Style and Coding Standard
-----------------------------------

Most projects do not have a consistent code formatting style. This is a big mistake. A well-written project looks like one person's project, where all things are written the same.

Basic formatting errors are well corrected by the <a href="https://doc.nette.org/en/3.1/code-checker">Nette Code Checker</a> tool that I regularly run over the entire project.

Coding Style, on the other hand, covers how the code is formatted and the indentation of individual language expressions. The <a href="https://www.php-fig.org/psr/psr-12/">PSR-12</a> standard is used a lot in the PHP world, and very many other standards are based on it. I recommend using.

Some projects awkwardly combine <a href="/spaces-and-tabs">tabs and spaces</a>. To effectively prevent breaking the whitespace convention (and other errors), create a `.editorconfig` file on the project root that tells your editor how to format newly written code.

I personally use this configuration:

```
root = true

[*]
charset = utf-8
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true

[*.{php,phpt}]
indent_style = tab
indent_size = 4
```

Also, fix all <a href="/apostrophes-and-carriage-notes">carriage-notes to apostrophes</a>. This can be done automatically.

You can also check the formatting of your code automatically, I've prepared a <a href="https://github.com/baraja-core/sandbox/blob/0db1f41068b6cedf79500c6a8b0ac26eae94a8eb/.github/workflows/coding-style.yml">fully functional demo on GitHub</a> for this. If you also need to auto-correct the code, this can be done with the same tool. PhpStorm is also very good at automatically fixing code directly, even in bulk over the whole project.

How to fix Coding Style for large projects
----------------------------------------------

For large projects, I recommend that you do automatic code formatting one module at a time, as this can create a huge number of conflicts in Git. Therefore, the best strategy is to fix as many lines as possible with a single large commit, push that commit directly to the master, and then distribute the change to the other branches.

If the conflicts are too large, it makes sense to format the code on the master first, and then again on each large branch where there are a lot of changes. Very many changed lines will cancel each other out (do a fast-forward), so you will resolve very few conflicts, or sometimes no conflicts.

If you do nothing, the code will still be bad. Iterative rewrites over many years definitely don't work, because each merge introduces the need to fix dozens of lines where no change actually happened, and it unnecessarily complicates both code review and potential change reverts.

PhpStan - static code analysis and type error correction
------------------------------------------------------

Before embarking on a large-scale logic fix, it's very important to review and fix **basic project lifecycle features**. These include such obvious things that you expect from any project, but still sometimes are not met.

For example:

- All classes, methods and functions must exist
- Class inheritance must not be broken
- Classes must implement the interface or ancestor interface used. At the same time, you must not inherit `final` classes
- You must not call unsafe functions such as `eval()`, `shell_exec()`, `var_dump()` and so on. And if you call them anyway, it must be explicitly stated in the comment
- You must always catch exceptions and not let the whole application crash

The solution to this problem is to install <a href="https://phpstan.org">PhpStan</a> in the project and fix it **at least to level 1**. Yes, it's hard, and yes, it's a lot of work. But if you don't do it, every refactoring becomes Russian roulette and the developer just hopes that the damage is as minimal as possible.

The base level of PhpStan doesn't require that data types and other formal things exist everywhere, for example, which we'll address in the future. On the other hand, it may not be the case that a function or method will return a certain data type but assert something else in the typehint.

Rector - safe iterative repair
-----------------------------------

A well-known programming dictum says that before adding a new error to the system (such as throwing an exception), we should first add that error as a warning, and only then turn it into a fatal error if it doesn't manifest itself for a long time.

It is the same with data types. When refactoring unknown code, I let automatic tools like <a href="https://getrector.org">Rector</a> add comment annotations to the code first, which doesn't break anything, but helps clarify what will be where later. These comments are then listened to by PhpStan, which can be used to safely verify that we haven't broken anything and it's a safe modification.

I generally add comments to properties and arguments in one commit, then wait for maybe a month, and when everything works in the long term, I resort to rewriting to fixed data types in places where there hasn't been a problem in the long term.

See the article <a href="https://tomasvotruba.com/blog/2019/07/29/how-we-completed-thousands-of-missing-var-annotations-in-a-day/">How we Completed Thousands of Missing @var Annotations in a Day</a>.

For more on <a href="https://github.com/rectorphp/rector">Rector, see GitHub</a>.

Updating dependencies
----------------------

Before getting into updating dependencies on packages or even PHP versions, it's important to research and learn how to use <a href="/github-actions-best-ci-for-2021">automated tests</a>.

If the tests are working, we can go to the <a href="/composer">Composer</a> tool to perform the update. If you can, enlist the help of a <a href="https://dependabot.com">Dependabot</a> robot that updates your project's `composer.json` automatically and can check for compatibility changes.

If you have a large set of changes coming, always make them slowly and increment them version by version. Never update many packages at once. After each update, scan the entire project with PhpStan and fix bugs. It's a long process that takes several hours, but the stakes are high.

Where to go next
-------

Next steps are individual depending on the type and status of the project. In general, well-designed code written by senior developers is maintained orders of magnitude better than code bought cheaply from a junior who worked at a media agency that has web development more as a sideline, so to speak.

Fingers crossed! It's going to be tough, but you'll get through it.
