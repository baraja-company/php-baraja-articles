Get a list of all loaded files
==============================

> id: '72716cbc-90a6-4870-848b-125e8430707f'
> slug:
> 	cs: ziskani-seznamu-vsech-nactenych-souboru
> 	en: get-a-list-of-all-loaded-files
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: f611e5d3-ed7b-4fe9-84ca-9271fc2bd2e3
> sourceContentHash: '7e896aa0e501d0fe33dc6595c1bfb43e'

When debugging more complex applications, it sometimes happens that I don't know what all files have been loaded and if something is missing.

If you use Composer or any other kind of <a href="/autoloading-trid">class autoloading</a>, you probably don't know this problem. However, it can be a relatively common occurrence when debugging older applications from other developers.

Getting all the loaded files can be done with the `get_included_files()` function, which returns them as an array of absolute path strings.

It is common in development to have a huge number of files loaded (for example, even this relatively simple blog uses almost 160 files). Most of the time, though, the large volume doesn't matter, because the contents of the files are retrieved from OPCache, which is optimized for bulk reads.
