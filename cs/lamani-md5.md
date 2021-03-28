Jak prolomit funkci `md5`
================================

> id: "9e678fcc-3d5e-46a3-9c3f-c1eb3d1ad367"
> slugCS: lamani-md5
> publicationDate: "2019-08-23 15:33:10"
> mainCategoryId: "3666a8a6-f2a3-405d-8263-bd53c4301fb3"

MD5 je velmi často používaná funkce pro výpočet hashů.

Začátečníci ji často používají pro <a href="/hashovani">ošetření hesel</a>, což není dobrý nápad, protože existuje mnoho způsobů, jak původní heslo získat.

Tento článek popisuje konkrétní metody, jak na to.

Časová složitost
----------------

Celá bezpečnost je postavena na faktu, že vyzkoušet všechna hesla trvá neúměrně dlouho. Tedy mělo by. Problém v algoritmu `md5()` je zejména v tom, že jde o funkci velmi rychlou. Na běžném počítači není problém vypočítat přes milion hashů za sekundu.

Pokud budeme heslo lámat postupným zkoušením kombinací, jedná se o **útok hrubou silou**.

Metody prolomení
----------------

Strategií existuje několik:

- Postupné zkoušení hodnot pokus-omyl (útok hrubou silou)
- Zkoušení slovníkových hesel
- Duhové tabulky (předpočítaná databáze hashů)
- Vyhledávání přes Google
- Trefování do kolize v algoritmu

Metod existuje mnohem víc, tento článek popisuje jen ty nejčastější.

Strategie lámání hrubou silou
-----------------------------

Postupně se zkoušejí všechny kombinace písmen, čísel a ostatních znaků.

Vygenerované pokusy se postupně zahashují a porovnají s originálním hashem.

Takže například:

```php
aaaaaaabaaacaaadaaaeaaaf...
```

Problém tohoto útoku je právě v samotném algoritmu `md5()`, pokud bychom zkoušeli jen malá písmena anglické abecedy a čísla, tak vyzkoušení všech kombinací na běžně dostupném počítači zabere maximálně desítky minut.

Důležité proto je volit dlouhá hesla, pokud možno náhodná se speciálními znaky.

Strategie slovníkového útoku
----------------------------

Lidé si obvykle volí slabá hesla, která existují ve slovníku.

Pokud tohoto faktu využijeme, můžeme rychle vyřadit nepravděpodobné varianty, jako je například `6w1SCq5cs` a místo toho raději hádat existující slova.

Podle předešlých úniků hesel velkých firem dále víme, že si uživatelé volí velké písmeno na začátku hesla a číslo na konci. Schválně - má to i vaše heslo? :)

Duhové tabulky - předpočítaná databáze
--------------------------------------

Protože jednomu heslu odpovídá vždy stejný hash, lze snadno přepočítat obrovskou databázi, ve které se budou hesla nejprve vyhledávat.

Hledání je totiž vždy řádově rychlejší, než hashe hledat znovu a znovu.

Navíc při větším úniku dat lze takto hesla promalovat paralelně a rychle tak získat třeba 10 % všech hesel uživatelů.

Dobrá databáze hesel je například <a href="https://crackstation.net/">Crack Station</a>.

Vyhledání na Google
-------------------

Mnoho jednoduchých hesel zná přímo Google, protože indexuje stránky, které hashe obsahují.

Google používám vždy jako první možnost. :)

Hledání kolize v algoritmu
--------------------------

<a href="https://cs.wikipedia.org/wiki/Dirichlet%C5%AFv_princip">Dirichletův princip</a> popisuje, že pokud máme množinu hashů, které mají vždy 32 znaků, tak existují aspoň 2 různá hesla o délce 33 znaků (o jedna delší), která generují stejný hash.

V praxi nedává smysl kolize hledat, ale někdy sám autor aplikace hádání usnadní, protože kolize přepočítá.

Například:

```php
$password = 'heslo';

for ($i = 0; $i <= 1000; $i++) {
    $password = md5($password);
}

echo $password; // 1000x zahashováno přes md5()
```

V takovém případě dává smysl hádat spíše kolize, než původní hash.

Bastlení zdar!
