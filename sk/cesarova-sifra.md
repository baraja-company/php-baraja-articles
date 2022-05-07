Cézarova šifra - ako funguje
============================

> id: '662dd627-c106-406e-ad6a-7ae9a492ad92'
> slug:
> 	cs: cesarova-sifra
> 	sk: cezarova-sifra---ako-funguje
> 
> publicationDate: '2019-08-23 15:43:02'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '892ad0746be469b5cdbb4de72d8e85b9'

> **Upozornenie:** Tento článok bol napísaný pred mnohými rokmi a niektoré informácie môžu byť zastarané alebo nesprávne. Majte to na pamäti pri čítaní.

Cézarova šifra je jednou z najjednoduchších hašovacích funkcií. Vo svojej dobe bol prakticky nerozbitný, ale v dobe moderných počítačov stačí na jeho prelomenie niekoľko desiatok sekúnd, dokonca niekoľko minút. Je založený na kľúči, podľa ktorého sa správa zašifruje a podľa ktorého sa môže opäť rozšíriť. Kľúč je preto tajný. V čase šifrovania je správa viditeľná a nič neznamená (len spleť znakov). Jediný spôsob, ako prelomiť šifru, je uhádnuť kľúč.

Kľúč
--------------------------

Kľúčom môže byť akékoľvek celé číslo, ktoré má menej číslic ako samotná správa. Zvyčajne sa uvádzajú 3 platné číslice (takže existuje 999 kombinácií). Každá ďalšia číslica zvyšuje bezpečnosť. Aby mohli dve strany komunikovať, obe strany musia poznať tento tajný kľúč (takže si ho musia nejakým spôsobom bezpečne odovzdať). Ak je kľúč známy im a nie druhej strane, potom sa správa môže šíriť aj nezabezpečene a potenciálne neohrozí obsah, pretože potenciálny útočník nepozná postup, ako správu získať späť.

Na demonštračné účely použijem kľúč **123**, zvyčajne sa používa nejaké iné náhodné číslo, o ktorom sa predpokladá, že ho nie je také ľahké uhádnuť.

Postup šifrovania
--------------------------

Princíp je založený na myšlienke nahradenia znakov správy inými pomocou kľúča. Ja tomu hovorím "posun charakteru".

Majme napríklad túto správu, ktorú chceme zašifrovať:

```php
TAJNA ZPRAVA
```

Teraz priraďte každému znaku číslo. Zvyčajne podľa abecedy (jeho sériového čísla). Budem používať anglickú abecedu, takže tento rad znakov:

```php
A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
```

Ak každému znaku priradíme číslo, dostaneme niečo podobné:

```php
20-1-10-14-1   26-16-18-1-22-1
```

Teraz prichádza kľúč. Vezmeme každé jednotlivé číslo a pridáme k nemu kľúč. Farebne zvýrazňujem, čo som kde pridal:

`1 2 3`

```php
21 3 13 15 3   3 17 20 4 23 3
```

Všimnite si, že keď šifrujem znak **Z**, napíšem číslicu 3. Je to preto, že **Z** je posledný znak abecedy, takže keď dosiahne koniec, počíta sa znova od začiatku riadku.

Odoslanie správy
--------------------------

Teraz môžeme správu odovzdať ľubovoľným spôsobom. Niekedy dokonca verejne. Ostatní uvidia len nelogickú sériu čísel, takže pravdepodobne ani nebudú vedieť, že ide o nejakú šifru. Bezpečnosť zvyšuje aj samotný kľúč, ktorý je tajný. Niekedy je vhodné preniesť správu ako sériu čísel, inokedy je vhodné previesť tieto čísla na znaky (opäť podľa rovnakého abecedného radu) a potom preniesť sekvenciu znakov. Veľa závisí od okolností. Vo všeobecnosti je však číselná sekvencia lepšia, pretože len málo ľudí bude mať podozrenie, že ide o kódovanú správu.

Dešifrovanie u príjemcu
--------------------------

Príjemca dešifruje rovnakým postupom. Vezme každý jednotlivý znak a odčíta čísla podľa kľúča a potom prevedie výsledné hodnoty späť na znaky pomocou abecedy. Je to len opačný postup šifrovania. Dôležité je poznať **kľúč** a **sadu znakov** - teda to, ako idú znaky za sebou.

Prelomenie a dešifrovanie
--------------------------

Jediný možný postup prelomenia je vyskúšať všetky možné kombinácie všetkých možných kľúčov. Ak nepoznáme dĺžku kľúča, celý proces sa ešte viac skomplikuje. Vo všeobecnosti však dnešné počítače dokážu vyskúšať približne 100 kľúčov za 1 sekundu, takže prelomenie 3-znakového náhodného kľúča trvá približne minútu.

Ak je však kľúč rovnako dlhý alebo dlhší ako pôvodná správa, nie je možné ho prelomiť, pretože každý jednotlivý znak má svoj vlastný kľúč, takže sa musia vyskúšať všetky kombinácie pre každý znak.

Ak by som mal správu "**POKRAČOVAŤ V SPRÁVE**", mala by 11 znakov (medzery sa nepočítajú). Ak by som chcel kľúč dlhý tiež 11 znakov a použil by som 26 znakový reťazec anglickej abecedy, existuje **11^26** = 1,191817654*10²⁷ kombinácií, priemerný počítač by tento kľúč prelomil za 1,310999419×10²⁶ sekúnd = 10^20 dní :)
