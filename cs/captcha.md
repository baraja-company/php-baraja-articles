Jak funguje Captcha (opisovací obrázek)
================================

> id: 9cf191e4-a49b-407b-b16e-21de7224ac43
> slugCS: captcha
> perex: Captcha je druh obrázku, který ověří, že uživatel není robot a chrání webovou aplikaci. Možnosti implementace v PHP.
> publicationDate: 2019-08-22 20:48:46
> mainCategoryId: 1f73dcfa-92a9-4738-ab30-8cbfb00ad23b

Captcha je v současnosti jeden z nejčastějších způsobů ochrany volně dostupných formálářů. Původně nevznikla pro ochranu bezpečnosti dat, ale pro ochranu vůči spamu a rozpoznání, že se jedná o člověka.

Jedná se ale o strojově generovaný materiál, takže není vždy perfektní, úspěšnost captcha testu se však pohybuje někde kolem 99% a jen 1% obrázků dokáže dobře udělaný spamovací robot rozluštit.

Jak to funguje
--------------------------

Možností je více, třeba já používám toto řešení:

- Server přijme informaci, že si uživatel vyžádal stránku s formulářem a začne ji generovat.
- Do budoucího pole s captcha testem vloží obrázek s tajnou nic neříkající adresou (třeba náhodné číslo, řetězec znaků, ... cokoli).
- Vygeneruje se obrázek a uloží se na tuto adresu. Zároveň se někam zapíše jeho správný přepis (třeba do sessions nebo databáze).
- Až uživatel odešle formulář, tak se ověří, zda se přepis shoduje s tím, co zadal. Pokud ano, proběhne zpracování formuláře. Pokud ne, vyžádá se znovu-opsání jiného obrázku.
- Po úspěšném i neúspěšném pokusu se dočasný captcha obrázek smaže (nikdy už nebude znovu použit). V případě že nebude formulář odeslán do určitého času, tak se smaže také (vyprší platnost).

Správný přepis
--------------------------

Pokud se povede captcha test vyřešit, tak je uživatel pravděpodobně člověk. Je však ale dobré myslet na uživatele, kteří nemůžou tuto úlohu vyřešit, tím myslím zejména slepce. Je dobré řešení kombinovat více možných testů (zejména hlasové předabování). Nicméně rozpoznání hlasu strojem je v současné době výrazně efektivnější, než čtení z obrázku. Proto toto řešení není vždy ideální.

Vlastní jednoduchý captcha obrázek
--------------------------

Dost často stačí vygenerovat prázdný obrázek o určitých rozměrech a do něj čitelně vepsat několik znaků bez dalších úprav. Vážně! Většina spamovacích robotů je hloupá a na obecné formuláře s tímto typem ochrany zaútočit nedokážou i přesto, že se jedná o výborně čitelný text, který lepší OCR dokáže perfektně přepsat.

Výsledný obrázek může vypadat třeba i takto:

```
<img src="captcha.php" alt="ukázková captcha">
```

Vyzkoušejte si několikrát obnovit stránku a uvidíte, že se kód pokaždé náhodně změní. Pro potřebu demonstrace se jen generuje bez ukládání, takže se ihned po vašem načtení odstraní.

Zdrojový kód jsem vyřešil pomocí PHPGD knihovny, která je k dispozici prakticky na každé instalaci PHP a na každém hostingu:

```php
Header("Content-type: image/png"); 
$obr = ImageCreate(100, 35); 
$pozadi = ImageColorAllocate ($obr, 219, 28, 49); //definice barvy pozadí
$bila = ImageColorAllocate ($obr, 255, 255, 255); //definice bílé barvy pro text
$styl = array ($pozadi); 
ImageSetStyle ($obr, $styl); 
  $nahodne_cislo = rand(11111,99999); //losování náhodného čísla dlouhého 5 znaků
  imagestring($obr, 5, 25, 10, $nahodne_cislo, $bila); //funkce pro vykreslení textu (v tomto případě čísla)
ImagePNG($obr); //vygenerování obrázku do paměti a vykreslení
ImageDestroy($obr); //smazání obrázku z paměti (už nebude potřeba, protože je generován jednorázově)
```

Vykreslení obrázku je pak už jen otázka HTML:

```html
<img src="captcha.php">
```


Pozor, tento script je samostatně bez další úpravy nefunkční. Slouží jen jako demonstrace vygenerování jednoduchého obrázku.

Shrnutí
--------------------------

Captcha testy jsou poměrně spolehlivé, ale otravné a zdržují. Někdy nejsou dobře čitelné, tak je fajn dát uživateli možnost načíst jiný obrázek pomocí javascriptu, aby se nemusela znovu načítat celá stránka a vše se muselo znovu vyplňovat.

Pamatujte: To co je pro člověka triviální úkol, může být pro stroj nikdy nedosažitelné řešení. Proto i tato primitivní captcha s výborně čitelným textem může ochránit proti více jak polovině spamu.
