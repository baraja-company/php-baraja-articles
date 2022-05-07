Kalkulačka v PHP: spracovanie matematického výrazu ako reťazca
==============================================================

> id: e6798758-031d-4e7c-b24e-1f77cf61558d
> slug:
> 	cs: pokrocila-kalkulacka
> 	sk: kalkulacka-v-php-spracovanie-matematickeho-vyrazu-ako-retazca
> 
> publicationDate: '2020-02-16 17:07:38'
> mainCategoryId: c2134b23-9b10-46b3-aa54-e3996707255e
> sourceContentHash: '63f5336bf2bbabe5312121b57e1ce34e'

Predstavte si, že máte za úlohu spracovať jednoduchý matematický príklad, ktorý používateľ zadá ako textový reťazec do vyhľadávacieho poľa. Používateľ chce zvyčajne vykonať jednoduchú číselnú operáciu s číslami. Tento článok opisuje myšlienkový postup a konkrétne pokyny, ako to urobiť.

Naivná implementácia
-------------------

Dlho som rozmýšľal, či by sa jednoduchý matematický výraz nedal spracovať nejakým trikom, aby bol kód čo najkratší... a po mnohých rokoch mám riešenie.

Uvedené riešenie považujte **len za príklad**, pretože je **veľmi nebezpečné** a nepoctivý používateľ môže ľahko podčiarknuť reťazec, čím napríklad vymaže celú aplikáciu alebo ukradne databázu!

```php
// Dotaz používateľa
$query = '5 + 3 * 2';

// Spracovanie výrazu ako regulárneho kódu PHP
eval('$result = @(' . $query . ');');

// Výpis premennej s riešením výrazu
echo $result; // vytlačí 11
```

Trik spočíva v tom, že funkcia <a href="/function-eval">eval()</a> vykoná reťazec, ako keby bol v kontexte kódu PHP. Je to šialené, ale funguje to. Obal potláča chybové hlásenia.

Riešenie zložitejších vstupov
--------------------------

Okrem toho, že **spracovanie výrazov pomocou funkcie eval() je mimoriadne nebezpečné**, neposkytuje ani dostatočne výstižnú syntax, ktorá by vyhovovala každému. Ak používateľ poruší čo i len jednu syntax, celý výraz nebude možné spracovať.

Preto je riešením najprv **pochopiť** a opraviť používateľský dotaz podľa formálneho aspektu (tzv. normalizácia na kanonickú formu) a potom ho odovzdať a ďalej spracovať.

V minulosti som naprogramoval [QueryNormalizer](https://github.com/mathematicator-core/engine/blob/master/src/QueryNormalizer.php) presne na túto úlohu.

Samotné spracovanie je veľmi náročná úloha, pretože musíte správne pochopiť rôzne súvislosti. Napríklad, že zátvorky označujú vnorené bloky a musia sa vyhodnocovať rekurzívne. Napríklad výraz `5+2^(1+3/2)` sa nedá vyriešiť jednoducho, pretože najprv sa musí vyriešiť zlomok, ktorý sa pripočíta k číslu v zátvorke, potom sa vyrieši celočíselná mocnina a nakoniec sa pripočíta na úrovni základu.

Aby sme vôbec mohli splniť túto náročnú požiadavku, výraz už nemôžeme považovať za obyčajný reťazec a musíme **prekročiť úroveň abstrakcie**. Matematika je v podstate druh jazyka, ktorý opisuje vzťahy medzi operáciami a číslami, pretože sa musíme zaoberať prioritami operátorov, rôznymi významami, kontextom, rekurziou a dokonca aj dátovými typmi. Tu prichádza na rad **proces tokenizácie dotazov**.

> Problémom matematickej tokenizácie sa zaoberám od roku 2015 a odvtedy som napísal niekoľko rôznych analyzátorov.

Najlepší z nich, ktorý v súčasnosti poháňa nový Mathematicator, je [dostupný ako opensource na GitHub](https://github.com/mathematicator-core/tokenizer).

Zmyslom tokenizácie je **rozobrať reťazec**, rozdeliť ho na skupiny menších reťazcov známych typov a tie potom previesť na objekty (dátové typy). Prevedené pole objektov sa potom pomocou **chytrej logiky prevedie na binárny strom**, ktorý dokáže popísať závislosti a rekurziu. Ide o veľmi náročný proces, pretože existujú stovky možných scenárov a používatelia môžu byť pri zadávaní požiadaviek veľmi kreatívni.

Hlavnou výhodou poľa tokenov je, že ho možno veľmi ľahko odovzdať ďalšej vrstve, ktorá napríklad [vykoná samotný výpočet](https://github.com/mathematicator-core/calculator) alebo [prekreslí strom do LaTeXu](https://github.com/mathematicator-core/tokenizer/blob/master/src/TokensToLatex.php).

Použitie môže vyzerať takto elegantne:

```php
$tokenizer = new Tokenizer(/* niektoré závislosti */);

// Prevod matematického vzorca na pole tokenov:
$tokens = $tokenizer->tokenize('(5+3)*(2/(7+3))');

// Teraz môžete konvertovať tokeny do užitočnejšieho formátu:
$objectTokens = $tokenizer->tokensToObject($tokens);

dump($objectTokens); // Vrátenie typovaných tokenov s metaúdajmi

// Renderovanie do LaTeXu
echo $tokenizer->tokensToLatex($objectTokens);

// Renderovanie do stromu ladenia (extrémne rýchle):
echo $tokenizer->renderTokensTree($objectTokens);
```

Zobraziť postupy
-----------------

Značný počet používateľov ocení, keď **pri výpočte program zobrazí postup** a ukáže, ako ho vykonal. To je vlastne užitočné aj pre programátora, pretože aspoň môže ľahko zistiť, kde je vo výpočte chyba, a podľa toho algoritmus opraviť. Keď toto všetko skombinujete so strojovým učením založeným na automatizovaných testoch, získate niečo úžasné.

Pozrite sa, ako `QueryNormalizer` dokázal porozumieť vášmu dotazu, odovzdať údaje tokenizéru, ten podľa neho vykreslil dotaz do LaTeXu a potom odovzdal strom objektov kalkulačke, ktorá vrátila celkový výsledok.

Příklad: [5+2^(1+3/2)](https://mathematicator.com/search/5%2B2%5E%281%2B3/2%29).

Reprezentácia postupu sa realizuje tak, že kalkulačka prechádza vstupným stromom a vyhodnocuje po jednom pravidle podľa tokenov a pravidiel, ktoré obsahuje. Keď sa vyhodnotí akékoľvek pravidlo, vloží sa informácia o kroku do poľa. Občas sa môže stať, že sa nejaký krok ukáže ako nesprávny a budeme sa musieť vrátiť späť a zvoliť inú cestu výpočtu, ale za tým sa skrýva pomerne veľa mágie, ktorá zatiaľ zostane skrytá a môžete si ju preštudovať pri implementácii.

Záver
-----

Uvedený postup opisuje, ako elegantne narábať s matematickými výrazmi, v ktorých máme čísla, operácie a vzťahy s nimi. Týmto prístupom nemožno napríklad upravovať výrazy alebo riešiť rovnice, ale na to sa pozrieme nabudúce.

*Ak máte iné nápady, ako efektívne spracovať matematiku, budem rád, ak mi ich dáte vedieť.*
