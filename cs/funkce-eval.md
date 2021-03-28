Funkce eval() v PHP
================================

> id: "861deebf-ad95-4b12-90df-62664843147e"
> slugCS: funkce-eval
> publicationDate: "2020-02-16 18:37:20"
> mainCategoryId: "3666a8a6-f2a3-405d-8263-bd53c4301fb3"

Funkce `eval` slouží ke spuštění předaného řetězce jako PHP kódu.

Návrh jazyka PHP a praktické vlastnosti
---------------------------------------

PHP je interpretovaný jazyk, což znamená zejména to, že se jeho kód vyhodnocuje tzv. **interpretem**, což je speciální typ programu, který čte napsaný kód a ten v reálném čase vyhodnocuje přímo z řetězce. Jiné jazyky (například C) se musí před spuštěním kompilovat do strojového kódu.

Díky tomu, že se PHP interpretuje, tak existuje způsob, jak za běhu programu změnit, co přesně se bude vyhodnocovat a kód sestavit dokonce dynamicky, a přesně k tomu se `eval()` hodí.

Použití jen na vlastní nebezpečí!
---------------------------------

Funkci `eval` použijte jen v případě, kdy přesně víte, co děláte! To znamená zejména to, že máte zkontrolované veškeré vstupy od uživatele a nemůže dojít k narušení bezpečnosti. Pokud by se totiž uživateli podařilo do funkce `eval` propašovat jeho řetězec, bude vyhodnocen jako reálný kód a tím může například smazat celý web, ukrást databázi nebo získat kontrolu nad celým serverem.

Reálný příklad
--------------

Dobrých příkladů, kde lze `eval` použít moc neexistuje, protože **prakticky vždy existuje lepší způsob**, jak úlohu řešit.

Použít lze například při vyhodnocování výrazů:

```php
// Uživatelský dotaz
$query = '5 + 3 * 2';

// Zpracování výrazu jako běžného PHP kódu
eval('$result = @(' . $query . ');');

// Výpis proměnné s řešením výrazu
echo $result; // vypíše 11
```

Podrobnosti jsou v článku <a href="/pokrocila-kalkulacka">Kalkulačka v PHP: Zpracování matematického výrazu jako řetězec</a>.

Použití pro vykreslení šablon
-----------------------------

Občas se `eval` používá pro vyhodnocení vygenerovaného kódu, typicky překompilované šablony.

Jak již ale bylo řečeno, tak lze každý případ řešit jinak a lépe a v tomto případě dává větší smysl serializovanou šablonu uložit do samostatného PHP souboru a ten načíst přes `require` nebo `include`. Kromě toho, že budeme mít nad obsahem šablony plnou kontrolu, tak také zůstane fyzicky na disku, což podporuje zlepšení výkonu aplikace kvůli možnosti cachování.
