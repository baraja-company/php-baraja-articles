Funkcia PHP Explode - rozdelenie reťazca podľa oddeľovača
=========================================================

> id: '4dc7cec6-0e96-4a6b-aee8-32c8817ba11e'
> slug:
> 	cs: explode
> 	sk: funkcia-php-explode---rozdelenie-retazca-podla-oddelovaca
> 
> perex: Rozdělení řetězce na více částí podle oddělovače.
> publicationDate: '2019-11-26 11:39:36'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '66725772be4aae0df8af399e7ce2ba07'

Explode sa používa na jednoduché rozdelenie reťazca pomocou oddeľovača.

- Jednotlivé výsledky vráti ako pole očíslované od nuly,
- nemôžete vložiť pole, vstupom je len reťazec,
- nemôže zmeniť oddeľovač počas analyzovania, nemôže vybrať viacero oddeľovačov.

| Podpora | PHP 4 a novšie |
|---------------|-----------------|
| Stručný popis | Rozdelenie reťazca na pole pomocou oddeľovača.
| Požiadavky | Žiadne
| Poznámka | Nemožno vložiť pole, iba reťazec.

Často potrebujeme reťazec rozdeliť podľa nejakého jednoduchého pravidla. Napríklad zoznam čísel oddelených čiarkou.

Na tento účel je výborná funkcia explode(), ktorá ako prvý parameter berie oddeľovač (oddeľujúci reťazec) a ako druhý parameter samotné údaje:

```php
$cisla = '3, 1, 4, 1, 5, 9';
$parser = explode(', ', $cisla);

foreach ($parser as $cislo) {
	echo $cislo . '<br>';
	// Tu môžeme čísla ďalej spracovať
}
```

Ale čo ak sú čísla oddelené čiarkami, ale okolo nich sú medzery?

Riešením je analyzovať podľa najmenšieho spoločného podreťazca a potom odstrániť nežiaduce znaky okolo neho (medzery a iné biele znaky):

```php
$cisla = '3, 1,4, 1 , 5 , 9';
$parser = explode(',', $cisla);

foreach ($parser as $cislo) {
     echo trim($cislo) . '<br>';
     // Tu môžeme čísla ďalej spracovať
}
```

V tomto prípade funkcia `trim()` elegantne odstráni biele miesta okolo znakov (medzery, tabulátory, zalomenia riadkov, ...) a poskytne nám len čisté údaje.

Limit, ktorý obmedzuje počet analyzovaných entít do poľa
--------------------------

> TIP: Pre mnohé príklady nie je funkcia explode() vhodná a je oveľa lepšie použiť regulárne výrazy.

Často však chceme analyzovať údaje len do určitej vzdialenosti a na tento účel možno použiť tretí (nepovinný) parameter limit.

Majme napríklad štruktúrované údaje oddelené dvojbodkou, kde chceme získať obsah za prvou dvojbodkou a ignorovať ostatné dvojbodky.
Príklad:

```php
$cas = 'format: "j.n.Y - H:i"';
```

Ak by sme analyzovali reťazec ako:

```php
$parser = explode(':', $cas);
```

Získali by sme tieto 3 podreťazce (v iných prípadoch ich môže byť oveľa viac):

```php
'format'
' 'j.n.Y - H'
'i"'
```

Preto nastavíme limit, koľko prvkov sa má do parseru dostať (a prípadne všetky ostatné budú pripojené na koniec posledného prvku):

```php
$parser = explode(':', $cas, 2);

// vráti:
echo $parser[0]; // formát
echo $parser[1]; // "j.n.Y - H:i"
```

> **Poznámka:** Nežiaduce úvodzovky možno z reťazca odstrániť napríklad pomocou funkcie `trim()`:

```php
echo trim($parser[1], '"'); // druhý parameter určuje mapu znakov, ktoré sa majú odstrániť
```

Medzi, získanie reťazca medzi dvoma reťazcami
--------------------------

Často potrebujeme získať reťazec, ktorý je ohraničený dvoma inými reťazcami. Priamo v PHP na to neexistuje žiadna funkcia, ale môžeme si ju napísať sami:

```php
funkcia between(string $left, string $right, string $data): string
{
   $l = explode($left, $data);
   $r = explode($right, $l[1]);

   vrátiť $r[0];
}
```

Regulárne výrazy
--------------------------

Oveľa elegantnejšie delenie a prácu s reťazcami možno dosiahnuť pomocou <a href="/regex">regulárnych výrazov</a>, ktorým sa venujem na samostatnej stránke.

Podobné funkcie
--------------------------

- <a href="/function-implode">Implode()</a> - spojenie poľa do reťazca

Príklad rozboru IP adresy
--------------------------

```php
$ip = '10.0.0.138';
$parser = explode('.', $ip);
echo $parser[0]; // vypíše "10"
echo $parser[1]; // vypíše "0";
```

Premenná `$ip` obsahuje vstupný reťazec, ktorý je analyzovaný podľa oddeľovača `.`; návratom je pole. Rozbor pokračuje až do konca reťazca, pokiaľ nie je zadaný limit.

Parametre
--------------------------

| # | Typ | Popis
|---|--------|------|
| 1 | reťazec | Oddeľovací reťazec.
| 2 | reťazec | Parsovaný reťazec.
| 3 | int | limit pre parsovanie. Toto je nepovinný parameter. Príklad:

```php
$text = 'PHP je môj obľúbený jazyk!';
$parser = explode(' ', $text, 1);

echo $parser[0]; // vypíše prvé slovo
echo $parser[1]; // vypíše zvyšok textu
echo $parser[2]; // nevypíše nič, pretože bol nastavený limit!
```

Vrátené hodnoty
--------------------------

Návratovou hodnotou je pole s analyzovaným reťazcom.

Indexy sú číslované od nuly po `X`, pokiaľ nie je zadaný limit.

Rozdiely oproti predchádzajúcim verziám
--------------------------

| Verzia PHP | Popis |
|-----------|-------|
| 5.1.0 | Pridaná podpora záporného limitu na priechody.
| 4.0.1 | Pridaný voliteľný parameter **limit**.

Tipy a poznámky
--------------------------

Pri použití záporného **limit** je uvedený počet prvkov od konca reťazca.

Príklad:

```php
$str = 'jeden|dva|tri|štyri';

// kladná hranica
print_r(explode('|', $str, 2));

// záporný limit (od PHP 5.1)
print_r(explode('|', $str, -1));
```

Vrátia sa nasledujúce polia:

```php
Pole
(
    [0] => jeden
    [1] => dva|tri|štyri
)

Pole
(
    [0] => jeden
    [1] => dva
    [2] => tri
)
```
