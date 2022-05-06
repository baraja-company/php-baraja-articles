Overview of web development knowledge
=====================================

> id: e5e9613c-66fe-4d5e-a686-a182aab8061a
> slug:
> 	cs: znalosti
> 	en: overview-of-web-development-knowledge
> 
> publicationDate: '2019-09-11 10:07:07'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: b7284b1fc11b40ddb7628995070198a1

Knowing how to create a website and then take comprehensive care of it is not just a matter of making it. There are many hurdles along the way and it's good to have at least a basic idea of each thing. When I started out, I didn't really know what all there was to learn. This page serves as a signpost through the topics I had to study gradually to be able to understand web development and deal with most common situations.

Server administration
--------------

> A web server is a computer that runs the web. When a user views a page, the web server sends the page to the user.
>
> At the moment (2021), it no longer makes sense to get free hosting if you are serious about the web. A server can be **rented from 50 crowns per month**.

- Server installation (differs on Windows and Linux)
- Server configuration
	- Apache settings
	- PHP setup
	- <a href="/info">getting the current PHP configuration</a> (function `phpinfo()`)
	- Ngnix routing (improves server performance)

Internet and web browser
--------------------------------

- Web browsers
- Request & response principle
	- URL address
	- Ajax and Ajaj
	- HTML code generation (templating systems)

Strings
-----------------

- Reading, writing and concatenating strings, especially basic string work
- Processing strings
	- Browsing character by character
	- <a href="/if">comparing strings</a>
	- String similarity (functions `levenshtein()`, `similar_text()` and `soundex()`)
	- <a href="/explode">Explode</a>, splitting by separator
	- **Regular expressions** split strings according to a universally configurable mask
	- **Tokenizer** breaks strings into small parts (tokens)
- Serialization of data into a string
	- <a href="/json">Json</a>, a javascript object stored in a string
