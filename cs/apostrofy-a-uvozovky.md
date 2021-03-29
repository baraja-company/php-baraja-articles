Apostrofy a uvozovky
====================

> id: "526ad995-3412-446e-bb56-9627dff8e29e"
> slugCS: apostrofy-a-uvozovky
> perex: Použití uvozovek a apostrofů pro ohraničení řetězců v PHP. Escapování řetězců a řešení kontextů.
> publicationDate: "2019-08-22 20:48:46"
> mainCategoryId: "0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1"

Pro ohraničení řetězců je možné použít buď uvozovky nebo apostrofy. Já osobně preferuji pouze **apostrofy**, pokud nejde o speciální znak zalomení řádku nebo tabulátor.

Důvodů je celá řada, pojďme si je konstruktivně projít.

> Hlavní důvod, proč uvozovky nepoužívat, je bezpečnost a nevhodná práce s datovými typy.

Použití HTML tagů uvnitř řetězce
--------------------------------

Pokud potřebujeme vrátit v řetězci HTML kód a string obalujeme v uvozovkách, tak musíme dost nepříjemně escapovat:

```php
return "<a href=\"{$link}\">{$text}</a>";
```

Nebo to samé čitelněji se zachováním uvozovek bez escapování:

```php
return '<a href="' . $link . '">' . $text . '</a>';
```

Delší vizuální vzdálenost řetězce od proměnné
---------------------------------------------

Není nic horšího, než nalepené proměnné uvnitř stringu přímo na sobě, kdy není možné rychle vizuálně poznat, co je proměnná a co string:

```php
$url = "{$baseImageUrl}/{$dirName}/{$basename}.{$ext}";
```

Tento zápis nemá žádný negativní vliv na funkčnost, jen jsou jednotlivé proměnné a pevné znaky z řetězce nalepeny příliš na sobě, což zhoršuje čitelnost.

Zkusme to znovu a více čitelněji:

```php
$url = $baseImageUrl . '/' . $dirName . '/' . $basename . '.' . $ext;
```

Hlavní výhodu vnímám hlavně v čistotě kolem proměnných, kdy v názvu nepřekáží žádné zbytečné znaky.

Navíc se to bude i lépe zalamovat a ve stringu nevzniknou znaky zalomení ;)

```php
$url = $baseImageUrl
       . '/' . $dirName
       . '/' . $basename . '.' . $ext;
```

Uvnitř uvozovek není možné volat funkci
---------------------------------------

Ale proměnnou ano, proto se "něco" připojuje ven za řetězec (zejména funkce), ale proměnné se píší zas dovnitř. A někdy programátor i proměnnou připojí za řetězec. Zkrátka zmatek nad zmatek.

Proč nemůžeme věci dělat jednotně?

Nevhodné přetypování jiného datového typu na string
---------------------------------------------------

Mějme následující volání funkce:

```php
echo getFullName("{$user->name}");

function getFullName(string $name): string
{
	// ... implementace ...
}
```

Do uvozovek je možné vkládat proměnné, což způsobí jejich přetypování na string. Pokud je ovšem proměnná v řětezci sama a je jiného datového typu, než string, může dojít k poškození informace o původním datovém typu. Kdyby například konstrukce `$user->name` vracela `false` nebo `null`, tak bychom nedokázali poznat, že jde o chybu.

Aplikace by měla vždy rozpoznat chybový stav a správným způsobem selhat, než aby ji ignorovala. Správné hlášení chyb vede k lepšímu debuggování v budoucnu.

Občas se lze setkat i s přetypováním zejména z toho důvodu, že funkce vyžaduje určitý datový typ:

```php
trim("{$returnText}");
```

V tom případě se spíše přikláním k zápisu:

```php
trim ((string) $returnText);
```

který nepůsobí tolik "magicky" a je i pro méně zdatné uživatele zřejmé, co se proměnnou stalo.

Zahození hodnoty `null` z databáze
----------------------------------

Dejme tomu, že načítáme název hotelu z databáze:

```php
return "{$row->hotel->name}";
```

Co když ale název neexistuje a je `null`? Použitím uvozovek jsme vytvořili potenciálně těžko odhalitelnou chybu, protože dojde k přetypování na prázdný string. My bychom ale rádi vrátili skutečný `null`. Je totiž rozdíl, když záznam neexistuje, nebo existuje, ale je prázdný.

Správně by se tedy nemělo uvádět nic:

```php
return $row->hotel->name;
```

Vyžaduje funkce vrácení konkrétního datového typu a je v tomto striktní? Pokud nemůžeme vyhovět zadání, měli bychom vyhodit výjimku.

```php
function getHotelName(int $hotelId): string
{
   // implementace

   if ($row->hotel->name === null) {
      throw new \Exception('Hotel name does not exists.');
   }

   return $row->hotel->name;
}
```

Nekonzistence kódování znaků a datových typů
--------------------------------------------

Nezřídka se setkávám s konstrukcí ve smyslu:

```php
echo "{$returnCode}" . self::CRLF;
```

Kterou je lepší zapsat spíše jako:

```php
echo $returnCode . "\n";
```

Použití uvozovek kolem proměnné je v tomto případě zbytečné a akorát ztěžují čitelnost, protože jde o další zbytečné znaky navíc. Zároveň zalomování řádků typu `CRLF` není moderní a je lepší používat jen `\n`, které je nativní pro kódování `UTF-8`.

Správně bychom ještě mohli validovat datový typ pro kód. Například, že je číslo a případně vrátit náhradu. To jde <a href="/ternarni-operator">ternárním operátorem</a>:

```php
echo (is_int($returnCode) ? $returnCode : '?') . "\n";
```

> Poznámka: Použití funkce `is_int()` značí nekvalitní návrh aplikace, protože by se nikdy nemělo stát, že neznáme datový typ proměnné.

Obalení výstupního řetězce do apostrofů
---------------------------------------

Často je potřeba v textu výjimky obalit výstupní řetězec z proměnné do uvozovek nebo apostrofů. Zbytečně tím ovšem vzniká "vyplácání" obojího typu ohraničení řetězců, navíc pokud řetězec začíná i končí stejným znakem, tak není možné v případě více řetězců rychle jednoznačně určit, kde řetězec začal a kde skončil, pokud se uvnitř vyskytne apostrof:

```php
throw new \Exception(__METHOD__ . ": Unsupported data type '{$fileType}'");
```

Já osobně toto řeším ohraničením do jiného typu znaku (osvědčily se mi hranaté závorky), aby byl elegantně vidět začátek a konec řetězce:

```php
throw new \Exception(__METHOD__ . ': Unsupported data type [' . $fileType . ']');
```

Nebo:

```php
throw new \Exception("Status '{$status}' is invalid");
```

Lze vyměnit za:

```php
throw new \Exception('Status [' . $status . '] is invalid');
```

Parsování po řádcích
--------------------

Uvozovky jsou vhodné například pro parsování podle nového řádku:

```php
foreach(explode("\n", $text) as $line) {
	// implementace
}
```

Pro tento účel byly speciálně stvořeny.

Nicméně pokud potřebujete zpracovat složitější dokument (například zdrojový kód programovacího jazyka nebo HTML stránku), tak doporučuji používat **Tokenizér** společně s <a href="/regex">regulárními výrazy</a>.

Nekombinujte dva způsoby ohraničení
-----------------------------------

Pokud už z nějakého důvodu uvozovky stejně použijete, tak je aspoň zachovejte v rámci jednotnosti všude.

Toto je odstrašující případ:

```php
throw new \Exception("Image on URL does not exist. ResponseSize: " . strlen($result) . ')');
```

Kde začátek řetězce ohraničují uvozovky a na konci zase apostrofy.

K tomuto defektu dochází zejména při použití automaticky formátujících nástrojů (například **Code Sniffer**), které se snaží konvence sjednocovat, ale ne vždy se to podaří.
