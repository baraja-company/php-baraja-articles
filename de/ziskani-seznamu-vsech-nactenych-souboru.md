Eine Liste aller geladenen Dateien erhalten
===========================================

> id: '72716cbc-90a6-4870-848b-125e8430707f'
> slug:
> 	cs: ziskani-seznamu-vsech-nactenych-souboru
> 	de: eine-liste-aller-geladenen-dateien-erhalten
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: f611e5d3-ed7b-4fe9-84ca-9271fc2bd2e3
> sourceContentHash: '7e896aa0e501d0fe33dc6595c1bfb43e'

Bei der Fehlersuche in komplexeren Anwendungen kommt es manchmal vor, dass ich nicht weiß, welche Dateien alle geladen wurden und ob etwas fehlt.

Wenn Sie Composer oder eine andere Art von <a href="/autoloading-trid">Klassen-Autoloading</a> verwenden, kennen Sie dieses Problem wahrscheinlich nicht. Es kann jedoch relativ häufig vorkommen, wenn ältere Anwendungen von anderen Entwicklern debuggt werden.

Alle geladenen Dateien können mit der Funktion `get_included_files()` abgerufen werden, die sie als Array absoluter Pfadzeichenfolgen zurückgibt.

Bei der Entwicklung ist es üblich, dass eine große Anzahl von Dateien geladen wird (selbst dieser relativ einfache Blog verwendet beispielsweise fast 160 Dateien). In den meisten Fällen spielt das große Volumen jedoch keine Rolle, da der Inhalt der Dateien aus dem OPCache abgerufen wird, der für Massenlesungen optimiert ist.
