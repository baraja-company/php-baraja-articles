Výjimky a jejich odchycení v PHP
================================

> id: 61b0f178-bb1c-4166-9e8a-af49de2e2a8c
> slugCS: vyjimky
> publicationDate: 2020-02-16 22:18:18
> mainCategoryId: 3e45c55a-a4cd-4745-b1bb-0332702fefbf

Výjimky jsou nástroj Objektově orientovaného programování, které přináší elegantní způsob, jak vyhazovat a zpracovávat (ošetřovat) aplikační chyby.

Výjimka se nejprve vyhodí (`thrown`), ošetří (`try`) a zachytí (`catch`). Povinné je pouze vyhození.

Filosofie vzniku výjimek
-------------------------

Ještě před vznikem výjimek bylo zpracování chyb v programování velmi komplikované, protože jsme museli spoléhat na návratové hodnoty funkcí a ty vlastním způsobem odchytávat a podle toho se dále chovat.

Samotné funkce totiž zpracování chyb nijak nevynucují, což může vést k fatálním problémů, ale o tom už psal David v článku <a href="https://phpfashion.com/programatori-chyby-neignoruji">Programátoři chyby neignorují</a>.

Příklad zapomenutého ošetření chyb:

```php
// přesouváme z disku na disk
copy('c:/oldfile', 'd:/newfile');
unlink('c:/oldfile');
// pokud první operace selže, soubor se nenávratně smaže
```

Správně bychom měli totiž ošetřit výstup z funkce `copy()` a v případě chyby nepokračovat a vyhodit chybu. V případě starých dobrých funkcí by to mohlo vypadat takto:

```php
function backup(): bool
{
   if (copy('c:/oldfile', 'd:/newfile')) {
      return unlink('c:/oldfile');
   }
   return false;
}
```

Naše funkce `backup()` vrátí `true` jen v případě, že funkce `copy()` neselhala a zároveň funkce `unlink()` vrátila `true`. V ostatních případech vrátí `false`.

Je tím ale aplikace nyní bezpečně ošetřena? Není! Nyní musíme totiž ošetřit výstup z funkce `backup()` v místě, kde ji budeme volat a pokud selže, tak se dokonce ani nedozvíme, proč. Zkrátka vrátí hodnotu `false` a chybu musíme odhalit nějak sami. Asi je v tomto případě dobře vidět, že se programátoři na ošetřování chyb často vykašlou, nebo zkrátka něco zapomenou ošetřit a aplikace kvůli tomu vyhazuje těžko odhalitelné chyby.

Řešení tohoto problému je použití výjimek, které si ošetření vynucují a pokud se neošetří, tak aplikace úplně spadne a vždy se dozvíme, z jakého důvodu.

Základní definice výjimky
--------------------------

Výjimka je v PHP speciální druh rozhraní, které implementuje nativní třída `Exception`, kterou budeme používat.

Pokud zpracování určité části programu selže, jednoduše vyhodíme výjimku s popisem problému:

```php
if (copy('c:/oldfile', 'd:/newfile') === false) {
   throw new \Exception('Can not copy file "oldfile".');
}
```

Vyhození výjimky provedeme klíčovým slovem `throw`, za kterým následuje vytvoření instance třídy s výjimkou. Instanci můžeme získat i jiným způsobem (napřípad si ji předat z proměnné) a samotné vytvoření instance výjimky ještě nezpůsobí její vyhození.

První argument konstruktoru třídy `\Exception` přijímá text výjimky, který by měl výstižným způsobem vysvětlovat, co se zrovna stalo. Dobrým zvykem je přikládat také informaci o prováděné operaci a referenci na data. Pokud například selhalo kopírování souboru, je dobrým zvykem předávat název souboru. V případě selhání provádění SQL dotazu zase předáme prováděný dotaz. Hodně nám to pomůže později při zpracování chyb, protože budeme rovnou vidět, v čem je přesně problém.

Ošetření výjimky
-----------------

Mějme například funkci `backup()`, která zálohuje data a může vyhodit dvojici chyb:

```php
function backup(): void
{
   if (copy('c:/oldfile', 'd:/newfile')) {
      if (unlink('c:/oldfile') === false) {
         throw new \Exception('Can nto remove old file.');
      }
   }
   throw new \Exception('Can not copy backup files.');
}
```

Všimněte si, že funkce nevrací žádný výstup a v definici uvádíme typ `void`. Funkce nic vracet nemusí, protože se za úspěch považuje stav, kdy se žádná chyba nevyhodila a pozitivní scénář nemusíme ošetřovat.

Pokud bychom v aplikaci funkci použili bez ošetření třeba takto:

```php
echo 'Zálohování souborů...';
backup();
echo 'Zálohování dokončeno.';
```

Tak bude běžným způsobem fungovat. Pokud však dojde k chybě, script se automaticky ukončí a na výstupu se zobrazí text výjimky. Důležité je, že se nebude pokračovat dále v provádění kódu a víme, že nedojde k poškození dat.

Pokud chceme v provádění dále pokračovat, je nutné chybu tzv. **ošetřit**, což uděláme kontrukcí `try` a `catch`:

```php
echo 'Zálohování souborů...';
try {
   backup();
} catch (\Exception $e) {
   echo 'Zálohování selhalo: ' . $e->getMessage();
}
echo 'Zálohování dokončeno.';
```

V případě vyhození výjimky dojde k zavolání kódu v oblasti `catch()` (která výjimku přijme podle toho, že splňuje její datový typ) a provede se vnitřní kód.

Vždy získáme instanci třídy výjimky, kterou můžeme použít například k zobrazení chybové zprávy, což se řeší metodou `getMessage()`. Užitečné je také znát metodu `getFile()`, která vrátí diskovou cestu k souboru s chybou, `getCode()`, která vrátí stavový kód chyby a `getLine()`, která vrátí číslo řádku, kde se vyhodila výjimka.

Předpřipravené výjimky
------------------------

Kromě základní výjimky `\Exception` PHP obsahuje ještě další předpřipravené typy výjimek, které se hodí pro různé způsoby použití.

| Datový typ | Vysvětlení |
|------------|-----------|
| `LogicException` | Logická chyba, předvídatelná již při návrhu programu |
| `BadFunctionCallException` | Chyba při volání funkce; funkce nenalezena; volání nepovoleno |
| `BadMethodCallException` | Chyba při volání metody |
| `InvalidArgumentException` | Špatný argument (nevalidní) předaný funkci nebo metodě |
| `OutOfRangeException` | Hodnota mimo rozsah pole či kolekce |
| `LengthException` | Hodnota překračuje povolenou délku |
| `DomainException` | Hodnota nespadá do požadované domény nebo rozsahu |
| `RuntimeException` | Chyba zjistitelná jen za běhu programu (například selhání zápisu do souboru) |
| `OverflowException` | Přetečení bufferu či aritmetické operace, často vzniká jako zpracování více dat, než očekáváno |
| `UnderflowException` | Podtečení bufferu či aritmetické operace, bylo předáno méně dat, než očekáváno |
| `OutOfBoundsException` | Index mimo rozsah pole či kolekce |
| `RangeException` | Hodnota nespadá do požadovaného rozsahu |
| `UnexpectedValueException` | Neočekávaná hodnota (např. návratová hodnota funkce) |

Výjimkám `LogicException` a `RuntimeException` bychom měli předcházet správným návrhem programu. Osobně je používám jen pro výjimečné situace, jako je selhání zápisu do souboru a komunikace s externí službou.

Výjimku typu `RuntimeException` doporučuji vůbec nezachytávat a nechat aplikaci selhat. Jde obvykle o závažný problém, o kterém bychom se měli co nejdřív dozvědět.
