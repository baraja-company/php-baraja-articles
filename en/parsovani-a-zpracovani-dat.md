Parsing and data processing in PHP
==================================

> id: '7dfc991d-a7ee-4e2b-8159-16cef1d27483'
> slug:
> 	cs: parsovani-a-zpracovani-dat
> 	en: parsing-and-data-processing-in-php
> 
> publicationDate: '2017-10-15 09:54:02'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '33947488a0005f2977e6283c532a08d7'

> This article will discuss methods for processing data in PHP, but it is not finished yet.

So temporarily just a quick sketch of the possibilities:

- **Crawl character by character** is a very old method that makes the code messy, but all other methods do it internally.
- <a href="/explode">Explode</a>, splitting a string by a delimiter
- <a href="/regex">**Regular expressions**</a> are the best way to handle simple strings
- **Tokenizer**, splits a complex string into pieces (tokens) according to regular expressions, for example this is how PHP is processed
