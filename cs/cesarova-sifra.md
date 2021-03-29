Césarova šifra - jak funguje
============================

> id: "662dd627-c106-406e-ad6a-7ae9a492ad92"
> slugCS: cesarova-sifra
> publicationDate: "2019-08-23 15:43:02"
> mainCategoryId: "3666a8a6-f2a3-405d-8263-bd53c4301fb3"

> **Upozornění:** Tento článek byl napsán před mnoha lety a některé informace mohou být zastaralé, nebo nepravdivé. Při čtení na to, prosím, berte ohled.

Césarova šifra je jedna z nejsnažších hashovacích funkcí. Ve své době byla prakticky neprolomitelná, ale v době moderních počítačů trvá její prolomení jen několik desítek sekund, až několik minut. Je založena na klíči, podle kterého se zpráva zašifruje a pak je dle něj jí možné opět rozšiřovat. Klíč je tedy tajný. V době zašifrování může být zpráva viděna a nebude nic znamenat (jen změť znaků). Jediný způsob prolomení šifry je uhádnutí klíče.

Klíč
--------------------------

Klíč může být jakékoli celé číslo, které má méně cifer, než samotná zpráva. Obvykle se udávají 3 platné cifry (takže existuje 999 kombinací). Každá další cifra zvyšuje bezpečnost. Pro komunikaci 2 stran je nutné, aby obě strany znali právě tento tajný klíč (takže si ho musejí nějak bezpečně předat). Pokud je pro ně klíč známý a pro ostatní ne, tak může být zpráva šířena i nebezpečně a potenciálně se nijak obsah neohrozí, protože potenciální útočník nezná postup, jak zprávu získat zpět.

Pro demonstrační účely budu používat klíč **123**, běžně se používá nějaké jiné náhodné číslo, u kterého se předpokládá, že nepůjde tak snadno uhádnout.

Postup zašifrování
--------------------------

Princip je založen na myšlence nahrazení znaků zprávy za nějaké jiné pomocí klíče. Já tomu říkám "posouvání znaků".

Mějme třeba tuto zprávu, kterou chceme zašifrovat:

```php
TAJNA ZPRAVA
```


Nyní ke každému znaku přiřadíme jeho číslo. Typicky podle abecedy (jeho pořadové číslo). Budu používat anglickou abecedu, tedy tuto řadu znaků:

```php
A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
```


Když bychom přiřadili ke každému znaku své číslo, tak získáme něco jako:

```php
20-1-10-14-1   26-16-18-1-22-1
```


Nyní přichází na řadu klíč. Vezmeme každé jednotlivé číslo a přičteme k němu klíč. Barevně zvýrazňuji, co jsem kam přičetl:

`1  2  3`

```php
21 3 13 15 3   3 17 20 4 23 3
```


Všimněte si toho, že když šifruji znak **Z**, tak zapisuji číslovku 3. Je to z toho důvodu, protože je **Z** poslední znak abecedy, takže po dosažení konce se opět počítá od začátku řady.

Předání zprávy
--------------------------

Nyní můžeme jakkoli předat zprávu. Někdy dokonce i veřejně. Ostatní uvidí jen nelogickou řadu čísel, takže pravděpodobně nebudou ani tušit, že se jedná o nějakou šifru. Bezpečnost také umocňuje samotný klíč, který je tajný. Někdy je vhodné zprávu předávat jako řadu čísel, někdy je vhodné tyto čísla převést na znaky (opět podle stejné abecední řady) a pak předávat posloupnost znaků. Hodně záleží na okolnostech. Obecně ale platí, že číselná řada je lepší, protože málokdo bude tušit, že se jedná o zakódovanou zprávu.

Rozšifrování u příjemce
--------------------------

Příjemce rozšifrovává stejným postupem. Vezme každý jednotlivý znak a odečte čísla podle klíče a následně výsledné hodnoty převede opět pomocí abecedy na znaky. Je to jen obrácený postup šifrování. Důležité je znám **klíč** a **sadu znaků** - tedy to, jak jdou znaky po sobě.

Prolamování a rozluštění
--------------------------

Jediný možný postup rozluštění je vyzkoušet všechny myslitelné kombinace všech potenciálníc klíčů. Pokud neznáme délku klíče, tak se celý proces ještě více komplikuje. Obecně ale platí, že dnešní počítače zvládnou za 1 sekundu prozkoušet něco kolem 100 klíčů, takže 3 znaky dlouhý náhodný klíč se prolomuje něco kolem jedné minuty.

Pokud je však klíč stejně dlouhý nebo delší než originální zpráva, tak ho nelze prolomit, protože každý jednotlivý znak má svůj vlastní klíč, takže se musí pro každý znak prozkoušet všechny kombinace.

Pokud bych měl zprávu "**TAJNA ZPRAVA**", tak má 11 znaků (mezera se nepočítá). Pokud bych chtěl klíč dlouhý též 11 znaků a používal bych řadu anglické abecedy o délce 26 znaků, tak existuje **11^26** = 1,191817654*10²⁷ kombinací, průměrný počítač by tento klíč prolamoval 1,310999419×10²⁶ sekund = 10^20 dní :)
