Introduction to PHP
===================

> id: '66b7fd22-6195-4999-ad8d-b799afdc1876'
> slug:
> 	cs: uvod
> 	en: introduction-to-php
> 
> perex: 'Úvodní tutoriál do jazyka PHP, možnosti jazyka, informace o tvorbě webu v PHP.'
> publicationDate: '2019-09-29 19:25:06'
> mainCategoryId: f4a34087-1b51-4761-8128-4459dfe83d8a
> sourceContentHash: db6ba8fe6d5ca28aab62d7302c1d7527

This is an online course for learning PHP, designed for general education. It is available for free on this website. The texts should not be used in any paid course without written confirmation. You may use the samples used throughout the site in your application without further restriction. [Terms and Conditions](https://baraja.cz/vseobecne-obchodni-podminky).

Upgrade from HTML
--------------

"Upgrade"? It sounds more like a media spin, and indeed it is.

There is no upgrade. PHP retains all the functionality and features of a pure HTML document, it just adds new writing and deployment options. Great, right?

You already know from developing static HTML pages that the code consists of tags, which are defined as keywords enclosed in pointed brackets (for example, `<b>Hello!</b>` expresses the bold text **Hello!**).

PHP is inserted into the HTML page as the `<?php` and `?>` tags, inside of which other application logic is written. Importantly, PHP has its own syntax (the rules by which the code is written) and, unlike HTML, is not error tolerant.

Specific examples of how to do this and what each tag means will be given later. For starters, it's important to understand the general principles of how PHP works on the server and how the code is processed.

The flow of communication between the user and the server
---------------------------------------

For an ordinary HTML page, this is roughly how it works:

> The user sends a request (asks for a specific HTML page), the server looks at the disk and sends back exactly what is stored. Nothing special happens, and don't expect anything more. The pages will just be static, with no possibility of server interaction.

If we add PHP, however, wonders will begin to happen:

> The user requests the page again. The server opens the file on disk, but sees that it doesn't just contain pure HTML, but also special tags that indicate a PHP script. So it first evaluates them and then sends what PHP has generated.

Evaluating PHP code is done by default every time the page is loaded, in the future you will learn how to cache the code (store it compiled for faster processing).

The difference between PHP script processing versus C/C++
------------------------------------------

You may have learned to use the `C` or `C++` language in school. PHP is directly based on the syntax of `C` language, and inside the kernel the `C` language is used, so it is good to know some commonalities and conversely differences.

The basic principle of common compiled programs (which run locally directly on your computer or smartphone) is to load application logic into the operating memory, which communicates directly with the operating system, which receives input from the user and then displays the program's output. The important thing is that the program runs in isolation all the way from startup to termination.

PHP starts with each request to render a page, reloads all the code and data each time, and then exits. PHP scripts therefore have a literally yuppie lifetime and typically exist for only tens of milliseconds.

The advantage of this approach is a higher level of isolation - if anything goes wrong, everything is reloaded with the next page load. On the other hand, this approach has higher performance requirements because we have to repeatedly connect to the database, read files from disk, and so on, for example.

In the future, you will learn that you can keep PHP scripts loaded in the operating memory by using the `OP Cache` extension, which most new servers (from PHP 7.1 onwards) have set in the basic configuration.

Downloading foreign PHP scripts
--------------------------

A relatively common question we discuss with students is how to download foreign PHP scripts from the server and look at their source code. This question is preceded by the consideration that the HTML code of a page can be easily displayed in a web browser.

The answer is that **PHP scripts cannot be downloaded**. This is because the PHP code is first evaluated on the web server, and then the generated HTML code (or other output) is sent to the user's browser. Therefore, only the output of the PHP script can be downloaded, not the script itself.

How does the generation to HTML work?
---------------------------------

It doesn't literally work by surfing through HTML pages. The PHP script must always be in a PHP file.

Until now, most of you were used to creating huge folders full of files ending in the **.html** extension. Now it will be much fewer files. In the extreme case, it may be a single file.
