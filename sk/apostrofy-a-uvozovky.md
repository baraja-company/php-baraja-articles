Apostrofy a úvodzovky
=====================

> id: '526ad995-3412-446e-bb56-9627dff8e29e'
> slug:
> 	cs: apostrofy-a-uvozovky
> 	sk: apostrofy-a-uvodzovky
> 
> perex: Použití uvozovek a apostrofů pro ohraničení řetězců v PHP. Escapování řetězců a řešení kontextů.
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1'
> sourceContentHash: c5b0bf8b74238134be5348f886591e2a

Na ohraničenie reťazcov môžete použiť úvodzovky alebo apostrofy. Osobne uprednostňujem iba **apostrofy**, pokiaľ nejde o špeciálny znak prelomu riadku alebo tabulátor.

Dôvodov je viacero, poďme si ich konštruktívne prebrať.

> Hlavným dôvodom, prečo nepoužívať úvodzovky, je bezpečnosť a nevhodné zaobchádzanie s dátovými typmi.

Používanie značiek HTML vo vnútri reťazca
--------------------------------

Ak potrebujeme vrátiť kód HTML v reťazci a tento reťazec zabalíme do úvodzoviek, musíme ho dosť nešikovne escapovať:

```php
return "<a href=\"{$link}\">{$text}</a>";
```

Alebo urobte to isté čitateľnejším spôsobom a ponechajte úvodzovky bez escapovania:

```php
return '<a href="' . $link . '">' . $text . '</a>';
```

Väčšia vizuálna vzdialenosť reťazca od premennej
---------------------------------------------

Nie je nič horšie ako nalepiť premenné vo vnútri reťazca priamo na seba, kde sa nedá rýchlo vizuálne rozlíšiť, čo je premenná a čo reťazec:

```php
$url = "{$baseImageUrl}/{$dirName}/{$basename}.{$ext}";
```

Tento zápis nemá negatívny vplyv na funkčnosť, len jednotlivé premenné a pevné znaky z reťazca sú uložené príliš blízko pri sebe, čo zhoršuje čitateľnosť.

Skúsme to ešte raz a urobme to čitateľnejšie:

```php
$url = $baseImageUrl . '/' . $dirName . '/' . $basename . '.' . $ext;
```

Hlavnú výhodu vidím v čistote okolo premenných, kde neprekážajú žiadne zbytočné znaky v názve.

Okrem toho sa bude ľahšie obaľovať a v reťazci nebudú žiadne obaľovacie znaky ;)

```php
$url = $baseImageUrl
       . '/' . $dirName
       . '/' . $basename . '.' . $ext;
```

Nie je možné zavolať funkciu vnútri úvodzoviek
---------------------------------------

Ale premenná môže, preto sa "niečo" pridáva mimo reťazca (najmä funkcie), ale premenné sa zapisujú späť dovnútra. A niekedy programátor dokonca pridá premennú za reťazec. Skrátka, zmätok nad zmätok.

Prečo nemôžeme robiť veci jednotne?

Nevhodné mapovanie iného dátového typu na reťazec
---------------------------------------------------

Zoberme si nasledujúce volanie funkcie:

```php
echo getFullName("{$user->name}");

function getFullName(string $name): string
{
	// ... implementácia ...
}
```

Premenné je možné vložiť do úvodzoviek, čo spôsobí, že sa prepíšu ako reťazec. Ak sa však premenná nachádza v samotnom reťazci a je iného dátového typu ako string, informácia o pôvodnom dátovom type môže byť poškodená. Ak by napríklad konštrukcia `$user->name` vrátila hodnotu `false` alebo `null`, nevedeli by sme povedať, že ide o chybu.

Aplikácia by mala vždy rozpoznať chybový stav a správne zlyhať, a nie ho ignorovať. Správne hlásenie chýb vedie k lepšiemu ladeniu v budúcnosti.

Občas sa môžete stretnúť s prepismi, najmä preto, že funkcia vyžaduje konkrétny dátový typ:

```php
trim("{$returnText}");
```

V takom prípade sa prikláňam k tomu, aby som si to zapísal:

```php
trim ((string) $returnText);
```

čo nie je až také "magické" a aj menej skúseným používateľom je jasné, čo sa s premennou stalo.

Odstránenie hodnoty `null` z databázy
----------------------------------

Predpokladajme, že z databázy získavame názov hotela:

```php
return "{$row->hotel->name}";
```

Ale čo ak názov neexistuje a je `null`? Použitím úvodzoviek sme vytvorili potenciálne ťažko odhaliteľnú chybu, pretože sa prepíše na prázdny reťazec. Chceli by sme však vrátiť skutočnú hodnotu `null`. Je rozdiel, ak záznam neexistuje alebo existuje, ale je prázdny.

Správne by teda bolo neuvádzať nič:

```php
return $row->hotel->name;
```

Vyžaduje funkcia vrátenie konkrétneho dátového typu a je v tomto smere striktná? Ak nemôžeme splniť špecifikáciu, mali by sme vyhodiť výnimku.

```php
function getHotelName(int $hotelId): string
{
   // implementácia

   if ($row->hotel->name === null) {
      throw new \Exception('Názov hotela neexistuje.');
   }

   return $row->hotel->name;
}
```

Nekonzistentnosť kódovania znakov a dátových typov
--------------------------------------------

Nezriedka sa stretávame s konštrukciami typu:

```php
echo "{$returnCode}" . self::CRLF;
```

Čo je lepšie napísať ako:

```php
echo $returnCode . "\n";
```

Použitie úvodzoviek okolo premennej je v tomto prípade zbytočné a len sťažuje čítanie, pretože ide o ďalšie zbytočné znaky. Obalovanie riadkov typu `CRLF` nie je moderné a je lepšie používať iba `\n`, ktoré je vlastné kódovaniu `UTF-8`.

Správne by sme ešte mohli overiť dátový typ pre kód. Napríklad, že ide o číslo a prípadne vrátiť náhradu. To možno vykonať pomocou operátora <a href="/ternary-operator">ternary</a>:

```php
echo (is_int($returnCode) ? $returnCode : '?') . "\n";
```

> Poznámka: Použitie funkcie `is_int()` svedčí o zlom návrhu aplikácie, pretože by sa nikdy nemalo stať, že nepoznáme dátový typ premennej.

Obalenie výstupného reťazca apostrofmi
---------------------------------------

V texte výnimky je často potrebné uzavrieť výstupný reťazec z premennej do úvodzoviek alebo apostrofov. Tým sa však zbytočne vytvárajú "odplaty" za oba typy ohraničenia reťazca, a ak reťazec začína a končí rovnakým znakom, nie je možné rýchlo jednoznačne určiť, kde reťazec začal a kde skončil v prípade viacerých reťazcov, ak sa vo vnútri objaví apostrof:

```php
throw new \Exception(__METHOD__ . ": Unsupported data type '{$fileType}'");
```

Ja osobne to riešim tak, že ho uzavriem do iného typu znaku (mne sa osvedčili hranaté zátvorky), aby bol elegantne viditeľný začiatok a koniec reťazca:

```php
throw new \Exception(__METHOD__ . ': Nepodporovaný dátový typ [' . $fileType . ']');
```

Alebo:

```php
throw new \Exception("Status '{$status}' is invalid");
```

Možno vymeniť za:

```php
throw new \Exception('Stav [' . $status . '] je neplatný');
```

Rozbor podľa riadkov
--------------------

Úvodzovky sú užitočné napríklad pri rozbore na nový riadok:

```php
foreach(explode("\n", $text) as $line) {
	// implementácia
}
```

Boli vytvorené špeciálne na tento účel.

Ak však potrebujete spracovať zložitejší dokument (napríklad zdrojový kód programovacieho jazyka alebo stránku HTML), odporúčam použiť **Tokenizer** spolu s <a href="/regex">pravidelnými výrazmi</a>.

Nekombinujte dve metódy uzavretia
-----------------------------------

Ak sa z nejakého dôvodu aj tak chystáte používať úvodzovky, aspoň ich udržujte konzistentné.

Ide o odstrašujúci prípad:

```php
throw new \Exception("Obrázok na adrese URL neexistuje. ResponseSize:" . strlen($result) . ')');
```

Kde začiatok reťazca je ohraničený úvodzovkami a koniec apostrofmi.

Táto chyba sa vyskytuje najmä pri používaní nástrojov na automatické formátovanie (ako napríklad **Code Sniffer**), ktoré sa snažia zjednotiť konvencie, ale nie vždy sa im to podarí.
