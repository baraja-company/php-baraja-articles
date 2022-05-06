Internet Search Engine Algorithm - Indexing and Canonicalization
================================================================

> id: daa97e66-e77f-4741-ade2-905c1cfdbd56
> slug:
> 	cs: vyhledavac-indexace-a-kanonizace
> 	en: internet-search-engine-algorithm---indexing-and-canonicalization
> 
> publicationDate: '2016-09-11 11:00:00'
> mainCategoryId: '367f936c-073f-44bd-b399-30738e93137a'
> sourceContentHash: d73a30f413e4b8b77dbceda60b3cf173

In today's lesson, we'll look at indexing and canonicalizing documents on the Internet.

Indexing
--------

The indexing process is performed by a component called the indexer. This is a specially designed program that makes the downloaded data (the data that the Crawler has downloaded) into a special data type for searching - barrels.

The problem with indexing is that you can't "smartly" browse the documents, but sequential reading (reading the entire text from start to finish) is inevitable, so it is a demanding discipline and search engines use the most powerful servers for this activity. No other task in the search process is as demanding as indexing, where plain text becomes indexes.

Take, for example, a page about cats, downloaded from Wikipedia. The indexer gets the complete text of the page and has to remove unnecessary things (e.g. user control menus, ads, footers, ...) and parse the page to get the plain text. For example, the text could be:

> The domestic cat (Felis silvestris f. catus) is a domesticated form of the wild cat that has been a companion to humans for thousands of years. Like its wild relative, it belongs to the small cat subfamily, and is a typical representative of the group. It has a lithe and muscular body, perfectly adapted to hunting, sharp claws and teeth, and excellent eyesight, hearing and sense of smell.

Excerpted from [Wikipedia](http://cs.wikipedia.org/wiki/Ko%C4%8Dka_dom%C3%A1c%C3%AD).

The search engine now parses out individual words from the text and writes them into individual barrels. However, it cannot do the barrel writing directly (mainly because each barrel exists in many copies and also because it would interfere with the search), so it creates a list of requirements for what the future barrel should look like and stores these in temporary memory. In our case, such a list might look something like this (some unimportant words can be ignored or marked as stopwords):

| Document ID | Word | Position | Type |
|--------------|-------|--------|-----------|
| 1 | Cat | 1 | Normal |
| 1 | Domestic| 2 | Normal |
| 1 | Felis | 3 | Normal |
| 1 | Is | 7 | StopWord |
| 1 | Domesticated| 8 | Normal |

Such a list is huge (much bigger than the original texts), but it is still faster to search because we don't have to read all the texts sequentially, but can search by index in each column and the words from one page are sorted in rows one after another.

After some time has passed (or some number of documents have been completed), the indexer stops working on building this list of requests (for a future barrel) and starts reading the data again and rebuilding the individual barrels (this list includes old records that are in an already working barrel). If new records are added from known addresses, this process will update them, while for new documents it will include them.

In this way, the indexer will go through the list again and slowly create new barrels that contain all the elements (they will look the same as shown in the example in the previous chapters) and when all the barrels are completed, they will be sent to the individual search servers. Updating the barrels on the servers is time consuming (mainly due to the huge amount of data being moved), so during this operation the servers are not available and the data is updated on different servers at different times. This results, for example, that some users may get different results because each user searches on a different dispensing server (due to load decomposition). Once the update is complete, everything is back to normal and all users find all documents equally.

The indexing process is important for every search engine, and the one that does it most often and most carefully is the one with the most up-to-date view of the Internet. Google performs this operation every few hours, Seznam once a week (and it has a million times less data).

Canonicalization of documents
--------------------

In the original design of the full-text search engine, there was no need for anything like canonicalization because the Internet was a medium that was constantly creating new content. Over time, however, duplication (i.e., the same content appearing at multiple different URLs) has occurred, and search engines must adapt to this. A typical example is Wikipedia, which has many articles. Some authors of other pages take over these texts (in part or even completely) and thus create duplication. In most cases, however, this does not matter, because the source page has a much higher rank (link quality) than the plagiarized one, but it can sometimes happen that it degrades the original at the expense of the pirate.

Search engines have had to adapt to this vice and the term "canonization" has been coined, which can be understood as the "removal" of certain pages from the index. Incidentally, this makes the indexes smaller, and the search engine does not have to crawl the same content all the time unnecessarily.

Duplicates are internally divided into 2 big categories by each search engine:

Natural duplicates
-------------------

These are created by the natural behaviour of the Internet and by its characteristics.

For example, the URL `http://mathematicator.cz` is likely to have the same content as the URL `http://www.mathematicator.cz` or `http://mathematicator.cz/index.php` because this is standard Apache server (and Internet in general) behavior.

If a natural duplication is found, the search engine creates a "canonical set", which is a group of pages from which the search engine selects one representative that stands out in the search. If a link leads to any page from the canonical set, its rank will be automatically passed to the main representative.

It is often a good idea to help the search engine create this set and set up the redirect correctly on the site, which will cause the search engine to look at the site better and the main representative will be selected better.

Duplicates leading to plagiarism
----------------------------

Plagiarism is a problem on the internet today. The existence of plagiarism per se would not matter that much, however, users are particularly bothered by it because they keep finding the same results for the same query. Have you also ever found several pages with identical text for a query? This is exactly the behaviour that search engines are trying to prevent.

The biggest problem is determining which page is the original source - and doing it by machine. Again, the search engines put all similar pages into a canonical set and select a principal representative from that set. If the sources are from different domains, then the situation cannot be looked at as a natural duplication (and any candidate chosen), but all pages must be evaluated qualitatively and the best one selected objectively - and ideally the original source of the content.

Search engines often make decisions based on the rank of the entire domain and the strength of the link network to a given document, but even this is a rather unreliable approach. The second factor is also usually the time of creation (indexation) of the document. Therefore, each search engine keeps track of which pages are frequently used to create new content and visits these pages frequently, so that the new page is ideally noticed immediately and therefore not chosen as a proxy.

A detailed description of the methods of how this selection works is beyond the scope of this paper and could be the subject of an entire book.
