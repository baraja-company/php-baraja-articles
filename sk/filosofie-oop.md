Základná filozofia objektovo orientovaného programovania
========================================================

> id: c38214d9-8c92-4484-9c31-239d716f2545
> slug:
> 	cs: filosofie-oop
> 	sk: zakladna-filozofia-objektovo-orientovaneho-programovania
> 
> publicationDate: '2020-01-02 22:21:56'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: a3841046b40a039413bacd045f275c68

Objektovo orientované programovanie je paradigma, pohľad na to, ako programovať. Čoskoro sami uvidíte, že OOP prináša dosť zásadné zjednodušenie všetkých bežných problémov a ťažkostí, ktoré sa v reálnom programovaní riešia stále dokola.

Základná myšlienka
-----------------

Základnou myšlienkou objektovo orientovaného programovania je rozdeliť veľkú aplikáciu (zložitú úlohu) na mnoho malých častí, ktoré môžeme riešiť elegantne a nezávisle.

Ak napríklad programujeme rezervačný systém pre letenky, ide o veľmi komplexný projekt, ktorý rieši tisíce prípadov. Ak dokážeme celú zložitú logiku rozložiť do mnohých vrstiev a častí, môžeme ľahko pochopiť celý zložitý systém a samostatne naprogramovať jednotlivé čiastkové úlohy.

Praktické výhody OOP a rozdelenia kódu na objekty
------------------------------------------------

Okrem akademického hľadiska existuje mnoho praktických dôvodov na používanie OOP:

- Aplikácia vytvorí <a href="/zapuzdrenie">zapuzdrenie</a>, čo znamená, že údaje sa vždy spracúvajú v lokálnom kontexte, ktorých je málo, takže nemusíme myslieť na celú aplikáciu naraz.
- Aplikáciu môžeme rozdeliť na stovky tisíc malých častí, ktoré môžu paralelne vyvíjať rôzni ľudia, a len zariadiť spoločné rozhranie.
- Na aplikáciu sa môžeme pozerať z pohľadu rôznych vrstiev (abstraktne na celé moduly alebo lokálne na konkrétne funkcie riešiace konkrétny algoritmus).
- Pri ladení aplikácie (debugging) môžeme aplikáciu krokovať, vynechať alebo nahradiť niektoré časti.
- Môžeme lepšie aplikovať **vzory návrhu**, a tak predchádzať chybám a zložitostiam v kóde skôr, ako sa vyskytnú.

Osobne si neviem predstaviť, že by tímy s viac ako jednou osobou programovali inak.

> **Poznámka:**
>
> Používanie objektov zaťažuje počítač o niečo viac pamäte a procesora, takže sa tým trochu zníži výkon aplikácie. V reálnom prostredí to však nevadí, pretože preprogramovaním aplikácie na objekty sa síce stratí určitý výkon (zvyčajne v jednotkách percent), ale programátorom sa ušetrí čas (zvyčajne desiatky až stovky percent). Ľudský čas je vždy oveľa drahší (a veľmi obmedzený) ako čas počítača.
>
> > Nezabúdajte tiež, že OOP prináša veľké zjednodušenie celej aplikácie a umožňuje dokončiť veľké aplikácie v rozumnom čase. Veľký počet zložitých aplikácií by bolo takmer nemožné naprogramovať bez objektov.

Vzťah k reálnemu svetu
-------------------------

Základným cieľom OOP pri návrhu softvéru je čo najviac simulovať vlastnosti, správanie a princípy reálneho sveta. Objekty v OOP predstavujú skutočné entity. Tento spôsob myslenia nám umožňuje vytvárať obrovské komplexné systémy, ktoré sa dajú dobre pochopiť, interne riešiť reálne problémy tak, ako by sa riešili bez počítača, a princípy sa dajú vysvetliť skutočným ľuďom.

Ak napríklad implementujeme aplikáciu na správu obsahu, má zmysel rozložiť všetku vnútornú logiku do mnohých reálnych entít (článok, autor, kategória) a relácie vytvárať nie podľa umelo vytvorených ID záznamov (ako sa to bežne robí v databázach), ale podľa reálnych vzťahov.

Príklad konkrétnej implementácie:

```php
class Article
{
    private Author $author;

    /** @var Category[] */
    private array $categories;

    private string $title;
}

class Author
{
    private string $name;
}

class Category
{
    private string $name;
}
```

Ako vidíme, trieda `Article` neobsahuje len technické parametre, ako je ID záznamu s autorom, ale je skutočnou väzbou na entitu Author, ktorá má opäť svoje vlastné vlastnosti.

Vysvetlenie konkrétnej implementácie a syntaxe nájdete v <a href="/uvod-do-oop">úvodnom tutoriáli</a>.
