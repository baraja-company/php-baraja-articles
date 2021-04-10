UUID a výkon rozsáhlých aplikací
================================

> id: "2f072ce8-13b1-41f6-b328-2bd3b416cdd2"
> slug:
> 	cs: uuid-performance
> 
> publicationDate: "2019-11-08 10:09:54"
> mainCategoryId: "95374429-e651-46bd-9149-15aa716f8207"

Když se velikost databáze rozroste nad řádově miliony řádků, je vhodné začít řešit škálování aplikace a databázi rozdělit na více fyzických serverů.

Největší problém rozdělení databáze na více částí je její následná synchronizace, pokud si uživatel vyžádá konkrétní data.

Proč používat UUID a jaké má výhody proti autoincrementu
--------------------------------------------------------

Dejme tomu, že máte tabulku článků `article`, ale protože máte obrovský web, tak článků existují vyšší desítky milionů a musíte je fyzicky rozdělit na více strojů.

Pokud bychom používali jako `id` (primární klíč) obyčejný integer s nastavením jako autoincrement, velmi rychle zjistíme, že při decentralizovaném vytváření záznamů na různých strojích a následné synchronizaci dochází ke kolizím v ID a musíme pak záznamy složitě přečíslovat. Pokud navíc řešíme mnoho relací na další tabulky, může jít o velmi složitou režii, ve které je snadné chybovat.

Místo číselného identifikátoru proto můžeme vygenerovat tzv. `UUID`, což je textový řetězec, který vzniká složitým algoritmem, jež zaručuje, že bude unikátní, i když se vygeneruje nezávisle na více strojích.

Výhody:

- Pokud máte více nezávislých databází, které následně synchronizujete, použití UUID znamená, že jedno ID je unikátní ve všech databázích, nejen v té, kde se zrovna nacházíte a kam se vygenerovalo. Při sloučení do jednoho clusteru nevznikají konflikty.
- Můžete znát svůj `primární klíč` před samotným vložením záznamu do databáze. Snižuje to počet SQL dotazů, zjednodušuje transakční logiku a můžete ho snadno použít jako cizí klíč ještě předtím, než kolekce záznamů existuje.
- UUID nezveřejňuje informace o počtu dat a návaznostech a je bezpečnější pro použití v URL. Pokud například zjistím, že jsem uživatel `19010018`, jde snadno uhodnout, že existuje i uživatel `19010017` a další. Útočit se dá tzv. Vektorovým útokem.

Generování nového UUID
----------------------

UUID můžeme získat buď jednoduše SQL dotazem `SELECT UUID();`, což ale zvyšuje počet dotazů do databáze a přijdeme o možnost si data nejdřív hromadně připravit v aplikační logice a pak je najednou zapsat.

Jako dobré řešení se mi proto zamlouvá použití balíku <a href="https://github.com/ramsey/uuid">ramsey/uuid</a>, který získáte Composerem. Samotné UUID má několik verzí a balík zvládne hravě vygenerovat všechny druhy podle potřeby.

Použití je pak snadné:

```php
require 'vendor/autoload.php';

use Ramsey\Uuid\Uuid;

// Vygeneruje verzi 1 (time-based) UUID objekt
$uuid1 = Uuid::uuid1();
echo $uuid1->toString() . "\n"; // e4eaaaf2-d142-11e1-b3e4-080027620cdd

// Vygeneruje verzi 3 (name-based a hashováno jako MD5) UUID objekt
$uuid3 = Uuid::uuid3(Uuid::NAMESPACE_DNS, 'php.net');
echo $uuid3->toString() . "\n"; // 11a38b9a-b3da-360f-9353-a5a725514269

// Vygeneruje verzi 4 (náhodně) UUID objekt
$uuid4 = Uuid::uuid4();
echo $uuid4->toString() . "\n"; // 25769c6c-d34d-4bfe-ba98-e0ee856f3e7a

// Vygeneruje verzi 5 (name-based a hashováno jako SHA1) UUID objekt
$uuid5 = Uuid::uuid5(Uuid::NAMESPACE_DNS, 'php.net');
echo $uuid5->toString() . "\n"; // c4a760a8-dbcf-5254-a0d9-6a4474bd1b62
```

Pokud používáte Doctrine, existuje rozšíření <a href="https://github.com/ramsey/uuid-doctrine">ramsey/uuid-doctrine</a>, které ID vygeneruje přímo jako datový typ.

Fyzické uložení do databáze
---------------------------

Při prvních pokusech jsem jako primární klíč (ID) používal `varchar(36)`, ale <a href="https://www.facebook.com/groups/backendisti/permalink/2465260887049808/">není to vůbec dobrý nápad</a>.

> **Vysvětlení vnitřní logiky:**
>
> MySql databáze (a i mnohé další) neumí `varchar`, `char` ani jiné datové typy vyjadřující řetězec použít jako primární klíč efektivně.
> V některých databázích existuje datový typ `GUID`, který je na ukládání UUID přímo vytvořen. Pokud tento typ použít nemůžete, existuje vhodná náhrada ve formě `binary(16)`.

Při fyzickém zkoumání databáze se pak ID reprezentuje v HEX formátu (protože binární formát nelze zobrazit), místo hezkého ID `726c67c4-e5eb-4a4c-8fcc-031da5d6f3c6` pak uvidíte jen `726C67C4E5EB4A4C8FCC031DA5D6F3C6`, což v INSERT dotazu vypadá třeba jako  `'?kYߟKg2c;'`.

Převod původních dat z `varchar(36)` na `binary(16)`
----------------------------------------------------

Předpokládám, že nově nastavené ID v databázi reprezentujete (nebo plánujete reprezentovat) jako:

```sql
`id` binary(16) NOT NULL
```

Pouhá změna datového typu však nefunguje, tedy něco jako:

```sql
SET FOREIGN_KEY_CHECKS=0;

ALTER TABLE article CHANGE id id BINARY(16) NOT NULL

SET FOREIGN_KEY_CHECKS=1;
```

Důvody jsou v podstatě dva:

- Primární klíč i relace na něj `musí mít stejný datový typ`. Je proto potřeba změnit jak datový typ pro ID článku, tak například v relační tabulce, co páruje články s autory.
- Binární formát obsahuje něco trochu jiného, než byl původní řetězec. Je potřeba použít konverzní funkci.

Jediné správné řešení je proto data zálohovat (to byste ale měli udělat před každou migrací tak jako tak), připravit si prázdnou databázi s funkčními relacemi a data tam přes migrace znovu vložit.

Pokud jste předtím UUID generovali podivně, je lepší zvolit nějakou sekvenční metodu, jak UUID získat a všechny záznamy přečíslovat. Důvod je ten, že sekvenční rozložení umožňuje lepší řazení hodnot a vytvoření `btree`, díky čemuž je výkon totožný téměř jako `bigint`.

**Pokud znáte lepší způsob, jak převést existující databázi z UUID ukládané jako varchar na binary formát bez nutnosti vymýšlet složité migrace a se zachováním cizích klíčů, budu moc rád za zpětnou vazbu.**
