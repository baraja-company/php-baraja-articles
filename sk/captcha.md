Ako funguje Captcha (popisný obrázok)
=====================================

> id: '9cf191e4-a49b-407b-b16e-21de7224ac43'
> slug:
> 	cs: captcha
> 	sk: ako-funguje-captcha-popisny-obrazok
> 
> perex: 'Captcha je druh obrázku, který ověří, že uživatel není robot a chrání webovou aplikaci. Možnosti implementace v PHP.'
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: '80357b2e7f0047bb488922f2ab7b4336'

Captcha je v súčasnosti jedným z najbežnejších spôsobov ochrany bezplatných formátov. Pôvodne nebol vytvorený na ochranu bezpečnosti údajov, ale na ochranu pred spamom a na rozpoznanie, že ide o človeka.

Je však generovaná strojom, takže nie je vždy dokonalá, ale úspešnosť testu captcha sa pohybuje okolo 99 % a iba 1 % obrázkov dokáže dobre vytvorený spamový robot rozlúštiť.

Ako to funguje
--------------------------

Existuje viac možností, ja napríklad používam toto riešenie:

- Server dostane informáciu, že používateľ požiadal o stránku s formulárom, a začne ju generovať.
- Do budúceho testovacieho poľa captcha vloží obrázok s tajnou adresou, ktorá nič nehovorí (napríklad náhodné číslo, reťazec znakov, ... čokoľvek).
- Obrázok sa vygeneruje a uloží na túto adresu. Zároveň je niekde (možno v relácii alebo databáze) zapísaný správny prepis.
- Keď používateľ odošle formulár, skontroluje sa, či sa prepis zhoduje s tým, čo zadal. Ak áno, formulár sa spracuje. Ak nie, požaduje sa opätovný opis iného obrázka.
- Po úspešných aj neúspešných pokusoch sa dočasný obrázok captcha odstráni (už sa nikdy nepoužije). Ak formulár nebude odoslaný do určitého času, bude tiež vymazaný (vyprší jeho platnosť).

Správny prepis
--------------------------

Ak sa dá test captcha vyriešiť, používateľ je pravdepodobne človek. Je však dobré myslieť aj na používateľov, ktorí úlohu nedokážu vyriešiť, najmä na nevidiacich používateľov. Je to dobré riešenie na kombináciu viacerých možných testov (najmä hlasového prefetchingu). Rozpoznávanie hlasu strojom je však v súčasnosti podstatne efektívnejšie ako čítanie z obrázku. Preto toto riešenie nie je vždy ideálne.

Vlastný jednoduchý obrázok captcha
--------------------------

Často stačí vygenerovať prázdny obrázok určitých rozmerov a čitateľne doň zadať niekoľko znakov bez ďalších úprav. Vážne! Väčšina spamových robotov je hlúpa a nedokáže napadnúť všeobecné formuláre s týmto typom ochrany, hoci ide o dokonale čitateľný text, ktorý dokáže lepšie OCR dokonale prepísať.

Výsledný obrázok môže vyzerať takto:

<img src="captcha.php" alt="ukázková captcha">

Skúste stránku niekoľkokrát obnoviť a uvidíte, že kód sa zakaždým náhodne zmení. Na demonštračné účely sa len vygeneruje bez uloženia, takže sa odstráni ihneď po načítaní.

Zdrojový kód som vyriešil pomocou knižnice PHPGD, ktorá je k dispozícii prakticky na každej inštalácii PHP a hostingu:

```php
Header("Content-type: image/png");
$obr = ImageCreate(100, 35);
$pozadi = ImageColorAllocate ($obr, 219, 28, 49); //definícia farby pozadia
$bila = ImageColorAllocate ($obr, 255, 255, 255); //definícia bielej farby pre text
$styl = array ($pozadi);
ImageSetStyle ($obr, $styl);
  $nahodne_cislo = rand(11111,99999); //vylosovanie náhodného čísla s dĺžkou 5 znakov
  imagestring($obr, 5, 25, 10, $nahodne_cislo, $bila); //funkcia na kreslenie textu (v tomto prípade čísla)
ImagePNG($obr); //vygenerovanie obrazu do pamäte a vykreslenie
ImageDestroy($obr); //odstrániť obrázok z pamäte (už nebude potrebný, pretože je vygenerovaný raz)
```

Vykreslenie obrázka je potom už len otázkou HTML:

<img src="captcha.php">

Upozorňujeme, že tento skript nie je funkčný sám o sebe bez ďalších úprav. Slúži len ako ukážka na vytvorenie jednoduchého obrázka.

Zhrnutie
--------------------------

Captcha testy sú pomerne spoľahlivé, ale otravné a časovo náročné. Niekedy nie sú čitateľné, preto je dobré dať používateľovi možnosť načítať iný obrázok pomocou javascriptu, aby sa nemusela načítať celá stránka a všetko sa muselo vyplniť znova.

Nezabudnite: To, čo je pre človeka triviálna úloha, nemusí byť pre stroj nikdy dosiahnuteľné riešenie. Preto aj tento primitívny captcha s dokonale čitateľným textom dokáže ochrániť pred viac ako polovicou spamu.
