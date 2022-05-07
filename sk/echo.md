Echo - výstup do zdrojového kódu
================================

> id: d6a1a7bf-03bf-49ea-9947-d4d6753ca6e4
> slug:
> 	cs: echo
> 	sk: echo---vystup-do-zdrojoveho-kodu
> 
> perex: Echo - výpis řetězce do stránky a na standardní výstup. Možnosti escapování a HTTP hlaviček.
> publicationDate: '2020-02-16 18:40:31'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '7d611691825ec537f58f387696d13e28'

Konštrukcia `echo` sa používa na vypisovanie premennej alebo reťazca do zdrojového kódu.

| Podpora: | Všetky verzie
|----------------|------
| Stručný popis: | Výstup jedného alebo viacerých reťazcov
| Typ: | <a href="/príkazy-a-funkcie">príkaz, konštrukcia</a> (nie funkcia)

Popis
-----

```php
echo 'hello world';
```

Hovorí "Hello world".

```php
$var = 'Text';
echo $var;
```

Vypíše hodnotu premennej `$var`, t. j. "Text".

**Echo** nie je funkcia (je to <a href="/príkazy-a-funkcie">príkaz</a>), takže môžete, ale nemusíte použiť zátvorku. Preto je správny aj zápis `echo ('hello world');`.

> **Dodatočná poznámka:** PHP považuje **Echo** za príkaz (konštrukciu), a preto ho považuje za výraz. V tomto prípade je zátvorka nepovinná. Ak uvedieme zápis: `echo ('niečo');`, príkaz **Echo** sa nestane funkciou a nebude sa s ním tak zaobchádzať. Zátvorka v tomto prípade znamená uzavretie presnej hodnoty výrazu, podobne ako to funguje v matematike.

Úvodzovky
--------

Reťazce môžu byť uzavreté v úvodzovkách a apostrofoch.

Takže toto:

```php
echo "Dobrý deň.";
```

Je to rovnaké ako toto:

```php
echo 'ahoj';
```

Pozor však na to, že každý reťazec musí začínať a končiť rovnakým typom znaku úvodzoviek a znak úvodzoviek nesmie byť v reťazci použitý.

Ak chcete napríklad vyvolať odkaz HTML (alebo akýkoľvek kód HTML), musíte pred úvodzovky umiestniť lomítko. Lomítko znamená "presne tento znak", takže sa v jazyku nechápe ako výraz.

```php
echo "<a href="index.php\">text odkazu</a>";
```

Technická poznámka: Uvádzacie značky majú v PHP <a href="/quotation-meaning">špeciálny význam</a>.

Parametre
---------

- `arg` Výstupný parameter.

Vrátené hodnoty
-----------------

Nevracia sa žiadna hodnota.

Nemožno použiť ako premennú.

Poznámka
--------

Poznámka: Keďže ide o **jazykovú konštrukciu** (konštrukcia = príkaz) (nie funkciu), nemožno ju načítať do premennej.

Príklad
-------

```php
echo "hello world";

echo "echo môže vypisovať viac riadkov textu.
Dajte si však pozor na značku HTML <br>, ktorá sa netlačí. Na to slúži funkcia nl2br().";

$a = "php";				// definícia premennej

echo "Páči sa mi " . $a;	// Píše: Páči sa mi php
```

**Echo** má tiež skrátenú syntax, v ktorej je možné použiť iba znak rovná sa za úvodným tagom php.

```php
Ahoj <?=$jmeno;?>!
```

To je užitočné, ak potrebujeme na stránku rýchlo zapísať nejaké informácie. Napríklad v aktuálnom roku:

```php
Píše Jan Barášek © <?=date('Y');?>
```

> Táto skrátená syntax bude fungovať len vtedy, ak sú povolené skrátené otváracie php tagy, t. j. ak je smernica `short_open_tag` nastavená na `on`.

Operácia
-------

Všetky bežné matematické operácie možno vykonať v rámci príkazu **echo**.

Podrobnú diskusiu o matematike nájdete v <a href="/matematika">samostatnom článku</a>.

```php
echo 5 + 3 * 2; // vytlačí 11
```
