File_get_contents
=================

> id: '6c8889f1-95e7-4540-9c3a-0225c6383954'
> slug:
> 	cs: file-get-contents
> 	sk: file-get-contents
> 
> publicationDate: '2019-09-11 10:18:03'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: f8b180a5f7208dab0d37761a5acdc9e3

Funkcia **file_get_contents** slúži na načítanie súboru a vloženie jeho obsahu do premennej. Táto funkcia je podobná funkcii <a href="/include">include</a>, ale na rozdiel od include dokáže načítať vzdialené súbory na internete a prenášať ich obsah prostredníctvom premenných.

Vzorka
------

Na načítanie lokálneho súboru z disku možno použiť ktorúkoľvek z týchto funkcií:

```php
$news = file_get_contents('news.html');

echo 'Najnovšie správy:<br>' . $news;
```

Alebo zo vzdialenej adresy URL:

```php
$page = file_get_contents('https://www.google.com');

echo $page;
```

Pri načítavaní adresy URL je možné prevziať akúkoľvek adresu a jej obsah načítať ako reťazec do premennej. V prípade HTML je to zdrojový kód.

Stránka sa vykresľuje nesprávne
----------------------------

Je to preto, že kód HTML sa odovzdáva presne tak, ako je umiestnený na adrese URL.

Ak je cesta k obrázku napríklad `<img src="kocka.png">`, potom tento súbor nemusí v kontexte nášho servera existovať, takže musíme opraviť cestu napríklad na: `<img src="https://server.cz/kocka.png">`.
