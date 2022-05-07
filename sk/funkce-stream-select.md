PHP funkcia stream_select()
===========================

> id: '5dd24afa-fcf3-4ce5-8ee4-f8267d7efb02'
> slug:
> 	cs: funkce-stream-select
> 	sk: php-funkcia-stream-select
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '5abf2fabc02959108b26155d6e57db78'

Dostupnosť vo verziách: `PHP 4.3.0`

Spustí ekvivalent systémového volania select() na danom
polia tokov s časovým limitom určeným tv_sec a tv_usec


Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$read` | `array` | *not* | Prúdy uvedené v poli read sa budú sledovať, či sa znaky uvoľnia na čítanie (presnejšie, či sa čítanie nezablokuje - najmä zdroj prúdu je pripravený aj na koniec súboru, v takom prípade fread vráti reťazec nulovej dĺžky) |
| `$write` | `array` | *not* | Toky uvedené v poli zápisu budú sledované, aby sa zistilo, či zápis nebude blokovaný. |
| `$except` | `array` | *not* | Toky uvedené v poli except budú sledované v prípade príchodu výnimočných dát s vysokou prioritou ("out-of-band"). |
| `$tv_sec` | `int` | *not* | Parametre tv_sec a tv_usec spolu tvoria časový limit, tv_sec udáva počet sekúnd, kým tv_usec počet mikrosekúnd. Časový limit je horná hranica času, ktorý bude stream_select čakať, kým sa vráti. Ak sú tv_sec a tv_usec nastavené na 0, stream_select nebude čakať na údaje - namiesto toho sa okamžite vráti a uvedie aktuálny stav tokov.
| `$tv_usec` | `int` | null | Pozri popis tv_sec. |


Vrátené hodnoty
----------------

`int`

V prípade úspechu vráti stream_select počet
zdroje prúdu obsiahnuté v upravených poliach, ktoré môžu byť nulové, ak
časový limit vyprší skôr, ako sa stane niečo zaujímavé. Pri chybe false
je vrátené a je zobrazené varovanie (to sa môže stať, ak je systémové volanie
prerušené prichádzajúcim signálom).

Ďalšie zdroje
------------

[Oficiálna dokumentácia k výberu prúdu](https://www.php.net/manual/en/function.stream-select.php)
