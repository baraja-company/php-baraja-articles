Få en lista över alla inlästa filer
===================================

> id: '72716cbc-90a6-4870-848b-125e8430707f'
> slug:
> 	cs: ziskani-seznamu-vsech-nactenych-souboru
> 	sv: fa-en-lista-oever-alla-inlaesta-filer
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: f611e5d3-ed7b-4fe9-84ca-9271fc2bd2e3
> sourceContentHash: '7e896aa0e501d0fe33dc6595c1bfb43e'

När jag felsöker mer komplexa program händer det ibland att jag inte vet vilka filer som har laddats och om något saknas.

Om du använder Composer eller någon annan typ av <a href="/autoloading-trid">klassautoloading</a> känner du förmodligen inte till det här problemet. Det kan dock vara relativt vanligt när man felsöker äldre program från andra utvecklare.

Du kan hämta alla laddade filer med funktionen `get_included_files()`, som returnerar dem som en array av absoluta sökvägssträngar.

Vid utveckling är det vanligt att ett stort antal filer laddas (till exempel använder även denna relativt enkla blogg nästan 160 filer). För det mesta spelar dock den stora volymen ingen roll, eftersom innehållet i filerna hämtas från OPCache, som är optimerad för massläsning.
