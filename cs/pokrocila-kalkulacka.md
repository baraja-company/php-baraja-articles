Kalkulačka v PHP: Zpracování matematického výrazu jako řetězec
================================

> id: e6798758-031d-4e7c-b24e-1f77cf61558d
> slugCS: pokrocila-kalkulacka
> publicationDate: "2020-02-16 17:07:38"
> mainCategoryId: c2134b23-9b10-46b3-aa54-e3996707255e

Představte si, že stojíte před úlohou zpracování jednoduchého matematického příkladu, který Vám uživatel zadá jako textový řetězec třeba do pole pro vyhledávání. Typicky chce provést jednoduchou početní operaci s čísly. Článek popisuje myšlenkové postupy a konkrétní návod, jak na to.

Naivní implementace
-------------------

Dlouho jsem přemýšlel nad tím, jestli by šlo jednoduchý matematický výraz zpracovat nějakým trikem, aby byl kód co nejkratší… a po mnoha letech ono řešení skutečně mám.

Uvedené řešení vnímejte **pouze jako příklad**, je totiž **extrémně nebezpečné** a nečestný uživatel může snadno podtrčit řetězec, který například provede smazání celé aplikace nebo krádež databáze!

```php
// Uživatelský dotaz
$query = '5 + 3 * 2';

// Zpracování výrazu jako běžného PHP kódu
eval('$result = @(' . $query . ');');

// Výpis proměnné s řešením výrazu
echo $result; // vypíše 11
```

Celý trik je v tom, že <a href="/funkce-eval">funkce eval()</a> spouští řetězec tak, jako kdyby byl v kontextu PHP kódu. Šílenost, ale funguje to. Zavináč potlačuje chybová hlášení.

Řešení složitějších vstupů
--------------------------

Kromě toho, že **zpracování výrazů přes funkci eval() je extrémně nebezpečné**, tak zároveň neposkytuje dostatečně výřečnou syntaxi, která vyhovuje všem. Pokud uživatel udělá byť jediný prohřešek vůči syntaxi, celý výraz nebude možné zpracovat.

Řešení proto je **uživatelský dotaz nejprve pochopit** a opravit podle formální stránky (tzv. normalizovat na kanonický tvar) a následně projít a dále zpracovat.

Přesně pro tuto úlohu jsem v minulosti naprogramoval [QueryNormalizer](https://github.com/mathematicator-core/engine/blob/master/src/QueryNormalizer.php).

Samotné zpracování je velmi náročný úkol, protože je potřeba správně chápat různé kontexty. Například, že závorky označují vnořené bloky a musí se vyhodnocovat rekurzivně. Například výraz `5+2^(1+3/2)` není možné řešit rovnou, protože se jako první musí vyřešit zlomek, sečíst s číslem v závorce, poté řešit celá mocnina a nakonec sčítat na základní úrovni.

Abychom tento náročný požadavek vůbec dokázali splnit, už nelze s výrazem pracovat jako s obyčejným řetězcem a je potřeba **zvednout úroveň abstrakce**. V postatě jde o to, že matematika je druh jazyka, který popisuje vztahy mezi operacemi a čísly, protože musíme řešit priority operátorů, různé významy, kontexty, rekurzi a dokonce datové typy. Na řadu proto přichází **proces tokenizace dotazu**.

> Problémem tokenizace matematiky se zabývám od roku 2015 a od té doby jsem napsal několik různých parserů.

Nejlepší z nich, který v současné době pohání nový Mathematicator je [k dispozici opensource na GitHubu](https://github.com/mathematicator-core/tokenizer).

Smyslem tokenizace je **projít řetězec**, rozdělit ho na skupiny menších řetězců známých typů a ty následně převést na objekty (datové typy). Převedené pole objektů následně **chytrou logikou převedeme na binární stromu**, který umí popisovat závislosti a rekurzi. Jde o proces velmi náročný, protože existují stovky možných scénářů a uživatelé umí být v zadávání dotazů hodně kreativní.

Výhoda pole tokenů je hlavně to, že lze velmi jednoduše předat další vrstvě, která například [provede samotný výpočet](https://github.com/mathematicator-core/calculator), nebo [strom překreslí do LaTeXu](https://github.com/mathematicator-core/tokenizer/blob/master/src/TokensToLatex.php).

Použití může vypadat takto elegantně:

```php
$tokenizer = new Tokenizer(/* some dependencies */);

// Convert math formula to an array of tokens:
$tokens = $tokenizer->tokenize('(5+3)*(2/(7+3))');

// Now you can convert tokens to a more useful format:
$objectTokens = $tokenizer->tokensToObject($tokens);

dump($objectTokens); // Return typed tokens with meta data

// Render to LaTeX
echo $tokenizer->tokensToLatex($objectTokens);

// Render to debug tree (extremely fast):
echo $tokenizer->renderTokensTree($objectTokens);
```

Zobrazení postupů
-----------------

Nemalá část uživatelů ocení, když se **při výpočtu zobrazí postup**, jak to program udělal. Hodí se to vlastně i programátorovi, protože aspoň může jednoduše zjistit, kde je ve výpočtu chyba a podle toho algoritmus opravit. Když to celé zkombinujeme se strojovým učením na základě automatických testů, vznikne něco úžasného.

Podívejte se, jak dokázal `QueryNormalizer` pochopit Váš dotaz, předat data na tokenizer, ten podle něj vykreslil zadání dotazu do LaTeXu a následně předal strom objektů do kalkulačky, která vrátila celkový výsledek.

Příklad: [5+2^(1+3/2)](https://mathematicator.com/search/5%2B2%5E%281%2B3/2%29).

Zobrazení postupu je realizováno tak, že kalkulačka postupně prochází vstupní strom a podle obsažených tokenů a pravidel postupně vyhodnocuje jedno pravidlo za druhým. Při vyhodnocení jakékoli pravidla si odloží informaci o kroku do pole. Občas se může stát, že se některý krok ukáže jako chybný a musíme se při výpočtu vrátit a vydat se jinou cestou, ale za tím je už celkem velká magie, která zatím zůstane skryta a můžete si ji prostudovat v implementaci.

Závěr
-----

Uvedený postup popisuje, jak elegantně zpracovávat matematické výrazy, kde máme k dispozici čísla, operace a vztahy s nimi. Tento přístup neumí například upravovat výrazy nebo řešit rovnice, ale na to se podíváme příště.

*Pokud máte jiný nápad, jak matematiku efektivně zpracovávat, budu rád, když mi napíšete.*
