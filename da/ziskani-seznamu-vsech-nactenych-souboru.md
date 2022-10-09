Få en liste over alle indlæste filer
====================================

> id: '72716cbc-90a6-4870-848b-125e8430707f'
> slug:
> 	cs: ziskani-seznamu-vsech-nactenych-souboru
> 	da: fa-en-liste-over-alle-indlaeste-filer
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: f611e5d3-ed7b-4fe9-84ca-9271fc2bd2e3
> sourceContentHash: '7e896aa0e501d0fe33dc6595c1bfb43e'

Når jeg debugger mere komplekse programmer, sker det nogle gange, at jeg ikke ved, hvilke filer der er blevet indlæst, og om der mangler noget.

Hvis du bruger Composer eller en anden form for <a href="/autoloading-trid">klasses autoloading</a>, kender du sikkert ikke til dette problem. Det kan dog være en forholdsvis almindelig forekomst, når man debugger ældre programmer fra andre udviklere.

Du kan få fat i alle de indlæste filer med funktionen `get_included_files()`, som returnerer dem som et array af absolutte stistrenge.

Det er almindeligt i udviklingsprocessen at have et stort antal filer indlæst (f.eks. bruger selv denne relativt simple blog næsten 160 filer). Det meste af tiden er den store volumen dog ligegyldig, fordi filindholdet hentes fra OPCache, som er optimeret til masseindlæsning.
