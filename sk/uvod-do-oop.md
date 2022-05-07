Úvod do objektovo orientovaného programovania v PHP
===================================================

> id: '44a79461-dcfd-4dd0-b6fd-ba5db76db6de'
> slug:
> 	cs: uvod-do-oop
> 	sk: uvod-do-objektovo-orientovaneho-programovania-v-php
> 
> publicationDate: '2020-01-01 19:54:35'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '888db2cf35845892eebade9fdcc18871'

Vitajte v prvom článku online kurzu OOP v PHP. Kompletný zoznam článkov nájdete na <a href="/oop">prehľadovej stránke</a>.

> **Poznámky k obsahu:**
>
> Cieľom tohto seriálu je **najlepšie vysvetliť podstatu** objektovo orientovaného programovania, aby ste nemuseli stráviť stovky hodín experimentovaním s vecami, ktoré nemajú zmysel. Všetky techniky a návody sú z praxe a sú vysvetlené tak, ako by som ich chcel čítať ja sám, keď som sa učil programovať. Niektoré **koncepty sú značne zjednodušené** na úkor 100 % vecnej správnosti, pretože si myslím, že je vždy **lepší pochopiť hlavne princípy** a mať aspoň nejakú predstavu o problematike, ako sa stratiť v programovaní a vidieť všetko ako jeden veľký zmätok.
>
> Ako čoskoro uvidíte, programovanie v OOP má pre váš kód obrovské výhody. Prináša novú úroveň abstrakcie nad vašou aplikáciou a umožňuje vám riešiť veľmi zložité problémy, ktoré by nebolo možné riešiť iným spôsobom.

Čo sú to objekty
------------------

V základnej koncepcii programovania sú objekty špeciálne dátové typy, ktoré okrem vlastnej hodnoty môžu definovať podrobný stav, metódy, správanie a umožňujú vykonávať rôzne operácie, ako v reálnom svete.

Objekt v programovaní je niečo ako "vec" z reálneho sveta, ktorú môžeme pomenovať (napr. podstatným menom) a má vlastnosti ako vec z reálneho sveta. Objektom môže byť napríklad článok, ktorý má určité hodnoty (názov, obsah, autor, dátum vydania, ...) a metódy (vloženie nového názvu, prečítanie obsahu, výpočet počtu dní od vydania, ...). V tomto článku sa budeme venovať najmä tomu, ako vytvoriť objekt a pracovať s ním na základnej úrovni.

Objekty a triedy
---------------

Ako sme už zistili, **objekt je niečo, čo existuje**. Slovo `existuje` je v tejto fáze veľmi dôležité, pretože, ako čoskoro uvidíme, v programovaní sa ešte musíme zaoberať praktickou implementáciou.

Keďže v programovaní píšeme kód a objekt je niečo ´živé´, budeme od tohto bodu rozlišovať dva rôzne pojmy, pri ktorých je dôležité podrobne pochopiť význam a striktne to rozlišovať:

- **Objekt** je niečo "živé", čo práve teraz existuje (má inštanciu) a môže niečo vykonať alebo reagovať na iné objekty.
- Trieda** je staticky napísaný kód, ktorý ešte nebol vyhodnotený a zostáva stále rovnaký.

Keďže pri programovaní vždy zapisujeme kód ako statický reťazec (text), budeme programy rozlišovať na `triedy` (statická definícia celého objektu) a `objekty` (`živá` inštancia triedy).

Príklad definície triedy:

```php
class Article
{
    public string $title;

    public ?string $autor = null;
}
```

> **Praktické poznámky:**
>
> V skutočnej aplikácii sa každá trieda zvyčajne zapisuje do samostatného súboru PHP, ktorý je pomenovaný rovnako ako názov triedy.
>
> Takže napríklad pre triedu `Article` vytvoríme `src/Article.php`. Táto konvencia sa v PHP nevyžaduje, ale pomáha urobiť aplikáciu prehľadnejšou, aby ste nemali veľa kódu na jednom mieste.
>
> Každá trieda môže v celej aplikácii existovať najviac raz (má jedinečný názov), ale inštancií môže byť (takmer) nekonečne veľa (v závislosti od kapacity pamäte RAM). Okrem toho jedna trieda nemôže byť rozdelená do viacerých súborov a musí byť definovaná na jednom mieste naraz. Ak potrebujete vytvoriť viacero tried s rovnakým názvom, použite **namespaces**.
>
> Neskôr si tiež ukážeme, že dobre štruktúrovaný kód umožňuje jeho opakované použitie v mnohých projektoch.

Tento kód definuje triedu `Article` s dvoma vlastnosťami (`title` a `author`). Komentáre sú v PHP ignorované a používajú sa len na statickú kontrolu typu (zvyčajne ich číta PhpStorm).

Kód:

```php
public string $title;
```

znamená definíciu `vlastnosti`, ktorá je vlastnosťou triedy `Article`. Môžeme si to predstaviť tak, že `Article` je druh poľa a `title` je jeho index, do ktorého môžeme zapísať hodnotu. Každá vlastnosť musí mať definovanú prístupnosť (`public`, `protected` alebo `private`), neskôr si vysvetlíme, čo jednotlivé nastavenia znamenajú. Ak neviete, ktorú prístupnosť si v konkrétnom prípade vybrať, zadajte `public`.

Vytvorenie inštancie triedy a objektu
----------------------------------

Pojem `instance` je jedným z najťažších na pochopenie, preto je dôležité, aby ste si nasledujúce riadky prečítali veľmi pozorne a odporúčam vám, aby ste si všetky príklady poctivo vyskúšali.

Ak už naša aplikácia má triedu definovanú v zdrojovom kóde, v skutočnosti sa nikde nepoužíva a PHP o nej nevie. Je to preto, že OOP zavádza princíp zapuzdrenia dát, čo znamená, že trieda má vnútorný (lokálny) kontext, ktorý je platný len vo vnútri triedy. Je to preto, lebo pri bezobjektovom vývoji sme boli zvyknutí vždy vyhodnocovať kód ako celok. Taktiež sa pokúste uvažovať o triede ako o určitej forme funkcie alebo externe volaného programu.

Teraz môžeme vytvoriť našu prvú inštanciu pomocou operátora new `new`.

```php
$myArticle = new Article;
$myArticle->title = 'Môj prvý článok'; // zapíše hodnotu

echo $myArticle->title; // zoznamy "Môj prvý článok"
```

Všimnite si, že celý objekt `Article` vytvorený operátorom `new` sme vložili do premennej `$myArticle`. To pre nás znamená, že objekt je vlastne dátový typ, takže ho môžeme presúvať medzi premennými a ďalej s ním pracovať.

Na manipuláciu s objektom sa používa operátor šípky `->`. Ak máme inštanciu konkrétneho objektu (máme premennú s jeho obsahom), môžeme ľahko zapisovať hodnoty do vnútorných vlastností, čítať ich alebo ich rôznymi spôsobmi meniť pomocou metód (uvidíme neskôr).

**Instancia objektu je vytvorenie lokálnej "živej" kópie triedy v pamäti (premenná).**

Vytvorenie viacerých inštancií tej istej triedy v rovnakom čase
---------------------------------------------

Ako sme už spomínali, každý objekt má lokálny kontext, ktorý sa uplatňuje v jeho vnútornostiach. Vďaka tomuto princípu môžeme vytvoriť mnoho nezávislých inštancií tej istej triedy a voľne s nimi manipulovať. Môžeme dokonca vytvoriť dynamický počet inštancií a uložiť ich napríklad ako prvky poľa. Možnosti sú nekonečné.

> **Upozornenie:**
>
> Pri vytváraní extrémneho počtu inštancií (stovky alebo viac) majte na pamäti pamäťovú stopu, pretože PHP musí uchovávať informácie o všetkých inštanciách v pamäti RAM. V praxi sa však problém pretečenia pamäte kvôli mnohým inštanciám nevyskytuje.

Príklad:

```php
$firstArticle = new Article;
$firstArticle->title = 'Môj prvý článok';

$secondArticle = new Article;
$secondArticle->title = 'O psovi a mačke';

echo $firstArticle->title;  // zoznamy "Môj prvý článok"
echo $secondArticle->title; // vypíše "O psovi a mačke"
```

Všimnite si, že výpis vlastnosti `title` (`->title`) závisí od toho, z ktorej inštancie čítame.

Zhrnutie
-------

Ukázali sme si, ako definovať našu prvú triedu a vytvoriť jej inštanciu (objekt), do ktorej sme zapísali niekoľko hodnôt.

V ďalšom diele si vysvetlíme pojem <a href="/methods-and-passing-input">konštruktor, metódy a odovzdávanie vstupných údajov</a>.
