Sådan håndteres pludselige nedbrud af PHP-scripts
=================================================

> id: '41edbf1b-3ca5-487f-a54c-fac9926bc3d2'
> slug:
> 	cs: senior-7-sessions
> 	da: sadan-handteres-pludselige-nedbrud-af-php-scripts
> 
> publicationDate: '2023-02-11 14:30:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: bdd1e207ee9c0ba19c39f0b5c7d83c43

En historie fra slutningen af 2016, hvor jeg bogstaveligt talt blev reddet af en kollega: I en PHP-applikation beslutter du at tjekke billeder ind via et proxyscript, som bl.a. kan justere deres dimensioner og andre parametre i henhold til den indgående anmodning. Som en del af optimeringen gemmer du også de genererede varianter fysisk på disken.

Men i produktionsdrift begynder du pludselig at se en enorm belastning og tusindvis af forespørgsler i kø. Billederne indlæses i rækkefølge et efter et for hver bruger. Opdateringer af siden og klik på links virker ikke. Programmet virker helt fastfrosset. Det virker kun at vente på, at alting bliver behandlet.

Hvad kan problemet være? Jeg har anført 3 vigtige ledetråde i teksten for at gøre det muligt at søge hurtigt efter problemet. Hotfix har en triviel løsning.
