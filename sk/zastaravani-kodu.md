Zastarávanie kódu - ako zachovať kompatibilitu
==============================================

> id: '7837ee7e-a150-47bf-a338-2f4c03d5bbed'
> slug:
> 	cs: zastaravani-kodu
> 	sk: zastaravanie-kodu-ako-zachovat-kompatibilitu
> 
> publicationDate: '2022-07-29 17:10:00'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '727b60c7258fb70e21b5acb445404db0'

Pri vývoji rozsiahlych systémov (napr. podnikových aplikácií, zdieľaných softvérových balíkov, knižníc, ...), kde navzájom komunikuje viacero vrstiev a vývojárov, vzniká problém, ako riešiť vydávanie nových verzií kódu.

Pozrime sa na príklad situácie, keď chceme vytvoriť zdieľaný balík Composer pre komunitu vývojárov.

Sémantické verzovanie
--------------------

Pred vyriešením problému spätnej a doprednej kompatibility musíme zistiť, ako sledovať zmeny softvéru. V súčasnosti (2022) je najlepším spôsobom verzovania všetkých zmien systém Git. Úložisko softvéru možno zdieľať napríklad prostredníctvom GitHubu alebo GitLabu. Každá zmena softvéru má jedinečný identifikátor, ktorý identifikuje každú revíziu a opisuje, čo sa skutočne stalo.

Pri vývoji knižníc sa mi osvedčila nasledujúca stratégia:

Na začiatku vývoja sa vytvorí počiatočná revízia vo vetve `master` (alebo `main`), kde sa odovzdá základná štruktúra súborov.

Pre každú novú požiadavku sa vytvorí samostatná vetva z master, v ktorej sa bude pracovať. Keď je zmena pripravená, odošle sa požiadavka na zlúčenie do hlavnej zložky vo forme `Pull request`. Nad požiadavkou sa vykoná kontrola kódu a ak je všetko v poriadku, zmena sa zlúči do hlavnej verzie.

Ak vetva obsahuje spätne nekompatibilnú zmenu (BC break, od `Back Compatibility Break`), musí byť príslušne označená. Metóda označovania prestávok BC sa rozoberá v nasledujúcich kapitolách.

Produkčná verzia knižnice sa potom označí pomocou značiek, ktoré majú nasledujúcu štruktúru (na základe **Semantic Versioning 2.0.0**):

Číslo verzie zapisujeme vo formáte `MAJOR.MINOR.PATCH`. Zvyšovanie čísel verzií sa vykonáva takto:

- `MAJOR` - ak ide o zmenu, ktorá nie je spätne kompatibilná s ostatnými (API)
- `MINOR` - keď sa pridáva funkčnosť pri zachovaní spätnej kompatibility
- `PATCH` - keď je opravená chyba a zachovaná spätná kompatibilita

Pomocou predbežných verzií a pridaním metadát je možné spresniť informácie. Napríklad: `1.0.0-alpha`, `1.0.1-beta+2`.

Viac informácií o sémantickom verziovaní nájdete na oficiálnej webovej lokalite: https://semver.org.

Spätná a dopredná kompatibilita
-------------------------------

Pri navrhovaní softvéru by ste mali vždy myslieť na **spätnú kompatibilitu** (nové funkcie a zmeny musia byť kompatibilné so starým kódom) a v niektorých prípadoch aj na **priamu kompatibilitu** (súčasné funkcie musia byť kompatibilné s budúcimi zmenami rozhrania).

Správne zvládnutie oboch úloh je veľmi náročné. Nie vždy je možné vykonať zmenu bez porušenia kompatibility.

Pri vykonávaní zmien by ste mali vždy postupovať postupne a poskytnúť používateľom dostatok času na reakciu na zmeny.

Nasledujúce časti opisujú, ako o tom premýšľať.

Fáza 1: Označenie funkcie ako zastaranej
--------------------------------------

Základným typom hrozby kompatibility je odstránenie alebo premenovanie funkcie, ktorá existovala v minulosti. Najčastejšie je to preto, že sa zmenili argumenty, ktoré funkcia prijíma, alebo ide o starú logiku, ktorá by sa mala novým spôsobom spracovať inak.

V prvej fáze by sa staré časti kódu mali označiť ako zastarané, ale nemali by sa nijako meniť.

V PHP je na to určená anotácia `@deprecated`, ktorá by sa mala písať priamo nad metódy, funkcie, vlastnosti, premenné, konštanty a všeobecne všetok zastaraný kód.

Dobrým zvykom je tiež napísať dôvod, prečo je daná vec zastaraná a ako sa v budúcnosti zmení. Uveďte napríklad názov novej funkcie alebo spôsobu použitia.

Reálny príklad označenia kódu ako zastaraného: Konštanty budú odstránené, je lepšie použiť vstavaný Enum (BC break kvôli prechodu na novšiu verziu PHP):

```php
class OrderNotification
{
	/** @odstránené od 2022-05-24, použite enum OrderNotificationType */
	public const
		TYPE_EMAIL = 'e-mail',
		TYPE_SMS = 'text';
```

Anotácia `@deprecated` spôsobí len tiché varovanie pre IDE (vývojový nástroj) a kompilačné nástroje. Nič sa tým neporušuje.

Fáza 2: Volanie novej metódy/logiky
--------------------------------------

V druhej fáze nahradíme starú implementáciu novou, ale novú metódu použijeme v starej implementácii. Pomôže to zachovať kompatibilitu rozhrania bez toho, aby si to používateľ všimol.

Príklad: Metóda je zastaraná, pretože namiesto nej bola vytvorená nová statická služba. Keďže ju niekto môže používať, je len označená ako zastaraná a interne volá novú implementáciu. Vývojár môže vo všeobecnosti predpokladať, že metóda bude v budúcnosti úplne odstránená.

```php
/** @odstránené od 2021-09-11 namiesto toho použite Ip::get(). */
public static function userIp(): string
{
	return Ip::get();
}
```

Fáza 3: Zmena anotácií pre statickú analýzu
-------------------------------------------

Ak používate statickú analýzu, ako je PhpStan (odporúčame!), je dobré najprv prepísať anotácie PHPDoc a až potom zmeniť dátové typy. Statická analýza upozorní používateľa, že je niečo pokazené, ale runtime zostane nedotknutý.

Fáza 4: Odhodenie oznámenia
-----------------------

Vo štvrtej fáze sa zavolá nová metóda a zároveň sa vyhodí chyba na úrovni `note`. Aplikácia stále funguje, len sa postupne začne ukladať do systémového protokolu informácia, že funkcia je zastaraná a bude zmenená alebo odstránená. Na tento typ zmien budeme teraz aktívne upozorňovať. Vývojár uvidí chyby počas vývoja alebo kompilácie.

```php
/** @zrušené od 2021-05-01, namiesto toho používajte UserMetaManager. */
public function getMeta(int $userId, string $key): ?string
{
	trigger_error(__METHOD__ . ': Táto metóda je zastaraná, namiesto nej použite UserMetaManager.');
	return $this->userMetaManager->get($userId, $key);
}
```

Fáza 5: Vyhodenie výnimky
------------------------

Pred úplným odstránením metódy odporúčam vyhodiť jednu z fatálnych výnimiek. Je to obzvlášť dôležité, pretože aplikácia sa úplne zastaví a chybu nemožno ignorovať. Na rozdiel od úplného odstránenia kódu bude používateľ informovaný o tom, čo sa vlastne stalo, a môže chybu ľahko opraviť.

Fáza 6: Úplné odstránenie kódu
-----------------------------

V poslednej fáze sa starý kód úplne odstráni. Ak niektorý používateľ neopravil závislosti, jeho aplikácia bude nefunkčná.

Závažné porušenia BC v citlivých oblastiach by sa mali vždy vykonať v nasledujúcej `MAJOR` verzii a malo by sa na ne upozorniť aspoň o jednu `MAJOR` verziu skôr vhodením upozornenia. Ak to neurobíte, aktualizácia knižnice bude veľmi náročná.
