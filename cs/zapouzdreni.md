Princip zapouzdření v OOP
=========================

> id: "54968a42-b678-4385-91ac-c13ba96c9b34"
> slug:
> 	cs: zapouzdreni
> 
> publicationDate: "2020-02-16 21:21:35"
> mainCategoryId: "3e45c55a-a4cd-4745-b1bb-0332702fefbf"

Jeden z hlavních principů OOP je tzv. **princip zapouzdření**, který říká, že by se měly složité úlohy rozdělit na mnoho malých problémů, které umíme řešit samostatně a najednou. Zároveň nás jako uživatele nezajímá, jak se to stane a data (vnitřní stav) zůstanou izolována.

Pokud například řešíme úlohu, jak na základě uživatelského dotazu s výrazem `(5+3)*(2/(7+3))` vrátit výsledek `1.6`, tak pravděpodobně nikdo z nás nedokáže napsat jednu funkci nebo metodu, která tuto úlohu vyřeší najednou.

**TIP:** Hotové řešení tohoto typu příkladu je ve článku <a href="/pokrocila-kalkulacka">Zpracování matematického výrazu jako řetězec</a>, ale připravte se ale na to, že to není nic lehkého.

Zapouzdření přináší abstrakci nad objekty
-----------------------------------------

Díky zapouzdření budete moci objekty používat "jako uživatel", tedy volat jejich metody a vůbec neřešit, jak vnitřně fungují.

Dejme tomu, že řešíme výpočet mzdy zaměstnance a chceme k tomu použít již hotovou třídu od jiného programátora. Stačí nám pouze znát povinné parametry konstruktoru a můžeme třídu "prostě používat":

```php
$mzda = new MzdaZamestnance(
    25000, // hrubá mzda
    6,     // počet let ve firmě
    10,    // počet let praxe
    true   // je to muž?
);

echo $mzda->getHruba(); // 25000

echo $mzda->getCista(); // 17800
```

Parametry objektu jsou smyšlené a neodpovídají reálně tomu, jak se mzda počítá. Princip ilustruje zejména to, že nám **stačí znát pouze obecné veřejné rozhraní** a nemusíme vůbec řešit **vnitřní stav objektu**, dokonce ani **vnitřní implementaci** a už vůbec **proč to funguje tak, jak to funguje**. Jednoduše zavoláme metodu `getCista()` a dostaneme čistou mzdu.

Zapouzdření je otázka návrhu
----------------------------

Je důležité si uvědomit, že samotné **zapouzdření není vlastnost ani syntaxe jazyka**. To, že je třída a aplikace zapouzdřená je pouze otázka návrhu aplikace od programátora a to, že o kódu přemýšlí.

Vždy přemýšlejte při návrhu třídy takto:

- KISS (keep it simple), nechte rozhraní jednoduché a nenuťte uživatele zbytečně přemýšlet. Složitou logiku vyřešte za uživatele a bude vděčný.
- Uživatel třídy (jiný programátor nebo vy budoucnosti) vůbec nemusí znát vnitřní logiku a musí mu stačit jen názvy metody a jejich parametry.
- Pokud pro výpočet potřebuji pomocné výpočty, které uživatele nezajímají a jsou pouze technického rázu, nedává smysl pro ně vůbec vytvářet getter a měly by se vypočítat pouze interně.
- Třída musí splňovat základní vlastnosti algoritmu a to zejména to, že funguje obecně pro jakákoli data.
- Veřejně dostupné metody by měly být navrženy tak, aby poskytovaly dostatek informací k tomu, aby šlo objekt v budoucnu snadno rozšířit o nové funkce a mohli jsme nová data snadno vypočítat z toho, co už víme.

Interní data ponechte neveřejná
-------------------------------

Pro property a metody, které řeší vnitřní logiku, dává smysl nastavit viditelnost jako `private`. Hlavní výhoda je zejména v tom, že nepůjdou volat zvenku a uživatel bude nucen použít vaše navržené rozhraní, díky čemuž si ochráníte data a vnitřní stav objektu.

Například mějme objekt reprezentující bankovní účet, na který chceme vkládat platby a řešit aktuální zůstatek:

```php
class BankAccount
{
    private int $sum;

    public function __construct(int $startSum)
    {
        $this->sum = $startSum >= 0 ? $startSum : 0;
    }

    public function getSum(): int
    {
        return $this->sum;
    }

    public function pay(int $price): void
    {
        $newSum = $this->sum - $price;
        if ($newSum < 0) {
            throw new \Exception('Tolik peněz nemáte!');
        }

        $this->sum = $newSum;
    }

    public function addMoney(int $money): void
    {
        $this->sum += $money;
    }
}
```

Všimněte si toho, že třída obsahuje pouze jedinou `private` property `$sum`, která obsahuje aktuální zůstatek.

Pokud chceme zjistit aktuální zůstatek, existuje pro to metoda `getSum()`, nicméně novou hodnotu zůstatku nemáme vůbec jak změnit. Můžeme pouze peníze odebírat metodou `pay()` nebo přidávat metodou `addMoney()`.

Díky tomuto principu vždycky bezpečně víme, že nám nikdo nemůže objekt rozbít.

Pokud se uživatel pokusí zaplatit více peněz, než kolik je reálně na účtu, metoda `pay()` to nedovolí, protože ještě před přepsáním property `$sum` provede kontrolní výpočet a pokud by měl být zůstatek záporný (menší než nula), vyhodí se chybová výjimka a operace se zastaví.

Závěr
-----

Ukázali jsme si základní princip zapouzdření, který nám umožňuje lépe přemýšlet nad abstrakcí objektů a přináší úplně nový pohled na věc.

Až tento princip dobře pochopíte, uvidíte, že vám <a href="/proc-pouzivat-frameworky">začnou dávat frameworky obrovský smysl</a>, protože interně zapouzdřují mnoho chytrosti, kterou můžete jen používat.

Příště se podíváme na <a href="/dedicnost-a-viditelnost">Dědičnost a viditelnost</a>.
