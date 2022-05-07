Ako porozumieť kódu PHP
=======================

> id: d8d452da-37b4-4502-a573-a27afe7ea37a
> slug:
> 	cs: jak-se-vyznat
> 	sk: ako-porozumiet-kodu-php
> 
> publicationDate: '2020-04-11 18:45:23'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: c3012a19e6dd01770fad9ab456a5b6b3

Väčšina jazykov sa dá zapísať rôznymi spôsobmi s rovnakým výsledkom. Zároveň platí, že keď už kód napíšete, pravdepodobne si ho niekedy v budúcnosti prečítate a opravíte ho alebo pridáte nové funkcie.

Preto, aby ste nemuseli neustále myslieť na kód a dobre sa v ňom orientovať, existuje súbor nástrojov a spôsobov, ako "správne písať kód" priamo v jazyku PHP, alebo ako vytvoriť kód spôsobom, ktorý priamo podporuje jeho budúcu čitateľnosť (aj iným človekom).

> **Poznámka autora:**
>
> Skúsenosti ukazujú, že kód zastaráva tak rýchlo, že aj samotný autor aplikácie po pol roku vníma svoj vlastný kód ako cudzí. Ak ho teda od začiatku napíšeme správne, nebude to brániť jeho rozšíreniu v budúcnosti.
>
> V reálnom vývoji sa tajne ukazuje, že jednotné formátovanie kódu a zavedenie pravidiel vo všeobecnosti zabraňuje množstvu chýb.

TL;DR
-----

Čitateľnosť kódu často súvisí s pravidlami formátovania a písania.

V rámci vývojových tímov má zmysel stanoviť formálne pravidlá pre formátovanie a údržbu kódu.

Osobne používam (v roku 2022) kódovací štandard pre rámec Nette a pravidlá sa vyhodnocujú automaticky v každej revízii. Viac informácií nájdete v článku o [používaní GitHub CI](https://php.baraja.cz/github-actions-nejlepsi-ci-pro-rok-2021#hotove-priklady).

Inštalácia štandardného testu kódovania a jeho spustenie sa vykonáva pomocou dvojice príkazov:

composer create-project nette/coding-standard temp/coding-standard ^3 --no-progress --ignore-platform-reqs
php temp/coding-standard/ecs check src

Poznámky v kóde
---------------

Poznámky nemajú žiadny vplyv na spracovanie kódu a slúžia len pre potreby programátora. V prípade väčších a úplnejších častí kódu je dôležité napísať poznámku, ktorá vysvetľuje, na čo kód slúži a ako v zásade funguje.

```php
// Definície premenných
$a = 5;
$b = 3;
$c = 2;

// Súčet všetkých čísel
$sum = $a + $b + $c;

// Zoznam pre používateľov
echo $sum;
```

Poznámka začína dvojicou lomiek (`//`) a platí až do konca riadku. Dá sa použiť kdekoľvek.

Poznámka by nemala vysvetľovať konkrétnu implementáciu algoritmu, ale skôr jeho všeobecné princípy. Dôvodom je, že kód sa môže v priebehu času viackrát zmeniť a v takom prípade by sme mali opraviť aj poznámku.

> **Poznámka autora:**
>
> Často sa stáva, že kód nerobí presne to, čo vysvetľuje jeho opis. Je to spôsobené najmä tým, že programátor niekde urobil chybu. V poznámke by preto mali byť opísané všeobecné zásady, aby sme mohli kód podľa nich upraviť. Nikdy však nezabúdajte, že jedinú pravdu o tom, čo sa v aplikácii skutočne deje, opisuje len skutočný kód a poznámka naň nemá žiadny vplyv.

Grafické oddelenie častí kódu
----------------------------

Pri návrhu aplikácie je dôležité oddeliť logické bloky od seba. Zvyčajne sú rozdelené do funkcií, metód alebo v prípade základného kódu aspoň do komentárov.

Pri dlhšom algoritme zvyčajne na začiatku najprv opíšem celý princíp algoritmu a potom očíslujem jednotlivé miesta v kóde, aby vývojár na ich základe lepšie pochopil konkrétnu funkcionalitu.

```php
/**
 * Funkcia vypočíta aritmetický priemer.
 *
 * 1. Získanie zoznamu čísel
 * 2. Získanie súčtu a počtu čísel
 * 3. Vypočítajte a vytlačte priemer
 */

// 1.
$numbers = [1, 3, 8, 12];

// 2.
$sum = array_sum($numbers);
$count = count($numbers);

// 3.
echo "Priemer je: . ($sum / $count);
```

Znaky `/**` začínajú viacriadkový komentár, ktorý sa aplikuje až po značku `*/`. Aby bol prehľadný, je dobré umiestniť na začiatok každého riadku hviezdičku.

Pripomienky k dokumentácii
----------------------

Dokumentačné komentáre sa zvyčajne používajú na popis a dokumentáciu funkcií (správanie, parametre, návratové hodnoty, autor atď.).

V predchádzajúcich verziách PHP (pred verziou `7.0`) sa ešte nepoužívali dátové typy, takže typ konkrétnej premennej bol popísaný priamo v komentári.

```php
/**
 * @autor Jan Barášek <jan@barasek.com>
 * @licencia MIT
 * @link https://php.baraja.cz
 * @param float[] $numbers
 */
function average(array $numbers): float
{
    $sum = array_sum($numbers);
    $count = count($numbers);

    return $sum / $count;
}
```

Dokumentačné komentáre sa nazývajú "dokumentácia" najmä preto, že majú vopred dohodnutý formát, ktorému rozumejú špecifické vývojové prostredia (a editory), ale aj automatizované nástroje na generovanie dokumentácie alebo kontrolu kódu.

Písať kód v češtine alebo angličtine?
-----------------------------

Všetok kód píšem len v angličtine (vrátane názvov funkcií, premenných, komentárov, ...).

Má to niekoľko výhod:

- Vývojár môže hneď aktívne trénovať svoju angličtinu.
- Veľká časť aplikácie používa knižnice tretích strán, ktoré sú v angličtine, takže automaticky udržiava konzistenciu
- Väčšina pokročilých vecí vôbec nemá anglický preklad
- Som si istý, že vás napadne mnoho ďalších príkladov.

PHP priamo nevyžaduje angličtinu a všetko môžete písať v angličtine. Používanie angličtiny vnímam skôr ako istý druh investície do budúcnosti a možnosť ľahko rozšíriť kód o ďalších ľudí, pre ktorých angličtina nie je rodným jazykom.

Vo firmách sa používa aj kompletne lokalizovaný kód v angličtine, preto je dobré precvičovať si angličtinu hneď od začiatku.

Poradie číselných operácií
------------------------

Pri vykonávaní číselných operácií majte vždy na pamäti, že PHP zaokrúhľuje čísla. To môže byť často nepríjemné, pretože každý výsledok s desatinnými číslami je sprevádzaný určitou nepresnosťou.

Dobrým riešením sa zdá byť najprv inkrementovať čísla a potom počítať s najväčšími možnými číslami. Týmto spôsobom dochádza k štatisticky menšiemu skresleniu.

Príklad:

```php
echo 10 / 3; // Píše 3,3333333333333
```

V niektorých prípadoch môžete použiť aj trik, pri ktorom vôbec nepoužívate desatinné čísla a všetko počítate ako celé číslo. V tomto prípade k takémuto skresleniu nedochádza:

```php
echo 1 / 2 * 2; // je to horšie, pretože 1/2 = 0,5*2 = 1
echo 2 * 1 / 2; // toto je lepšie, pretože 2*1 = 2/2 = 1
```

Pri riešení veľkých a zložitých číselných operácií používajte na zápis čísel zlomky.
