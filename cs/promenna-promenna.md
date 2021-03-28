Proměnné proměnné
================================

> id: a0baeb3a-ab6e-4dd9-b1bc-b1872a12ee08
> slugCS: promenna-promenna
> publicationDate: "2019-08-22 20:48:46"
> mainCategoryId: "0e39aee9-2818-480c-8081-e0c2d039bb24"

> **Upozornění:** Tento článek byl napsán před mnoha lety a některé informace mohou být zastaralé, nebo nepravdivé. Při čtení na to, prosím, berte ohled.

**Proměnné proměnné** nejsou určeny k běžnému nasazení (řeší problémy, které lze řešit i jinak), slouží zejména k zestručnění zápisů a ke složitější práci přístupu do paměti.

Mějme následující příklad:

```php
$x = 25;                  // obsahuje 25
$nacitana_promenna = 'x'; // obsahuje "x"
$y = $$nacitana_promenna; // obsahuje 25
echo $y;                  // vypíše 25
```


Všiměte si dvou dolarů následujících po sobě. V tomto případě bude do proměnné $y načtena hodnota proměnné, která má název, který obsahuje proměnná $nacitana_promenna.

Trochu zmatek, co? Proto radši proměnné proměnné nepoužívejte.
> **Poznámka:** Proměnné proměnné jsou specialitka jazyka PHP právě kvůli znaku dolaru. V jiných jazycích se začátek názvu proměnné neoznačuje žádným znakem, proto tedy nelze proměnné proměnné použít, protože by bylo nejednoznačné, kdy se jedná o klasickou proměnnou a kdy ne.
