Odsazení kódu pomocí mezer a tabulátorů
=======================================

> id: "116f19ed-3753-498d-bb9e-e0f93b88c347"
> slugCS: mezery-a-tabulatory
> publicationDate: "2019-08-22 20:48:46"
> mainCategoryId: "0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1"

Abychom mohli udržet kód snadno čitelný i pro další programátory a zachovali jej elegantní, musíme se naučit jednotně formátovat. Tento článek pojednává o použití mezer a tabulátorů.

Jsou pro odsazování kódu lepší mezery nebo tabulátory? To je často nekonečné téma k diskusi, pokud hledáte rychlou a jednoznačnou odpověď, tak většina dobrých programátorů raději používá tabulátory, ale pojďme si to hezky rozebrat.

Mezery
----------------------

Každý programátor a editor používá jiné množství mezer pro odsazení (nejčastěji však 4), což vede k tomu, že při čtení kódu po někom jiném nemáme konzistentní kód, který se může hůře číst. Navíc k odsazení je potřeba větší počet znaků (což zvyšuje jeho datovou velikost).

Mezery mají ovšem výhodu při vykreslování kódu ve webovém prohlížeči (kde se pro odsazení používá HTML entita `&nbsp;`), proto jde o poměrně snadno přenositelný formát, který získává výhodu pouze jako stabilní a spolehlivý způsob vykreslení (4 mezery se vždy zobrazí jako 4 mezery).

Tabulátory
----------------------

Mají takovou šířku, jakou si programátor v editoru nastaví (pokud to editor umí), proto pokud máte rádi nějaké konkrétní odsazení, není problém - každý se můžeme na ten samý kód dívat s jinou šířkou tabulátorů. Zároveň jde o velice úsporný znak, který není nutné opakovat tak často, jako právě mezery.

Při vykreslování kódu odsazeného tabulátory do HTML stránky je zvykem tabulátory nahradit za pevné mezery, aby bylo zajištěno korektní zobrazení ve všech prohlížečích:


```php
$code = '<?php
	$a = 5+3;
	$b = 4;
	if ($a > $b) {
		echo $a . " > " . $b;
	} else {
		echo $b . " <= " . $a;
	}
?>';

echo str_replace("\t", '&nbsp;', $code);
```
