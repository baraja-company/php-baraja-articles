PHP funkce date(), datum a čas
==============================

> id: "9b0ec1c7-3431-4d7d-9bcc-6093285f14f1"
> slug:
> 	cs: date
> 
> perex: "Zjištění data a času, aktuální datum, formátování data a času a převod tvaru."
> publicationDate: "2019-09-11 10:14:16"
> mainCategoryId: "6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070"

Funkce `date()` je nástroj pro práci s datem a časem. Používá se ve dvou případech:

- **Zjištění aktuálního stavu**, tedy aktuálního data, času, ... a vypsání v určitém formátu,
- **Převod** konkrétního data do jiného formátu (například číslo měsíce na název, tvar zápisu roků, 12 a 24 soustava hodin, ...).

Ukázka
------

```html
Já jsem speciální stránka. Vím, že právě je
<?php
    echo date('H:i'); // Hodina:minuta
?>
```

> POZOR: PHP nevypisuje Váš čas, ale čas na serveru. Proto se vám může stát, že se zobrazí jiný čas, než má nastavený Váš počítač.

Syntaxe zadávání
--------------------------

Funkce se volá běžným způsobem a jednotlivé požadavky se zadávají jako argumenty funkce.

```php
echo date('formátovací značky', atribut konkrétního času);
```

Formátovací značky uvádějí, v jakém formátu se datum vypíše. Mezi značky můžou být mezery, tečky, dvojtečky, pomlčky, spojovníky a jiné znaky, které nejsou sami formátovací značkou (pokud chcete použít formátovací znak, musíte jej <a href="/uvozovky-vyznam">escapovat</a>). Přehled jednotlivých značek je níže.

Druhý (nepovinný) atribut uvádí ručně zadaný datum nebo čas, který bude převeden a vypsán ve formátu podle prvního parametru. Musí být zadán jako *timestamp* (lze získat přes formátovací značku "U").

Příklad:

```php
echo date('d. m. Y', 1405856605); // vypíše 20. 07. 2014
```

Tabulka povolených formátovacích značek
--------------------------

| Znak | Popis
|------|---------------------
| `Y`  | Rok jako čtyřčíslí (např. 1998)
| `y`  | Rok jako dvojčíslí (např. 98)
| `M`  | Anglická zkratka jména měsíce (např. Jan)
| `m`  | Číslo měsíce (01–12)
| `F`  | Anglické jméno měsíce (např. January)
| `D`  | Anglická zkratka dne v týdnu (např. Fri)
| `l`  | Anglické jméno dne v týdnu (např. Friday)
| `N`  | Číslo dne v týdnu (1 - pondělí, 7 - neděle)
| `w`  | Číslo dne v týdnu (0 - neděle, 1 - pondělí, 6 - sobota)
| `d`  | Číslo dne v měsíci (01–31)
| `j`  | Číslo dne v měsíci (1–31)
| `z`  | Číslo dne v roce (001–365)
| `H`  | Hodina (00–23)
| `h`  | Hodina (01–12)
| `i`  | Minuta (00–59)
| `s`  | Sekunda (00–59)
| `U`  | *Timestamp:* Počet sekund od začátku času (od 1. ledna 1970)
| `S`  | Anglická koncovka pořadového čísla dne v měsíci
| `A`  | Indikátor dopoledne/odpoledne (AM/PM)
| `a`  | Indikátor dopoledne/odpoledne (am/pm)
| `P` | Rozdíl oproti Greenwich time (GMT) s oddělovačem mezi hodinami a minutami (přidáno v PHP 5.1.3), například: `+02:00`

Formátování času pro sitemapu
---------------------------------

Velmi často je potřeba formátovat čas pro soubor `sitemap.xml`, ve kterém se ve značce `<lastmod>` uvádí datum a čas poslední změny.

Od PHP 5.1.3 k tomu lze použít tato syntaxe:

```php
date('Y-m-d\TH:i:sP');
```

Datum a čas se bude zobrazovat na sekundu přesně a bude zahrnuta také informace o časovém pásmu, kde se server nachází (časové pásmo určuje operační systém serveru).

Získání českých názvů dnů a měsíců
----------------------------------

V PHP nelze běžným způsobem získat české názvy dnů a měsíců, proto si takové hodnoty musíme napsat sami. Nejlepší způsob je uložení položek do pole a jejich získání za pomoci volání indexu.

```php
$mesice = [
    1 => 'leden', 'únor', 'březen', 'duben', 'květen',
    'červen', 'červenec', 'srpen', 'září', 'říjen',
    'listopad', 'prosinec'
];

$dny = ['neděle', 'pondělí', 'úterý', 'středa', 'čtvrtek', 'pátek', 'sobota'];
```

Tato ukázka je zjednodušený příklad od <a href="https://php.vrana.cz">**Jakuba Vrány**</a> ze článku <a href="https://php.vrana.cz/ceske-nazvy-mesicu-a-dnu-v-tydnu.php">České názvy měsíců a dnů v týdnu</a>.

Přeložení data z angličtiny do češtiny
--------------------------------------

Často máme datum třeba ve formátu `Friday, 13 September` a chceme jej přeložit do češtiny, ale jak na to? Nejlepší způsob je zavolat nějakou funkci, třeba `datumcesky()`, které předáme anglické datum a ona jej přeloží.

```php
function datumCesky(string $date): string
{
    $men = [
        'January', 'February', 'March', 'April', 'May',
        'June', 'July', 'August', 'September', 'October',
        'November', 'December'
    ];

    $mcz = [
        'ledna', 'února', 'března', 'dubna', 'května',
        'června', 'července', 'srpna', 'září', 'října',
        'listopadu', 'prosince'
    ];

    $date = str_replace($men, $mcz, $date);

    $den = [
        'Monday', 'Tuesday', 'Wednesday', 'Thursday',
        'Friday', 'Saturday', 'Sunday'
    ];

    $dcz = [
        'Pondělí', 'Úterý', 'Středa', 'Čtvrtek',
        'Pátek', 'Sobota', 'Neděle'
    ];

    return str_replace($den, $dcz, $date);
}
```

Příklad použití:

```php
echo datumCesky('Friday, 13 September'); // Pátek, 13 prosince
```

Tato funkce pro překlad rozhodně není ideální, protože pouze nahrazuje anglická slova za česká, nicméně pro mnoho nasazení může stačit. Pro pokročilejší překlad je potřeba vždy garantovat přesnou syntaxi, na základě které se bude překládat vždy do jednotného stylu, třeba: `pátek, 13. prosince`.

Zjištění prvního dne v měsíci
-----------------------------

První den v dubnu 2018 byla neděle, ale jak to snadno zjistit?

Od verze PHP 5.1.0 existuje jednoduché řešení:

```php
echo date('N', strtotime('2018-04-01')); // 1 (pondělí), 7 (neděle)
```

Ve starších verzích si musíme funkci implementovat sami:

```php
/**
 * @author Jan Barášek
 */
function getFirstDayPosition(?int $year = null, ?int $month = null): int
{
    $day = (int) date('w', strtotime($year . '-' . $month . '-1')) - 1;

    return $day < 0 ? 7 : $day + 1;
}
```

Protože modifikátor `w` vrací výstup v americkém formátu, tak číslo dne ještě upravuji jednoduchým výpočtem. Funkce vrací celé číslo v intervalu 1 (pondělí) až 7 (neděle).

Posuny v čase / převod formátování a validace data
--------------------------------------------------

Často potřebujeme přeskočit o relativní čas (třeba o + 5 dnů), nebo z textového vstupu uživatele vytáhnout datum tak, aby byl validní. K tomu slouží funkce <a href="https://php.net/manual/en/function.strtotime.php">strtotime()</a>, která povoluje následující syntaxi:

```php
echo strtotime('now');
echo strtotime('10 September 2000');
echo strtotime('+1 day');
echo strtotime('+1 week');
echo strtotime('+1 week 2 days 4 hours 2 seconds');
echo strtotime('next Thursday');
echo strtotime('last Monday');
```

Výstup funkce je **timestamp** *(univerzální čas)* zadaného času jako integer.

> Obecně doporučuji tuto funkci nasadit na všechny formuláře, kde nějakým způsobem pracujeme s časem od uživatele. Určitě není dobré uživatele nutit psát datum v nějakém konkrétním formátu, ale vždy si takový formát vytvořit automaticky, ať si uživatel může psát, co chce. Často totiž text například odněkud vykopíruje a ruční přeformátování je příliš mnoho práce, když to lze dělat automaticky.

Počet sekund od začátku času
--------------------------

Od 1. ledna 1970 se každou sekundu přičte jednička a tím získáme obrovské celé číslo, které uvádí čas, který od té doby uběhl. Toto se hodí zejména pro jednoduché počítání rozdílů časů. Je to poměrně spolehlivé řešení, ale má i své rizika (problém roku 2038).

**Obecné vlastnosti tohoto zápisu:**
- Je to celé číslo, takže se s tím dobře pracuje,
- Je v desítkové soustavě, takže je i pro lidi dobře čitelné (akorát nelze rychle poznat, o jaký přesný čas jde),
- Nelze jím vyjádřit období před 1.1.1970, protože nemůže být záporné *(některé nové verze algoritmu implementují i záporné časy, ale nelze se na to vždy spolehnout)*,
- V roce 2038 přestane fungovat na 32 bitových počítačích a způsobí pád programu (kvůli přetečení zásobníku a maximální velikosti integeru).

Problém roku 2038
--------------------------

<h1 id="universalTime" style="background: #222; margin-top: 32px; padding: 32px; color: white; text-align: center; border-radius: 3px;">Načítám...</h1>

<script>
	setInterval(function() {
		window.document.getElementById('universalTime').innerHTML = Math.floor(Date.now() / 1000);
	}, 250);
</script>

> Nad tímto textem se zobrazuje aktuální hodnota času, která se každou sekundu zvětší o +1. Aktuální hodnota se načítá javascriptem, proto nemusí být vždy přesná (udává systémový čas vašeho počítače).

Počítače ukládají celá čísla v binární podobě, a pokud jde o integer, tak má často 32 bitů (32 jedniček a nul). Nejvyšším možným 32-bitovým číslem je: `011 111 111 111 111 111 111 111 111 111 11`.

Pokud však každou sekundu přičteme k této konstantě +1, tak jednoho dne takové číslo získáme *(bude to 19. ledna 2038 v 03:14:07)*. Při pokusu o přičtení další jedničky již ale nebude stačit bitový rozsah (číslo by bylo větší, než je možné uložit na 32 bitů), tak dojde k **přetečení zásobníku**, tj. na začátek čísla se doplní jednička, což v binární podobě znamená **záporné číslo**, (takže to nejspíše způsobí pád programu), dostaneme se někam na rok 1901 (uff!).

Řešení tohoto problému jsou dvě:

- Rozšíření rozsahu integeru z 32 bitů na 64 bitů, čímž získáme rozpětí na několik tisíc let
- Použít datový typ `\DateTime` (v PHP běžně dostupný), který poskytuje objektový přístup ke správě data, je dobře kompatibilní s databází a nabízí rozpětí roků od `0001` do `9999`, což je dostatečné pro potřebu většiny reálných aplikací.

Osobně zastávám variantu použít datový typ `\DateTime` a ukládání do integeru vůbec nepoužívat.

Různé časové zóny
-----------------

PHP je inteligentní, proto se snaží vždy použít aktuální nejlepší časovou zónu, kde je umístěn server. Občas se však může stát, že je zóna nesprávně určena, nebo programujeme globální aplikaci, kam přistupují uživatelé z celého světa a proto potřebujeme časovou zónu ručně změnit.

To se dá udělat globálně pro celý PHP script zavoláním funkce:

```php
date_default_timezone_set('UTC');
```

Občas se lze setkat i s tímto řešením (detekce toho, že má server jinou časovou zónu, než UTC):

```php
$defaultTimeZone = 'UTC';

if (date_default_timezone_get() != $defaultTimeZone) {
    date_default_timezone_set($defaultTimeZone);
}
```

Změna data a času
--------------------------

Za získání aktuálního data a času není odpovědné přímo PHP, ale operační systém, kde běží. Pokud tedy potřebujete ručně změnit čas, změňte to přímo tam.
