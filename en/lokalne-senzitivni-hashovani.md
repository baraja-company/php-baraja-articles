Locally sensitive hashing
=========================

> id: '76e09294-08ea-4dde-a431-2116595d9f04'
> slug:
> 	cs: lokalne-senzitivni-hashovani
> 	en: locally-sensitive-hashing
> 
> publicationDate: '2022-07-11 10:45:00'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: c23def9586aa05d73c4f1cf1fd1dda43

The principle of most document fingerprinting hashing functions is that they always return the same output for every single input. This is called deterministic behavior. At the same time, a small change in the input will cause a large change in the output (effectively returning a completely different hash). This is especially useful when we want to check if the input document has changed, and if it has, we get something completely different. An example of such a function is MD5 or SHA1.

If we want to infer the contents of the original input from the hash, there is no easy way to do this and we have no choice but to brute force each option until we get a hit. This is because, due to the large change in the output, we cannot determine by iterations whether we are getting close to the target or not.

Some hashing functions such as BCrypt for the same input will return a different output each time to make them robust to password hashing. In fact, the output directly contains salt, which makes a so-called dictionary attack impossible. This feature is useful just for password hashing, but is very unsuitable for document review.

Document similarity checking and content duplication search
-----------------------------------------------------------

Imagine that we are solving an algorithm of a web search engine that wants to check how much a web page has changed. Alternatively, it wants to quickly check whether text from one page is similar to text on another page, or even completely duplicated.

One way to verify this is to compare each page with each other, which is very system resource intensive. Another option is to compute a SHA1 hash, but this hashes the contents of the entire document, and if the page changes by just one character, we get a different hash - so it is not suitable for these purposes.

Therefore, one possible solution is to compute the hash from different areas of the document differently and then look for changes in the output of the hashing function.

The principle of locale-sensitive hashing
----------------------------------

We have downloaded the HTML code of a web page for which we want to compute a hash. We divide the document into different regions (cells) by an algorithm that needs to be configured correctly.

We hash each region independently using a common algorithm and concatenate the result into a single string.

The output can then be, for example, `3791744029724361934` (this content hash was provided by the Ahrefs tool).

For example, if we know that the content of the header represents the first 5 characters and the footer represents the last 5 characters, we know that the middle of the string represents the content of the page. If the footer content changes on all pages (for example, the webmaster adds a new link, the update date changes, etc.), then when the new version of the page is downloaded, only a few characters in the hash in the right-hand area change slightly, and we know that only the footer has changed, but the content remains unchanged. So we can ignore such a change and don't have to go through the whole site.

How does Google optimize web crawling?
----------------------------------------

Internet bots need to save where they can. Web crawling is very expensive and updates cost computing time.

For example, in observing the behavior of Google's robot algorithm, I observed that it only responds to large changes in content. If the page changes little, it crawls in the normal way. But when, for example, the footer and header of a site change significantly, it evaluates it as a redesign and goes through most of the site at once to get the new look as soon as possible.

It also detects duplicates across sites in a similar way. When a group of pages are very similar or have the same content, they get a very similar hash, which the robot can use to quickly verify that the documents are similar and can select the canonical one and ignore the rest.

Implementation of the hashing function
-----------------------------

There is no ready-made implementation of the locale-sensitive hashing function in PHP. At the same time, I don't know of any freely available package that implements this function well.
