Princíp zapuzdrenia v PPE
=========================

> id: '54968a42-b678-4385-91ac-c13ba96c9b34'
> slug:
> 	cs: zapouzdreni
> 	sk: princip-zapuzdrenia-v-ppe
> 
> publicationDate: '2020-02-16 21:21:35'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '3fbce6cdcbf5f274312496604569d9c2'

Jedným z hlavných princípov OOP je **princíp zapuzdrenia**, ktorý hovorí, že komplexné problémy by sa mali rozdeliť na mnoho malých problémov, ktoré môžeme riešiť nezávisle a súčasne. Zároveň nás ako používateľov nezaujíma, ako sa to deje, a údaje (vnútorný stav) zostávajú izolované.

Ak napríklad riešime problém, ako vrátiť výsledok `1,6` na základe používateľského dotazu s výrazom `(5+3)*(2/(7+3))`, pravdepodobne nikto z nás nedokáže napísať jedinú funkciu alebo metódu, ktorá by tento problém vyriešila naraz.

**TIP:** Hotové riešenie takéhoto typu príkladu je v článku <a href="/pokrocila-kalkulacka">Spracovanie matematického výrazu ako reťazca</a>, ale pripravte sa na to, že to nie je jednoduché.

Zapuzdrenie prináša abstrakciu nad objektmi
-----------------------------------------

Vďaka zapuzdreniu budete môcť objekty používať "ako používateľ", to znamená, že budete volať ich metódy a nebudete sa vôbec starať o to, ako fungujú vo vnútri.

Predpokladajme, že sa zaoberáme výpočtom platu zamestnanca a chceme na to použiť existujúcu triedu od iného programátora. Potrebujeme poznať len povinné parametre konštruktora a môžeme triedu "jednoducho použiť":

```php
$mzda = new MzdaZamestnance(
    25000, // hrubá mzda
    6,     // počet rokov v spoločnosti
    10,    // počet rokov praxe
    true   // je to muž?
);

echo $mzda->getHruba(); // 25000

echo $mzda->getCista(); // 17800
```

Parametre objektu sú fiktívne a reálne nezodpovedajú spôsobu výpočtu mzdy. Tento princíp ilustruje najmä skutočnosť, že **stačí poznať všeobecné verejné rozhranie** a nemusíme sa zaoberať ani **interným stavom objektu**, dokonca ani **internou implementáciou** a už vôbec nie tým, **prečo funguje tak, ako funguje**. Jednoducho zavoláme metódu `getCista()` a získame čistú výplatu.

Zapuzdrenie je otázka návrhu
----------------------------

Je dôležité poznamenať, že samotné **zapuzdrenie nie je vlastnosťou alebo syntaxou jazyka**. To, že je trieda a aplikácia zapúzdrená, je len záležitosťou programátora, ktorý aplikáciu navrhuje a premýšľa o kóde.

Vždy uvažujte o dizajne triedy týmto spôsobom:

- KISS (keep it simple), udržujte rozhranie jednoduché a nenúťte používateľa zbytočne premýšľať. Vyriešte zložitú logiku za používateľa a on vám bude vďačný.
- Používateľ triedy (iný programátor alebo vy v budúcnosti) vôbec nemusí poznať vnútornú logiku a mali by mu stačiť len názvy metód a ich parametre.
- Ak pre výpočet potrebujem pomocné výpočty, ktoré používateľa nezaujímajú a sú len technické, nemá zmysel pre ne vôbec vytvárať getter a mali by sa počítať len interne.
- Trieda musí spĺňať základné vlastnosti algoritmu, najmä to, že funguje všeobecne pre akékoľvek údaje.
- Verejne dostupné metódy by mali byť navrhnuté tak, aby poskytovali dostatok informácií na jednoduché rozšírenie objektu o nové funkcie v budúcnosti, aby sme mohli ľahko vypočítať nové údaje z toho, čo už poznáme.

Udržujte interné údaje neverejné
-------------------------------

Pre vlastnosti a metódy, ktoré sa týkajú vnútornej logiky, má zmysel nastaviť viditeľnosť ako `private`. Hlavnou výhodou je, že nebudú volané zvonku a používateľ bude nútený používať vami navrhnuté rozhranie, čím sa ochránia údaje a vnútorný stav objektu.

Majme napríklad objekt predstavujúci bankový účet, na ktorom chceme zaúčtovať platby a pracovať s aktuálnym zostatkom:

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
            throw new \Exception("Nemáš toľko peňazí!);
        }

        $this->sum = $newSum;
    }

    public function addMoney(int $money): void
    {
        $this->sum += $money;
    }
}
```

Všimnite si, že trieda obsahuje len jednu `private` vlastnosť `$sum`, ktorá obsahuje aktuálny zostatok.

Ak chceme získať aktuálny zostatok, existuje na to metóda `getSum()`, ale nemáme možnosť zmeniť novú hodnotu zostatku. Peniaze môžeme odstrániť len pomocou metódy `pay()` alebo pridať pomocou metódy `addMoney()`.

Vďaka tomuto princípu máme vždy istotu, že objekt nikto nemôže rozbiť.

Ak sa používateľ pokúsi zaplatiť viac peňazí, ako je v skutočnosti na účte, metóda `pay()` to nedovolí, pretože pred prepísaním vlastnosti `$sum` vykoná kontrolný výpočet, a ak by mal byť zostatok záporný (menší ako nula), vyhodí sa chybová výnimka a operácia sa zastaví.

Záver
-----

Ukázali sme si základný princíp zapuzdrenia, ktorý nám umožňuje lepšie uvažovať o objektovej abstrakcii a prináša úplne nový pohľad.

Keď tento princíp dobre pochopíte, uvidíte, že <a href="/proc-use-frameworks">frameworky začínajú dávať obrovský zmysel</a>, pretože interne zapuzdrujú množstvo šikovnosti, ktorú môžete jednoducho použiť.

Nabudúce sa pozrieme na <a href="/dedicacy-and-visibility">dedicacy and visibility</a>.
