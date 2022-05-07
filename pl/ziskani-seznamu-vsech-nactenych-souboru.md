Uzyskaj listę wszystkich załadowanych plików
============================================

> id: '72716cbc-90a6-4870-848b-125e8430707f'
> slug:
> 	cs: ziskani-seznamu-vsech-nactenych-souboru
> 	pl: uzyskaj-liste-wszystkich-zaladowanych-plikow
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: f611e5d3-ed7b-4fe9-84ca-9271fc2bd2e3
> sourceContentHash: '7e896aa0e501d0fe33dc6595c1bfb43e'

Podczas debugowania bardziej złożonych aplikacji czasami zdarza się, że nie wiem, jakie wszystkie pliki zostały wczytane i czy czegoś nie brakuje.

Jeśli używasz Composera lub innego rodzaju <a href="/autoloading-trid">autoloading klas</a>, prawdopodobnie nie znasz tego problemu. Jednak może to być stosunkowo częste zjawisko podczas debugowania starszych aplikacji innych programistów.

Wszystkie załadowane pliki można pobrać za pomocą funkcji `get_included_files()`, która zwraca je jako tablicę łańcuchów ścieżek bezwzględnych.

W programowaniu często zdarza się, że wczytywana jest ogromna liczba plików (na przykład nawet ten stosunkowo prosty blog wykorzystuje prawie 160 plików). W większości przypadków jednak duża objętość nie ma znaczenia, ponieważ zawartość pliku jest pobierana z pamięci podręcznej OPCache, która jest zoptymalizowana pod kątem masowych odczytów.
