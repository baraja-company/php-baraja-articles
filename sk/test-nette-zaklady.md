Test základných znalostí o Nette
================================

> id: d137c177-503c-4e84-8709-0e65b0ce6060
> slug:
> 	cs: test-nette-zaklady
> 	sk: test-zakladnych-znalosti-o-nette
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '28a2aef7-7490-43a9-add7-80d8a051f8a9'
> sourceContentHash: '66fb122b33aa8ce41158345bb01769ab'

Hranica úspešnosti: 15 bodov

*Získate 1 bod za každú správne zodpovedanú otázku. Za nesprávne zodpovedanú otázku nedostanete nič. Ak je odpoveď len čiastočná (a nebolo by možné na jej základe vec naprogramovať), otázka sa počíta ako nesprávna (nie je možné získať polovicu bodu). Ak riešenie obsahuje bezpečnostnú chybu alebo preklep v kóde, alebo preklep v kóde, odpoveď sa považuje za nesprávnu, pretože by sa nespustila.*

-----------

1 Vysvetlite rozdiel medzi cyklami `for`, `while` a `foreach`. Ku každému uveďte 1 konkrétny príklad jeho použitia, ktorý jasne ukazuje jeho hlavnú výhodu.


2. Máme premennú, o ktorej nevieme takmer nič (poznáme len jej názov). Ako môžeme vidieť jeho obsah? Nazýva sa napríklad `$data`.


3. Napíšte nasledujúce príkazy na prácu s úložiskom Git:
- Stiahnutie najnovších zmien zo servera
- označiť súbor `Statistic.php`
- označiť všetky súbory v projekte
- označiť všetky súbory v adresári `cron`
- odovzdanie zmien so správou "Moje odovzdanie"
- odoslanie revízie na server


4. V premennej majme textový reťazec. Uveďte príklad funkcie na výpočet kontrolného súčtu.


5. Napíšte úryvok kódu, ktorý vytvorí akciu `delete` v programe `Presenter`, ktorá prijme ID položky ako celé číslo a odstráni riadok z tabuľky `question` podľa zadaného ID. Po úspešnom vymazaní sa vypíše správa "Question deleted" (Otázka vymazaná) a presmeruje sa na akciu `list`.

Pod otázkou o bod navyše: Ak sa vymazanie z nejakého dôvodu nepodarí, nevyhodí fatálnu chybu, ale informuje o tom používateľa správou (flash message).

6. Keď vytvorím formulár Nette, stane sa komponentom. Čo je komponent Nette?

7. Potrebujem vytvoriť jednoduchý formulár Nette na vloženie záznamu do tabuľky `question`, ktorá obsahuje zoznam otázok. Štruktúra tabuľky je:

| Stĺpec | Vlastnosti |
|-----------|----------------------------------|
| id | int(8), unsigned, auto increment |
| otázka | varchar(255) |
| is_active | tinyint(1), unsigned, predvolená hodnota: 1 |

Vytvorte príslušné polia formulára na vloženie nového riadku do tejto tabuľky. Po vložení záznamu musí byť vypísaná správa FlashMessage informujúca o úspešnom vložení záznamu + presmerovanie na editáciu záznamu (akcia `edit`).

- Overenie, že pole formulára bolo vyplnené
- Overenie, či sa text otázky zmestí do varchar podľa štruktúry tabuľky
- Overenie, že otázka s týmto textom už neexistuje
- Definujte ďalšiu vlastnú tabuľku `group`, ktorá bude obsahovať informácie o skupinách. Pri vytváraní otázky bude potom možné určiť, do ktorej skupiny otázka patrí. Budete musieť nastaviť reláciu medzi tabuľkami (opíšte, ako sa to robí a ako sa to nastaví).
- Aké makro Latte mám použiť na vykreslenie formulára do šablóny (predvolené vykreslenie)?

8. Majme editačný formulár v `Presenter`, ktorý je vytvorený ako komponent. Chceme odovzdať predvolené hodnoty z toho, čo je v databáze, t. j. potrebujeme získať údaje z tabuľky nejakým vhodným spôsobom.
- Ako budete postupovať a aké prvky v kóde potrebujeme?
- Ako odovzdáte identifikátor aktuálne upravovanej položky do formulára?
- Ako nastavíte predvolenú hodnotu prvku formulára?
- Ako môžeme overiť, že sa používateľ pokúša upraviť položku, ktorá v databáze neexistuje, a ako ho o tom môžeme vhodne informovať?

9 Uvažujme nasledujúce údaje získané z databázy (pomocou bežnej databázy Nette):

```php
$questions = $this->db->questions()->fetchAll();
```

- Ako uvedieme text všetkých otázok ako zoznam s odrážkami?
- Ako prenesieme údaje z tabuľky do šablóny Latte?
- Aké makrá Latte budeme potrebovať na zoznam položiek? Uveďte konkrétnu implementáciu výpisu stĺpcov `id` a `name` vo formáte:

	*1024: Ako sa máte?
	*1025: Čo ste mali dnes na obed?

10. Uveďte príklad aspoň 3 rôznych polí formulára, ktoré sú zapísané vo formulári:

```php
$form->add(tady bude příklad);
```

a pre každý z nich vysvetlite, na čo sa používa a aký výstup vracia (dátový typ + príklad).


11. Nechajte si odoslať formulár Nette.
- Ako môžeme odoslať všetky polia (názvy a hodnoty)?
- Uveďte príklad výstupu poľa s názvom `otázka`.
- Poskytnite konkrétnu implementáciu kódu, ktorý bude prechádzať poľom hodnôt a kľúčov a vráti jeden reťazec obsahujúci celkový zoznam kľúčov a hodnôt, aby sme tento reťazec mohli napríklad uložiť do databázy alebo odoslať e-mailom (ukladanie a odosielanie nie je predmetom zadania a nebude sa vyhodnocovať).


12. Pre každú podmienku rozhodnite, či je výsledok TRUE alebo FALSE:
- `1 > 0`
- `1 == 1`
- `1 == "1"`
- `1 === "1"`
- `1 == true`
- `1 === true`
- `1 === false`
- `1 == "1" && 1=== true`


13. Aké dátové typy poznáme v jazyku PHP?
- Uveďte aspoň 5 príkladov názvov dátových typov
- Pri každom príklade uveďte aspoň 3 možné hodnoty, ktoré je možné uložiť do dátového typu (ak dátový typ nedokáže uložiť toľko možných hodnôt, napíšte to)
- Pre každý dátový typ uveďte typický príklad jeho použitia (čo sa v ňom v praxi ukladá)
- Aký je rozdiel medzi podmienkami `==` (dve rovnosti) a `===` (tri rovnosti)?
- Vysvetlite nevýhodu používania `==` v podmienkach a ako konkrétne `==` rieši tento problém (príklad, kedy `==` môže zlyhať a `==` zachráni situáciu)


14. Majme tabuľku koordinácií (coordinations table), ktorá obsahuje zoznam všetkých koordinácií medzi 2 osobami. Jeden z nich organizuje koordináciu a druhý je hosťom. Napíšte databázový výber, ktorý vráti všetky riadky s koordináciami, ktoré sa ma týkajú (som organizátorom koordinácie alebo som hosťom koordinácie). Tabuľka má stĺpce `id`, `id_user_organizer` (id organizátora), `id_user_quest` (id hosťa). Moje ID je uložené obvyklým spôsobom v položke `Presenter`.


15. Skupina otázok o Latte:
- Čo je Latte?
- Aký je rozdiel medzi `variable`, `macro`, `filter` a `n:attribute`? Čo sa kde používa?
- Ako vytvorím odkaz `DashboardPresenter` na akciu `default`?
- Ako vygenerujem odkaz na konkrétnu úpravu (`QuestionPresenter`, `edit` akcia) otázky, aby som odovzdal ID aktuálne uvedenej otázky? Napíšte špecifický kód Latte.

Symbolicky napísané (ukážka v PHP, preložiť do Latte):

```php
foreach ($questions as $question) {
   echo $question->id; // ID otázky
   echo $question->question; // text otázky
}
```

- Ako napísať pevný priestor HTML?


16. Na čo sa používajú služby v systéme Nette?
- Ako sa inštanciuje?
- Čo musí služba robiť, aby sa mohla používať?
- Ako načítam službu do programu Presenter?
- Vezmime si napríklad službu `StatisticManager`, ktorá má verejnú metódu `getStatistics()`, ktorá neprijíma žiadne parametre. Ako načítam túto službu v aplikácii Presenter a zavolám verejnú metódu `getStatistics()` v predvolenej akcii a odovzdám výsledok šablóne?
- Aký je rozdiel medzi `objektom`, `triedou` a `službou`?
- Čo je to `model`, `entita` a `hodnotový objekt`?
- Všetky služby majú hlavného správcu, ktorý ich pozná a môže ich vyzdvihnúť v tomto správcovi. Ako sa volá tento manažér? Ako do nej môžem zaregistrovať novú službu?


17. Neón
- Čo sú to neónové súbory?
- Aké typy existujú a podľa čoho sa triedia?
- Čo obsahujú súbory Neon? Aké údaje sú v nich uložené?
- Uveďte príklad, ako zaregistrovať nasledujúce pole (recept je v jazyku PHP, budete ho musieť preložiť), aby sa dalo použiť ako parameter:

```php
$imageGenerator = [
   "body" => [
      480: [910, 30, 1845, 1150],
      600: [875, 95, 1710, 910],
      768: [975, 130, 1743, 660]
   ]
];
```

- Uveďte príklad registrácie služby v Neone, ktorej odovzdáme parameter `imageGenerator`, ktorý sme zaregistrovali v predchádzajúcej úlohe, aby ho služba dostala v konštruktore a mohla ho použiť v službe (v zmysle konfigurácie). Pre službu uveďte vzorovú implementáciu konštruktora tak, aby sa prvý vstupný parameter považoval za dátový typ poľa.


18. Predmety všeobecne
- Čo sú to `metóda`, `vlastnosti` a `konštanty`? Aký je medzi nimi rozdiel?
- Metódy aj vlastnosti majú 3 základné stavy prístupnosti (`public`, `private`, `protected`), vysvetlite rozdiel a konkrétny príklad použitia a kto a kedy môže čo vidieť.
- Mám triedu `course`, v ktorej je súkromná vlastnosť `currentCourse`, v ktorej je uložený aktuálny kurz. Ako zabezpečiť, aby vlastnosť bola len na čítanie a nebolo možné do nej zapisovať zvonku?


19. Keď v databáze vytváram tabuľky, ktoré spolu logicky súvisia (napríklad tabuľku pre používateľa a potom tabuľku pre jeho články), potrebujem ošetriť, aby sa údaje správne prepojili.
- Čo zabezpečuje správne prepojenie údajov v tabuľkách v databáze?
- Čo je referenčná integrita a aká je jej úloha v databáze?
- Aké druhy stretnutí máme? Aký je účel jednotlivých typov?
- Aké parametre nastavíme pre relácie, aby sme mohli rôznymi spôsobmi spracovať vymazanie alebo zmenu údajov? Uveďte 3 príklady a konkrétne spôsoby použitia + opis fungovania.


20. Na čo slúžia továrne (návrhový vzor `OOP`)?
- Uveďte príklad použitia.
- Sú služby Nette továrňami?
- Aký je účel injekcie závislostí?
- Aký je rozdiel medzi `DI` a `DIC`?
