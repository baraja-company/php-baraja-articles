Internet Search Engine Algorithm - Sorting and Descriptor
=========================================================

> id: ca4aaac5-0cd8-41db-b860-c32661c43d82
> slug:
> 	cs: vyhledavac-trideni-a-popisovac
> 	en: internet-search-engine-algorithm---sorting-and-descriptor
> 
> publicationDate: '2016-09-11 13:00:00'
> mainCategoryId: '367f936c-073f-44bd-b399-30738e93137a'
> sourceContentHash: '541310dfdc96b75a320c7cad1b03e734'

In this lesson on the principles of an Internet search engine, we will understand how a search engine sorts, describes, and evaluates the results.

Sorting results
----------------

Let's imagine a finished barrel that is currently ready on the search server. Our first search query comes in from the user and now we need to do the first "rough" sorting, which will be further refined.

Let's have the following sample input query:

```
[About (and) dog (and) and (and) cat]
```

Yes, this is the form in which the search server gets the processed query from the user and now waits for the result to be returned. All in all we have less than `300 ms`, so quick :)

In the first step, we get barrels full of future result IDs (also called candidates) and operations to perform. This retrieved barrel may not be complete in some cases, but contains only the values that the server managed to read from disk in some predetermined time interval (also called **timeout**). For example, we get the following number series (which are partially sorted in advance):

| Barrels | Documents |
|-------|---------|
| o | 2,3,4,5,9,12,15,19,23,26,28,34,39,42,58,67,83,91,115 |
| dog | 5,19,23,42,46,58 |
| a | 2,3,4,5,9,12,15,19,23,26,28,34,39,42,58,67,83,91,115 |
| pussy | 9,19,42,57,58,62,68,83 |

We now perform a coarse intersection of all the sets and obtain a list of document IDs that are the same in all the barrels. In practice, we go through the shortest list and search the others.

The common IDs in all barrels are "19, 42, 58", so the future results page contains 3 items in some, as yet unknown, order. We still look at the documents as IDs because this saves a huge amount of data. In search results, however, we generally don't want to list document numbers to the user, but rather the document title (heading), description, URL, and other information. This is provided by the "descriptor" component.

Descriptor
---------

From the previous chapter we are left with a list of candidates for future results. We retrieved it in a sorted form in terms of the system, i.e., as the search engine sequentially ranked them in the index. At this point, we can no longer perform a sequential read of the big data, but must sequentially traverse some type of relational database to find additional information for another type of sorting that is more understandable to the user.

For example, a database slice might look like this:


| ID | Title | Label | URL | Rank |
|----|---------|---------|-----|------|
| 19 | Josef Čapek: A Tale of a Dog and a Cat | That was when the dog and the cat were still farming together; they had their little house by the forest and lived there together and wanted to do everything the way great people do it | www.troglodytarium.cz/webz/axf/.../Capek_J_Pejsek_a_kocicka.htm | 72 |
| 42 | Talking about the dog and the cat | "All right," said the dog, and the cat took soap and a pot of water, and knelt down on the floor, and took the dog as a brush, and scrubbed the whole floor with the dog. The floor was all wet | www.capek.narod.ru/povidani.htm | 86 |
| 58 | How the dog and the cat made a cake for the holiday| Tomorrow was the dog's holiday and the cat's birthday. The children knew this and wanted to surprise the dog and the cat for their birthday. They were thinking what they could do for the dog and | a.da.mek.sweb.cz/capek.j/dort.htm | 34 |

Calculation of relevance
-----------------

The last step is to calculate the relevance of the answer. For most results, it is sufficient to represent relevance as a single number with a fixed radius by which the results will be sorted. By rank, the results are again "roughly" sorted into several groups (the number of groups depends on the variance of the ranks) and these groups are further worked with.

The groups with the highest rank are taken first, often this is enough to sort the first page of results and we don't have to deal with anything else. However, if several different documents have the same rank, we need to do a more detailed analysis and calculate additional auxiliary ranks to help rank the page. The additional rank can be for example the quality of links, the age of the document, the length of the title, the "look and feel" of the URL and many other factors.

Returning results to the user
---------------------------

Hooray! We have the results and now all that's left is to render the page for the user. The results package is handed back to the server, which fits the results into a pre-prepared template, inserts advertising banners around the page and sends the page back to the user. At the same time, it can also perform other auxiliary operations, such as caching the results. If someone else searches for the same query at some point in the near future (for example, within the last hour), the results will not be searched again, but only read from temporary memory - which often helps improve overall search engine performance.

Conclusion and insight into semantics
---------------------------

This paper was intended to describe the basic principles of how search engines work internally and how they manage to retrieve a theoretically unlimited number of documents in a still reasonable amount of time. These are huge mathematical optimizations that have been developed over many years by the best programming teams.

A full description of the details is beyond the scope of this paper. The whole thing is also complicated by the fact that most of the other steps are not described in detail by any of the search engines, because they are their trade secrets.

It's also important to note that search engines still don't understand the content of pages and the meaning of results, and it's still just a statistical calculation based on raw computing power and people's ability to link to quality text, and this system is also quite vulnerable (take the "Google bomb" example, where a sneaky webmaster forces content on a search query that is irrelevant).

This problem should be partly solved by artificial intelligence, which all search engines are gradually trying to integrate. However, this is still distant music of the future, as it is not an easy task and mostly just a matter of improving statistical computation methods. Let Google graph search be an example, which tries to answer the query directly (although in its internal structure it still just searches to a predefined knowledge base).

So the next time you ask a query to your favorite search engine, remember how much work your search engine had to do and how many TB of data it had to read from disk to find the top 10 documents for your search query.
