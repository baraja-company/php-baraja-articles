Immutabilita objektů - klíčový návrhový koncept
===============================================

> id: "057467db-4e3b-4e18-9ea5-dfb25feb3800"
> slug:
> 	cs: immutabilita
> 
> publicationDate: "2022-07-24 15:00:00"
> mainCategoryId: "ae4c1c70-11b3-433e-b1d0-e590155bb8b9"

Immutabilita je jeden z nejdůležitějších návrhových konceptů pro budování stabilních aplikací. Základní princip říká, že jednou zapsaný stav může být později už pouze čten bez možnosti jeho modifikace. Pokud potřebujeme stav změnit, musíme vytvořit novou instanci a celý objekt vyměnit za jiný.

Datové typy lze proto velmi hrubě rozdělit na dvě velké kategorie:

- **Mutable** (měnitelný stav v rámci jedné instance)
- **Immutable** (neměnný vnitřní stav)

Mutable objekty lze vnitřně změnit. To znamená, že poskytují operace, které při různé kombinaci volání způsobí, že dostaneme různé výsledky. Immutabilita se tomuto chování snaží zabránit.

Definice
--------

> Třída je **immutable** právě tehdy, pokud po vytvoření instance nejdou data instance žádným způsobem změnit.

Všechna data jsou tedy zafixována v konstruktoru. Všechny skalární datové typy jsou také automaticky immutable.

Zásadní výhoda
--------------

Navrhovat aplikace s immutable stavy přináší zásadní výhodu v bezpečnosti provádění operací. Pokud víme, že jednou zapsaná data nebude možné později změnit (mutovat), můžeme například velmi spolehlivě debugovat, nebo rozdělit aplikaci na dílčí funkce bez rizika, že zapomeneme na některý z mezistavů.

Myšlenka immutability je obecně proti principu ukládání stavů do properties v objektu/entitách a spíše vystihuje funkcionální přístup, kdy data pouze "protékají" přes aplikaci, jako to dělá například javascript.

Z pohledu výkonu můžeme o immutable objektech automaticky říct, že je lze cachovat na neomezeně dlouhou dobu, protože nikdy nebudou neaktuální.

Reálný příklad z PHP
--------------------

Zdaleka nejčastější použití immutable objektů v PHP je objekt `DateTimeImmutable`, který jednou vytvoříme a už nad tím lze volat pouze formátovací metody. Pokud bychom vnitřní nastavení modifikovali, metoda vrátí novou instanci. Tato vlastnost je klíčová například při použití ORM, které využívá tzv. identity pattern - to nám umožní například při čtení data vytvoření objednávky garantovat, že bude všude v aplikaci stejný a nedojde k poškození referenční integrity.

Konkrétní příklad muttable objektu:

```php
$date = new DateTime('2021-05-14');
$tomorrow = $date->modify('+1 day');
echo $date->format('Y-m-d'); // 2021-05-15
echo $tomorrow->format('Y-m-d'); // 2021-05-15
```

Stejný datum se vypsal z toho důvodu, protože metoda `modify()` pouze změnila vnitřní stav objektu `DateTime` a vrátile stejnou instanci. Došlo tedy z tzv. **mutování vnitřního stavu**, což je základní chování objektově orientovaného programování. Aktualizace proměnné změnila i tu původní.

A nyní příklad immutable objektu:

```php
$date = new DateTimeImmutable('2021-05-14');
$tomorrow = $date->modify('+1 day');
echo $date->format('Y-m-d'); // 2021-05-14
echo $tomorrow->format('Y-m-d'); // 2021-05-15
```

Objekt `DateTimeImmutable` je immutable, což znamená to, že se nikdy jeho vnitřní stav nezmění. Do proměnné se po zavolání metody `modify()` uloží nová modifikovaná instance (také immutable). Pokud bychom novou hodnotu neuložili do proměnné, nebude ji možné později použít.

Na originální hodnotu se nikdy nesáhne a zůstane bezpečně uložena.

Kdy má být třída immutable?
---------------------------

Pokud nemáte velmi dobrý důvod, aby byla mutable, pište vždy třídu nebo funkci jako immutable. Do budoucna vám to zjednoduší návrh.

Mutable třídy by se měly měnit co nejméně. Chování immutability doporučuji vždy zdokumentovat.

S immutabilitou se pojí snad jediná nevýhoda, a to ta, že se s každou změnou atributu musí vytvářet nová instance, což má drobný dopad na výkon. Pokud vaše aplikace (stejně jako většina aplikací) data spíše zobrazuje a méně často mění, tak je tato nevýhoda s výkonem dnešních počítačů spíše bezvýznamná.

Jaké typy dat by měly být immutable?
------------------------------------

- Všechny identifikátory a unikátní kódy
- Většina ManyToOne a OneToOne databázových relací
- Data, časy, kalendářní hodnoty
- Cyklem procházené pole, kdy chceme s každým prvkem udělat tu stejnou operaci
- Intervaly, dvojice, trojice, ...
- Geometrické útvary, body, úsečky, GPS souřadnice, fyzické adresy, ...
- Logy a historické záznamy
- Informace o zpracovaných objednávkách a většina finančních dat
- Meta data o související entitě

Co by naopak nemělo být immutable:

- Velké objekty se spoustou vlastností
- Většina tabulkových výstupů z databáze, například Doctrine entity
- Postupně konstruované objekty z menších částí
