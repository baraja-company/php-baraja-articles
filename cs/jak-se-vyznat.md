Jak se vyznat v PHP kódu
================================

> id: d8d452da-37b4-4502-a573-a27afe7ea37a
> slugCS: jak-se-vyznat
> publicationDate: "2020-04-11 18:45:23"
> mainCategoryId: "6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070"

Většinu jazyků lze zapisovat různými způsoby, přičemž dosáhneme stejného výsledku. Zároveň jednou napsaný kód pravděpodobně budete po sobě někdy v budoucnu číst a opravovat nebo doplňovat o nové funkce.

Abyste proto nemuseli nad kódem stále přemýšlet a dobře se v něm orientovalo, existuje přímo v PHP sada nástrojů a způsobů, jak kód "psát správně", resp. jak ho budovat tak, aby se přímo podporovala jeho budoucí čitelnost (klidně i jiným člověkem).

> **Autorská poznámka:**
>
> Praxe ukazuje, že kód zastarává tak rychle, že i sám autor aplikace po půl roce vnímá svůj vlastní kód jako cizí. Pokud ho tedy budeme hned od začátku psát správně, tak to v budoucnu neznemožní jeho další rozšířitelnost.
>
> V reálném vývoji se tajké ukazuje, že jednotné formátování kódu a obecně zavedení pravidel předchází celé řadě chyb.

Poznámky v kódu
---------------

Poznámky nemají na zpracování kódu žádný vliv a slouží pouze pro programátora. U větších ucelených celků kódu je důležité psát poznámmku, která vysvětluje, k čemu kód slouží a jak v principu funguje.

```php
<?php
// Definice proměnných
$a = 5;
$b = 3;
$c = 2;

// Součet všech čísel
$sum = $a + $b + $c;

// Výpis pro uživatele
echo $sum;
```

Poznámka začíná dvojicí lomítek (`//`) a platí až do konce řádku. Použít se může kdekoli.

Poznámka by neměla vysvětlovat konkrétní implementaci algoritmu, ale spíše jeho obecné principy. Důvod je ten, že se kód může v čase několikrát změnit a v takovém případě bychom měli opravit i poznámku.

> **Autorská poznámka:**
>
> Často se stává, že kód nedělá přesně to, co jeho popis vysvětluje. Je to způsobené zejména tím, že programátor někde udělá chybu. Poznámka by proto měla popisovat obecné principy, abychom byli schopni podle toho poté kód vhodně upravit. Nikdy ale nezapomeňte na to, že jediná pravda o tom, co se v aplikace reálně stane, popisuje až konkrétní kód a poznámka na to nemá vliv.

Grafické oddělení částí kódu
----------------------------

Při návrhu aplikace je důležité oddělovat jednotlivé logické bloky od sebe. Obvykle se oddělují do funkcí, metod, nebo u základního kódu aspoň komentářem.

V delším algoritmu obvykle popisuji nejprve celý princip algoritmu na začátku a poté čísluji jednotlivá místa v kódu, aby podle nich mohl vývojář lépe pochopit konkrétní funkčnost.

```php
/**
 * Funkce vypočítá aritmetický průměr.
 *
 * 1. Získání seznamu čísel
 * 2. Zjištění součtu a počtu čísel
 * 3. Výpočet a vypsání průměru
 */

// 1.
$numbers = [1, 3, 8, 12];

// 2.
$sum = array_sum($numbers);
$count = count($numbers);

// 3.
echo 'Průměr je: ' . ($sum / $count);
```

Znaky `/**` začneme víceřádkový komentář, který platí až po značku `*/`. Aby se zachovalo jednoduché čtení, je dobré na začátek každého řádku umístit hvězdičku.

Dokumentační komentáře
----------------------

Pro popis a dokumentaci funkcí (chování, parametry, návratové hodnoty, autor a podobně) se obvykle používají dokumentační komentáře.

Ve starších verzích PHP (před verzí `7.0`) se ještě nepoužívaly datové typy, proto se typ konkrétní proměnné popisoval přímo v komentáři.

```php
/**
 * @author      Jan Barášek <jan@barasek.com>
 * @license     Public domain
 * @link        https://php.baraja.cz
 * @param       float[] $numbers
 * @return      float
 */
function average(array $numbers): float
{
    $sum = array_sum($numbers);
    $count = count($numbers);

    return $sum / $count;
}
```

Dokumentační komentáře se jmenují `dokumentační` zejména z toho důvodu, že mají předem domluvený formát, kterému rozumí jak konkrétní vývojová prostředí (a editory), ale také automatické nástroje pro generování dokumentace nebo kontrolu kódu.

Psát kód česky nebo anglicky?
-----------------------------

Veškerý kód píšu jenom v angličtině (včetně názvů funkcí, proměnných, komentářů, ...).

Má to celou řadu výhod:

- Vývojář si rovnou proaktivně trénuje angličtinu
- Velká část aplikací používá knihovny třetích stran, které jsou v angličtině, takže se automaticky drží konzistence
- Většina pokročilých věcí nemá vůbec český překlad
- Určitě vás napadá celá řada dalších příkladů

PHP angličtinu přímo nevyžaduje a můžete psát všechno v češtině. Použití angličtiny vnímám spíše jako druh investice do budoucna a možnost kód jednoduše rozšiřovat dalšími lidmi, kteří nejsou rodilí mluvčí češtiny.

Kompletně lokalizovaný kód do angličtiny se také používá ve firmách, proto je dobré angličtinu trénovat hned od začátku.

Pořadí početních operací
------------------------

Při početních operacích vždycky myslete na to, že PHP při výpočtech čísla zaokrouhluje. To může často vadit, protože každý výsledek s desetinnými čísly doprovází určitá nepřesnost.

Jako vhodné řešení se jeví nejprve čísla zvětšovat a pak počítat s co největšími čísly. Dochází tak statistcky k menšímu zkreslení.

Příklad:

```php
echo 10 / 3; // Vypíše 3.33333333333
```

V některých případech lze také využít triku, že se desetinná čísla vůbec nebudou používat a všechno vypočítáme jako celé číslo. V takovém případě k takovému zkreslení nedojde:

```php
echo 1 / 2 * 2; // tohle je horší, protože 1/2 = 0.5*2 = 1
echo 2 * 1 / 2; // tohle je lepší, protože 2*1 = 2/2 = 1
```

Až budete řešit velké složité početní operace, použijte pro zápis čísla zlomky.
