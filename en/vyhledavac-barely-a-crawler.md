Internet search engine algorithm - Barely a crawler
===================================================

> id: '5ad042bf-7fda-4b2e-a38c-762169d0b594'
> slug:
> 	cs: vyhledavac-barely-a-crawler
> 	en: internet-search-engine-algorithm---barely-a-crawler
> 
> publicationDate: '2016-09-11 12:00:00'
> mainCategoryId: '367f936c-073f-44bd-b399-30738e93137a'
> sourceContentHash: '5daf3854688c39f9e0071a93ab11dd4e'

In today's lesson, we will discuss data barrels, their structure, StopSlovas and finally we will describe crawlers.

Data Barrels
-------------

This is a special data type that resides on multiple servers simultaneously in multiple copies. These are usually data-intensive files of hundreds of GB and are slow to read (which is why they are split into parts) and virtually impossible to edit. If we want to make even a minimal change, we have to recompute the whole barrel. For example, the search engine Seznam can recalculate data barrels at most once every few days or weeks, while Google recalculates once every few hours (and only some parts, never the whole thing at once).

Barrels contain words and their occurrences on the Internet. It can be said that this is raw data for search results in a not yet sorted form (sometimes they are partially sorted by relative importance) and prepared in advance. The search itself takes place long before the user asks the search query and it is here that all the results are prepared. The search itself would be impossible in real time, so the search engine basically reads the results already prepared here (the search is done by the indexer component, which will be discussed in a separate chapter).

The barrels contain all the basic information needed to build search results for every word that occurs on the Internet, so performance issues must be addressed when reading sequentially. In practice, barrels tend to be stored in batches on multiple servers and also in RAM for faster access. For example, the Google search engine does not read the barrels from disk at all, but keeps them completely in RAM and only keeps a copy of them on disk in case data is lost from RAM.

Large search engines with sufficient resources also keep so-called "golden barrels", which contain the most important pages on the Internet and are searched in case of a heavy search load. For example, if several search servers crash, the gold barrels temporarily cater for searches. The search engine may not find all the results, but at least the search will not be completely interrupted, but only the number of searched documents will be temporarily limited. It is also necessary to update the barrels regularly, as the number of pages on the Internet is constantly increasing. Since the barrels are usually in the order of gigabytes in size, it is not possible to perform incremental maintenance, but it is necessary to rebuild the whole thing once every set time. Therefore, the search engine keeps an auxiliary barrel where all the data is in the form of a large table from which the barrels are built - for example, Google builds the barrels approximately every 12 hours. The big challenge is precisely that the barrels must be at least partially sorted and reflect the actual state of the Internet. So until the search engine updates the barrels, the user will not find new web pages.

Barrel structure
----------------

In practice, a barrel is stored as a large text file that contains the ID of each document where the search word occurs and the position of the occurrence of the currently searched word.

An example of a very small barrel with three pages:

```
[1:5,8,12,27,30;5:1,3,6;12:4,8,10]
```

This sample barrel contains pages with identifiers `1`, `5` and `12`. The associated numbers indicate the position of the word occurrence in the document. Such a barrel only captures occurrences of a single word, so each word on the Internet has its own barrel. When searching for multiple keywords, it is necessary to read multiple barrels (a different one for each word) and then perform the operations according to the binary tree (more in the previous chapters).

The intersection is usually done by going through one of the documents and searching the other. If the barrels are large, they may not be read completely, so search engines often only manage to read part of them and then have to return the result, so it makes sense to sort the barrels at least partially, so that the most important pages (which the user is likely to be looking for) are at the beginning of the barrel and everything else is at the end of the barrel.

StopLead
---------

You might be thinking that for some words the barrels could be really huge and therefore difficult to work with. For example, the word `a` or the word `1` is found in more than half of the documents, and yet they carry no meaning to the searcher (because they are rarely searched for and require more surrounding words). Such words are called Stopwords.

The problem with StopWords is that they cannot all be searched for, and search engines therefore only show a distorted reality of what the Internet actually looks like for the search word.

An example of a search with StopSlovy is the query `Prague 1`.

The search engine gives only `3,020,000 results` for this query. In reality, however, there are many more such sites. However, not all of them are searched for performance reasons.

Search access options with StopSlovy:

- Not indexing them at all and not making barrels for them (small search engines do this)
- Indexing them, but only storing relevant pages in barrels (this is done by Google and Seznam, for example)
- Index them as regular words and create complete barrels (this solution has the problem that barrels can easily exceed the size of a regular hard drive and therefore cannot be saved and can take seconds to read - this solution is therefore not used in practice or only for small and specialized search engines)

In general, there is no universally correct approach and even Google does not use it perfectly. This is such a large problem that it could take a lifetime to solve. For the purposes of this paper, however, this general description is sufficient.

Crawler (robot, also spider)
---------------------------

A crawler is a program that systematically crawls the Internet and keeps track of what words appear on what pages and also collects auxiliary information (also called meta information). A crawler usually runs on many servers, because crawling the Internet is demanding in terms of network bandwidth and therefore many pages must be crawled simultaneously. Google estimates that up to 30,000 pages are crawled simultaneously at any one time.

Before opening a page, Crawler looks at a database of what addresses it plans to go to in the future. If it has already been to the address, it will only update its records (it goes to major sites every few hours, to news sites every few minutes, to regular sites once a month at most).

If the robot arrives at a new address that it does not know, it will look at the contained links to other documents (pages). For each link, it calculates its importance, and if it is an important link, it also comes to the page later.

For example, List only crawls a few levels of links on each site in this way, while Google tries to crawl the entire site, including unimportant documents, making it the most comprehensive search engine on the Internet.
The robot also tries to rank the page as it crawls and calculates its "rank", which is a numerical representation of how important the page is on the Internet. In the past, Google has used PageRank, which has a range from 0 to 10. Google only calculates PageRank 10 for itself and sites such as microsoft.com or w3c.org. Nowadays it calculates the value differently (artificial intelligence and vector mathematics).

The highest PageRank in the Czech Republic is Seznam.cz with a value of 7. The rank has a high influence on how often a robot will visit the page and how high it will be in the search results. Ranks are calculated just from backlinks pointing to the desired page.

The crawler does nothing else with the data, it just downloads it from the internet and stores it in a database where it awaits the indexing process.
