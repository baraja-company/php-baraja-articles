How to deal with sudden PHP script crashes
==========================================

> id: '41edbf1b-3ca5-487f-a54c-fac9926bc3d2'
> slug:
> 	cs: senior-7-sessions
> 	en: how-to-deal-with-sudden-php-script-crashes
> 
> publicationDate: '2023-02-11 14:30:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: bdd1e207ee9c0ba19c39f0b5c7d83c43

A story from the end of 2016, when I was literally saved by a colleague: in a PHP application, you decide to check in images via a proxy script, which, among other things, can adjust their dimensions and other parameters according to the incoming request. As part of the optimization, you also save the generated variants physically to disk.

However, in production operation, you suddenly start seeing a huge load and thousands of requests queued up. The images are loaded sequentially one by one for each user. Page refreshes and link clicks don't work. The application seems completely frozen. It only works to wait for everything to process.

What could be the problem? I have listed 3 major clues in the text to enable a quick search for the problem. Hotfix has a trivial solution.
