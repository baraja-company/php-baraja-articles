Výnimky a ich zachytávanie v jazyku PHP
=======================================

> id: '61b0f178-bb1c-4166-9e8a-af49de2e2a8c'
> slug:
> 	cs: vyjimky
> 	sk: vynimky-a-ich-zachytavanie-v-jazyku-php
> 
> publicationDate: '2020-02-16 22:18:18'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: d843fe11a092943db429cb8a28384a31

Výnimky sú nástrojom objektovo orientovaného programovania, ktoré poskytuje elegantný spôsob vyhadzovania a spracovania (ošetrenia) chýb aplikácie.

Výnimka je najprv vyhodená (`thrown`), spracovaná (`try`) a zachytená (`catch`). Povinné je len hádzanie.

Filozofia generovania výnimiek
-------------------------

Pred vznikom výnimiek bolo ošetrovanie chýb v programovaní veľmi komplikované, pretože sme sa museli spoliehať na návratové hodnoty funkcií, zachytávať ich vlastným spôsobom a podľa toho sa správať.

Samotné funkcie v skutočnosti nevynucujú spracovanie chýb, čo môže viesť k fatálnym problémom, ale David o tom písal v <a href="https://phpfashion.com/programatori-chyby-neignoruji">Programátori neignorujú chyby</a>.

Príklad zabudnutého spracovania chýb:

```php
// presun z disku na disk
copy('c:/oldfile', 'd:/newfile');
unlink('c:/oldfile');
// ak prvá operácia zlyhá, súbor sa nenávratne vymaže
```

Je to preto, že správny spôsob spracovania výstupu z funkcie `copy()` je nepokračovať a vyhodiť chybu. V prípade starých dobrých funkcií to môže vyzerať takto:

```php
function backup(): bool
{
   if (copy('c:/oldfile', 'd:/newfile')) {
      return unlink('c:/oldfile');
   }

   return false;
}
```

Naša funkcia `backup()` vráti `true` len vtedy, ak funkcia `copy()` nezlyhala a funkcia `unlink()` vrátila `true`. V opačnom prípade vráti hodnotu `false`.

Je to však teraz pre aplikáciu bezpečné? Nie je! Pretože teraz musíme spracovať výstup funkcie `backup()` v mieste, kde ju voláme, a ak zlyhá, nebudeme ani vedieť prečo. Skrátka, vráti `false` a my musíme chybu nejako zistiť sami. Myslím, že v tomto prípade je dobré vidieť, že programátori často rezignujú na ošetrovanie chýb alebo jednoducho zabudnú niečo ošetriť a aplikácia kvôli tomu vyhodí ťažko zistiteľné chyby.

Riešením tohto problému je použitie výnimiek, ktoré si vynútia ošetrenie, a ak nie sú ošetrené, aplikácia úplne spadne a vždy zistíme prečo.

Základná definícia výnimky
--------------------------

V PHP je výnimka špeciálny druh rozhrania implementovaný natívnou triedou `Exception`, ktorú budeme používať.

Ak spracovanie niektorej časti programu zlyhá, jednoducho vyhodíme výnimku popisujúcu problém:

```php
if (copy('c:/oldfile', 'd:/newfile') === false) {
   throw new \Exception('Nemožno skopírovať súbor "oldfile".');
}
```

Vyhodenie výnimky sa vykoná pomocou kľúčového slova `throw`, po ktorom nasleduje vytvorenie inštancie triedy s výnimkou. Inštanciu môžeme získať aj iným spôsobom (napríklad odovzdaním z premennej) a samotné vytvorenie inštancie výnimky nespôsobí jej vyhodenie.

Prvý argument konštruktora triedy `\Exception` prijíma text výnimky, ktorý by mal stručne vysvetliť, čo sa práve stalo. Vhodným postupom je uviesť aj informácie o vykonávanej operácii a odkaz na údaje. Ak napríklad zlyhalo kopírovanie súboru, je vhodné odovzdať názov súboru. Ak sa vykonanie dotazu SQL nepodarí, opäť odovzdáme vykonávaný dotaz. To nám neskôr veľmi pomôže pri riešení chýb, pretože presne vidíme, v čom je problém.

Spracovanie výnimiek
-----------------

Majme napríklad funkciu `backup()`, ktorá zálohuje dáta a môže vyhodiť pár chýb:

```php
function backup(): void
{
   if (copy('c:/oldfile', 'd:/newfile')) {
      if (unlink('c:/oldfile') === false) {
         throw new \Exception("Nemožno odstrániť starý súbor.);
      }
   }

   throw new \Exception("Nemožno kopírovať záložné súbory.);
}
```

Všimnite si, že funkcia nevracia žiadny výstup a v definícii sme uviedli typ `void`. Funkcia nemusí nič vracať, pretože za úspech sa považuje stav, keď nie je vyhodená žiadna chyba, a nepotrebujeme ošetrovať pozitívny scenár.

Ak by sme funkciu použili v aplikácii bez ošetrenia, napríklad takto:

```php
echo "Zálohovanie súborov...;
backup();
echo "Zálohovanie dokončené.;
```

Takto to funguje bežne. Ak však dôjde k chybe, skript sa automaticky ukončí a na výstupe sa zobrazí text výnimky. Dôležité je, že nebude pokračovať vo vykonávaní kódu a vieme, že nedôjde k poškodeniu údajov.

Ak chceme pokračovať vo vykonávaní, musíme chybu **odstrániť**, čo urobíme pomocou konštrukcií `try` a `catch`:

```php
echo "Zálohovanie súborov...;
try {
   backup();
} catch (\Exception $e) {
   echo 'Zálohovanie zlyhalo: ' . $e->getMessage();
}
echo "Zálohovanie dokončené.;
```

Ak je vyhodená výnimka, zavolá sa kód v oblasti `catch()` (ktorý akceptuje výnimku, ak zodpovedá jej dátovému typu) a vykoná sa vnútorný kód.

Vždy získame inštanciu triedy výnimiek, ktorú môžeme použiť napríklad na zobrazenie chybovej správy, ktorá sa spracúva metódou `getMessage()`. Užitočné je tiež poznať metódu `getFile()`, ktorá vracia diskovú cestu k súboru obsahujúcemu chybu, `getCode()`, ktorá vracia stavový kód chyby, a `getLine()`, ktorá vracia číslo riadku, na ktorom bola vyhodená výnimka.

Pripravené výnimky
------------------------

Okrem základnej výnimky `\Exception` obsahuje PHP aj ďalšie preddefinované typy výnimiek, ktoré sú vhodné pre rôzne prípady použitia.

| Typ údajov | Vysvetlenie |
|------------|-----------|
| `LogicException` | Logická chyba, predvídateľná pri návrhu programu |
| `BadFunctionCallException` | Chyba volania funkcie; funkcia nebola nájdená; volanie nie je povolené |
| `BadMethodCallException` | Chyba volania metódy |
| `InvalidArgumentException` | Zlý (neplatný) argument odovzdaný funkcii alebo metóde |
| `OutOfRangeException` | Hodnota mimo rozsahu poľa alebo kolekcie |
| `LengthException` | Hodnota prekračuje povolenú dĺžku |
| `DomainException` | Hodnota nepatrí do požadovanej domény alebo rozsahu |
| `RuntimeException` | Chyba zistiteľná len za behu (napríklad neúspešný zápis do súboru) |
| `OverflowException` | Preplnenie vyrovnávacej pamäte alebo aritmetickej operácie, často spôsobené spracovaním väčšieho množstva údajov, ako sa očakávalo |
| `UnderflowException` | Podpĺňanie vyrovnávacej pamäte alebo aritmetická operácia, bolo odovzdaných menej údajov, ako sa očakávalo |
| `OutOfBoundsException` | Index mimo rozsahu poľa alebo kolekcie |
| `RangeException` | Hodnota nie je v požadovanom rozsahu |
| `UnexpectedValueException` | Neočakávaná hodnota (napr. návratová hodnota funkcie) |

Výnimkám `LogicException` a `RuntimeException` by sa malo zabrániť správnym návrhom programu. Osobne ich používam len vo výnimočných situáciách, napríklad pri neúspešnom zápise do súboru a pri komunikácii s externou službou.

Odporúčam vôbec nezachytávať `RuntimeException` a nechať aplikáciu zlyhať. Zvyčajne ide o závažný problém, ktorý by ste mali čo najskôr nahlásiť.
