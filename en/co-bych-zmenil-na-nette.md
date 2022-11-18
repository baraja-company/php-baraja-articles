What I would change about Nette if I were David Grudl
=====================================================

> id: bad8fe4f-92a4-4396-8698-7ec42bb2aef8
> slug:
> 	- null
> 	en: what-i-would-change-about-nette-if-i-were-david-grudl
> 
> cs: co-bych-zmenil-na-nette
> publicationDate: '2022-11-18 17:45:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '953e6c78b9dff185bbf8e3e9582d24c3'

I have been actively using Nette Framework for almost 6 years for commercial software development. During the first years I was very enthusiastic and the framework perfectly addressed all the needs we had as a team, and there was basically no reason to look for another tool.

Since about 2019, I'm starting to see some fall-off of the original enthusiasm within Nette, where I'm starting to miss some of the advanced features. Often these are things beyond the framework itself, so I don't even expect them to ever be implemented, but on the other hand, a developer has to make a decision on how to continue development going forward, and maybe reach for a different technology.

Database layer
-----------------

I consider Nette Database to be one of the biggest mistakes of the framework, unfortunately.

Every week we have interviews at the company where juniors right out of school, or even intermediate people who are transitioning from another framework, apply and discover Nette Database as part of their exploration of Nette. As a database layer, it's not too bad, but the problem then is practical use in real projects.

This is because Nette Database makes no effort to push developers to use strict data types from the start, to name columns and relations, to separate query (repository) methods from models, and other little nuances that do a lot overall.

For many years I used the Dibi library, which played a huge role in its day. It allowed you to query the database quite nicely and unified the interface. On the other hand, times have moved on, and when databases return untyped objects, PHPStan just won't be happy just on principle.

Personally, I would either incorporate native support for entities and repositories into Nette Database, or provide official support for Doctrine within Nette. If you're programming applications at the level of a large corporation, you have to be serious about development, and Doctrine in particular makes a lot of sense. At the same time, you want to educate newcomers in better technology right away, and you don't want to teach them old habits.

Tracy and error logging
---------------------

I still consider Tracy to be the best runtime debugger for PHP.

When I came to an unnamed digital agency in 2017, and saw the technical state of their Nette projects, I was horrified. How many times they had sites written in pure PHP without any attempt to log errors. A fairly easy solution was to install Tracy on the project, call `Debugger::enable()` and do nothing more. The bugs were automatically logged to a directory that just needed to be manually picked up every few days.

As for Tracy, it seems to me that it is a finished product that David has little reason to improve. On the other hand, I still see room for improvement in a new direction.

For example, why doesn't Tracy include paid features for companies or individuals who are serious about security?

Tracy could implement a custom Sentry-type web interface where you create an account, and production bugs are automatically sent through an API where you can track them across projects. The app could also request individual sites, and automatically detect that it's unavailable, and notify the owner in other ways (since Tracy can't logically detect a server crash). I also sometimes run into the problem that Tracy is accidentally enabled on a production site, and you can find out through DIC how to access the database or elsewhere. Tracy should alert you to this as well.

Tracy could also implement other little things that I know from Node.js. For example, for the source code display, it could be possible to choose the character interval at which the line is highlighter in a special way, why the possibility of highlighting a specific token within a line. At the same time, it would be nice to add support for a second (perhaps blue) highlighting line to show context in the next part of the application.

When displaying a snippet of source code, I wouldn't be afraid to add a button that can render the rest of the code in ajax so I can explore it directly in the browser from the callstack. This is what Symfony does, and it's a great feature. If this was complemented with basic static analysis with the ability to crawl through methods, classes and inheritance, it would save a lot of debugging work for the whole team.

If the callstack is traversing a package, it would be nice to show the version of the package I'm working with for a particular file. Often I'll throw an error in a package I'm developing, and I might as well visually know it's an older version.

Static routing
----------------

Within Nette Router, I would allow to define a new type called static router. It would be a descendant of the basic router, but it would only be able to accept string-literal, which would not be a regular expression, but a straight path. A lot of pages work statically (contacts, various articles, surprisingly often even homepage), and the router has to evaluate regex rules for match and URL composition every time because of this.

If a static route were used in a Latte template, for example, it could be safely translated to a static `{$basePath}/path` string at template compile time, so there would be no need to compile the 100 URLs that are in the layout, for example, on each request.

I understand that I can achieve the same functionality by caching on the template side, but I would appreciate a system solution a lot more.

In the Next.js framework, I completely fell for the concept of static routing. There's no such thing as `n:href`, in short you have to know the URL in advance, which forces you to not do so many redirects, think about URL formats in advance, and keep them persistent.

PSR interface
------------

I find it a shame that Nette doesn't natively implement the PSR interface. As a package developer, this leads you astray if you want to define a Composer dependency on Nette at all. On the one hand Nette solves a lot of problems for you, on the other hand you know you simply won't replace it afterwards. More than once I've found myself preferring to use Symfony, Laravel, or a completely different generic interface because of the lack of a generic interface.

The lack of support for generic standards for future portability is the main reason why I walked away from Nette completely in 2022 and we develop new applications in teams exclusively in generic.

API layer
----------

What if you want to use Nette as a backend to handle REST API requests? Do you build a presenter and send the response as `$this->sendJson()`? Will you install a third party library? Or are you David Grudl who solves the need for a missing library by writing a new one right away?

Why can't Nette implement something as common as the REST API right out of the box? Today, almost every application needs to communicate via an API - for example, to provide data to the frontend.

For newcomers, this again leads them to use the built-in Latte and snippets rather than finding out that there is a better option, and that is to build the entire frontend separately alongside, and just provide data to it.

Trusting what will happen in 10 years
-------------------------

The last point is a very pragmatic one, and comes from a debate we hear from time to time from business departments in the team.

What would happen if David Grudl stopped developing Nette altogether? What if anyone stopped developing it? What would we switch to? And how challenging would that be?

Many developers (often with no corporate experience) like to scoff at this concern, saying that it's okay. Like yeah, but it's not.

When you're grappling with the question of whether to invest time in Nette or React and Node.js to train junior developers in your company, unfortunately the latter option wins.

Nette hasn't missed the train yet, but in the dozens of interviews I've had over the last six months, I feel it pretty strongly.
