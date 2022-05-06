Internet search engine algorithm - Trees and StopLead
=====================================================

> id: '11ec73f2-e505-4338-89aa-8307a55b7ba0'
> slug:
> 	cs: vyhledavac-stromy-a-stopslova
> 	en: internet-search-engine-algorithm---trees-and-stoplead
> 
> publicationDate: '2016-09-11 10:00:00'
> mainCategoryId: '367f936c-073f-44bd-b399-30738e93137a'
> sourceContentHash: d011d2d6943271ca5e45c3d3cdf03d61

There are 5 million new pages added to the internet every second, and this rate is increasing all the time. In order to give some order to this huge sea of information and to find something in it, there are search engines. The following work aims to introduce the issue of search and explain the whole process from the creation of a new page to finding it in a search engine.

The task of finding and sorting a set of billions of documents is not easy. Google alone needs 300,000 web servers to handle this task in just a few hours. In fact, the search for your query takes place long before you even ask it. Google already has the search results you will be asking for in the coming days stored in its memory.

Search engine architecture
------------------------

<img src="{$baseUrl}/images/fulltext-schema.png" alt="Common search engine architecture for up to 50 million documents" class="w-100 mb-3">

A properly designed search engine contains many components, of which there are on the order of several hundred. This diagram describes the most basic elements that a search engine must contain at a minimum in order to function. This is an old and no longer used architecture that worked until about 2005, when there were only a few million documents on the web. This architecture does not allow for an "infinite" amount of content and also for possible downtime of the search servers (base search). In the event of a single component being down, the whole system would cease to function and the search engine would be unavailable.

A complete description of current trends in the design of new architectures is beyond the scope of this paper, as these are usually trade secrets of large companies. The aim of this paper will be to explain how this basic architecture works. The individual components are explained in detail in the following chapters.

Query input
------------

The most visible component even for ordinary users is the query input, which is currently most often represented as a text box. The input field is located on a special page that is usually only for input.

In the next stage, the user enters the search string and submits the form to the search engine servers, thus initiating communication with the dispensing component, sometimes also called Main search. Dispensing servers are built to handle huge loads from users and must be able to handle all searchers at once.

The architecture of a dispatch server is usually a simple Apache server with a basic installation and excellent network bandwidth. When a query enters the server, it is processed through regression decision trees and a "query map" is created that captures the complete semantics around the other meanings to make it clear what the user is actually looking for. Query rewriting methods are too far beyond the scope of this paper, so we will just show general descriptions of what rewriting can and cannot look like.

Consider the following query:

```
"About a dog and a cat"
```

This query will be converted into a binary tree that captures its semantic essence:

<img src="{$baseUrl}/images/fulltext-tree.png" alt="Symbolic parse tree diagram" class="w-100 mb-3">

The whole point of a parse tree is to capture how a search engine will look at search results. This tree, along with the query, is sent out to the auxiliary search servers (labeled Base search in this paper), where individual keywords are searched for (index reads) and then sorted according to rules (by operations).

| Operator | Sorting operations |
|----------|------------------
| AND | Intersection |
| OR | Sum |
| NOT | Complement |

There is no actual sorting of search results, this component just performs fast binary operations on huge amounts of data (often millions of records) and basically just compares the IDs of individual documents.

How the actual sorting of search results for the results page works will be discussed in the following sections. The output server is simply used to combine a lot of data from different search servers to increase overall performance and spread the load, and then feeds the detected values into the results page (this is called the "web page"), containing composite information from dozens of servers.

Rewriting to a binary tree
-----------------------

As mentioned in the previous chapter, the entire search is built on binary trees that tell you what the results will look like and what to look for. The whole "logic" and "intelligence" of a search engine is directly dependent on the quality of the program that creates the tree - namely, the descriptor.

A properly constructed tree should contain all the necessary meta information about the query in such a form that it can be easily processed even with the help of sequential disk reading and should eliminate random accesses to memory, so it usually contains the individual search words (from the user), their transcriptions (and spellings) and, last but not least, the links between words, i.e. their relative distances in the text.

Query transcription methods
---------------------

The most basic way to transcribe a query is to parse it into individual words (according to the spaces), which will be worked with further. The second step is to search for StopWords and replace them with a variable pointer (because, as we will show later, there is no point in searching for StopWords at all).

Let's have the following query: "About a dog and a cat", its transcription in its most basic state looks like this:

```
"About (and) dog (and) and (and) cat"
```

The second step is to eliminate words that carry no search relevance. Of course, for example, the word "about" or "and" is meaningful to us humans, but not to a database search because it can be replaced with another word and the results listed. The goal should not be to find a literal match of the query against the index, because that would lead to bad results, because often words in text on the Internet are in different spellings, and this method tries to find results in other forms than the ones we got from the user. This is therefore the first indication of an "intelligent" search engine.

These meaningless words are often called "stopwords", every search engine should keep an index of such words and this index should be updated frequently (manually).

```
["dog (and) * (and) cat"]
```

At this point, we are looking for 2 different words ("to dog" and "to cat") that had an extra word in between (internally, we mark it with an asterisk).

The purpose of this modification is not to increase the quality of the search, but to increase performance. This is because when we search for a word that is too frequent, there is a rapid load, and this modification helps the process - especially by not searching for a word that will be available in that position in most documents anyway.

It is also important to note that we cannot always make this adjustment (omitting a word), so each search engine should keep a separate index of phrases that are found on the Internet to check which adjustment leads to good results and which does not. However, occasionally a search engine will make this adjustment even if it does not have the phrase in its index - especially when a user is searching for a significant word that is expected to have significant pages in the first positions, which override this "error" because users usually request them anyway.

The last step is to work with the English language and "clean" the query of unnecessary characters. For example, on the query "washing machine, " the user usually expects results only on the word "washing machine" and we can ignore the comma character.

The exact methods of how this system works are not known and each search engine has its own. It is believed that it is mostly just a statistical model that makes these adjustments based on knowledge of billions of texts in the database.

English transcription is also a science in itself, so only a basic description will be given here as well. In most cases, it is enough to add its hyphenated forms (according to the dictionary) to each word and search for them together with the word, thus achieving the effect that the search engine can find documents where the words do not appear only in their basic form (as entered by the user), but it can also find the inflected versions - a very useful feature. The problem with this approach is in the inflection of obscure and problematic words, and also in the inflection of queries that are short (and therefore it is not possible to determine from the context how to inflect the word). Let the word "games" be an example of a problematic inflection.

Given an English query, "I love her", a poorly designed inflectioner will inflect the word "games" as, for example, "games, gaming, gaming room, ...", so it is also necessary to index the surrounding words as phrases and perform inflection only in familiar situations, or use a different procedure (this is where phonetic algorithms often come in).

The last step is to send the finished tree to the Base search servers, where the actual barrel search will take place - this is the subject of the next chapter.
