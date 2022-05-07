GitHub Actions - najlepší CI na rok 2021
========================================

> id: c1fe3b77-a506-4f20-bf1e-90ee82817c7d
> slug:
> 	cs: github-actions-nejlepsi-ci-pro-rok-2021
> 	sk: github-actions---najlepsi-ci-na-rok-2021
> 
> perex: 'Protože už nějaký čas vyvíjíte webové aplikace, určitě jste si všimli, že se pro vás celá řada věcí rutinně opakuje, i když by nemusela. Velmi často jde o technickou správu projektů, verzování souborů, automatická kontrola kódu, zpracování různých dat, nebo třeba deploy na server a bezpečnostní věci kolem toho.'
> publicationDate: '2021-02-07 15:46:39'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: d3d151927a02dde744d1202449c904dc

Keďže už nejaký čas vyvíjate webové aplikácie, pravdepodobne ste si všimli, že mnohé veci sa pre vás bežne opakujú, aj keď by sa opakovať nemali. Veľmi často ide o technické riadenie projektov, verzovanie súborov, automatizovanú kontrolu kódu, rôzne spracovanie údajov, prípadne nasadenie na server a bezpečnostné záležitosti okolo toho.

Pri poradenstve vo firmách sa veľmi často stretávam s problémom, že prevencia je veľmi podceňovaná. Dôvodom je často to, že vývojári vnímajú niektoré veci ako veľmi náročné a že im to pridá prácu. Pravdou však je, že zvyčajne stačí nastaviť celé nastavenie len raz a potom už len využívať dlhodobé výhody.

Čo je CI (Continuous Integration)
----------

CI/CD je nástroj, ktorý dokáže automatizovať rutinné úlohy, ktoré majú podobný základ a v projektoch sa neustále opakujú. CI sa najčastejšie používa na spúšťanie automatizovaných testov, kontrolu kódu, automatizované nasadenie (nasadenie aplikácie na webový server), riadenie projektov alebo napríklad bezpečnostné audity.

CI si predstavte ako virtuálny operačný systém, ktorý vykonáva preddefinované príkazy terminálu alebo spúšťa konkrétne programy. Výstupom CI je buď úspech, alebo zlyhanie, ktoré sa zobrazuje priamo v projekte, ale posiela sa aj správcom, ktorí naň môžu reagovať. Dobrou praxou je spúšťať úlohy CI po konkrétnej udalosti (napríklad po odovzdaní do úložiska, prijatej požiadavke na zlúčenie/vyzdvihnutie, označení/verzii/vydaní alebo spustení časovej slučky cronu).

Ako konkrétne funguje CI
------------

Základom CI je konfiguračný súbor, v ktorom definujeme jeho správanie a reakcie na udalosti. V prípade služby GitHub stačí vytvoriť pomocný adresár `.github/workflows` a v ňom vytvoriť ľubovoľný súbor s príponou `.yml`. GitHub ho bude analyzovať pri každej revízii a podľa potreby ho vykoná.

Predstavme si, že chceme definovať nový konfiguračný súbor s konkrétnou úlohou. Keďže ide o úlohu, ktorú bude spúšťať priamo GitHub alebo iné prostredie vo virtuálnom počítači, musíme definovať základné veci o prostredí vrátane:

- Názov úlohy (napríklad názov testu)
- Spúšťacie udalosti (na základe ktorej udalosti sa má úloha spustiť, napríklad pri každom odoslaní do úložiska alebo prijatí žiadosti o stiahnutie)
- Definícia samotných úloh (môže existovať mnoho paralelne bežiacich úloh)

V prípade pracovných miest ďalej definujeme:

- Názov úlohy
- Spustené prostredie (zvyčajne názov operačného systému)
- Kroky na zostavenie kontajnera

Vyššie uvedený kontajner je už prvou prípravou na **úplnú dockerizáciu projektu**. Základné používanie CI je pomerne jednoduché a nemusíte vôbec rozumieť aplikácii Docker, stačí poznať príkazy v termináli.

V rámci konkrétnych krokov tejto úlohy musíme spustiť jednotlivé príkazy, ktoré vytvoria inštaláciu práve nainštalovaného operačného systému. V prípade PHP je teda dôležité nainštalovať len jazyk PHP (a určiť verziu), klonovať úložisko projektu do runnera, nainštalovať závislosti projektu (najčastejšie [Composer](/composer)) a spustiť samotný test.

Prečo používať GitHub Actions (CI)?
--------

V komunite IT je rozšírená povera, že Microsoft je zlá korporácia, ktorá vyrába nekvalitné veci. V prípade GitHub Actions to však vôbec neplatí, pretože majú objektívne najlepšiu platformu CI na svete.

Základnou výhodou GitHub CI je jednoduchosť a elegancia zápisu. Pomocou niekoľkých riadkov môžete nakonfigurovať vlastné testovacie prostredie a automaticky ho spravovať aj bez predchádzajúcich znalostí. Zároveň považujem rýchlosť spracovania konkrétnych úloh za obrovskú výhodu. Napríklad test na codestyle (formátovanie kódu) a PhpStan (kontrola dátových typov, logiky a všeobecnej správnosti) dokáže GitHub Actions vyhodnotiť na bežnom projekte za menej ako 50 sekúnd, zatiaľ čo napríklad GitLab vyhodnotí rovnaký test za takmer 8 minút! Iné riešenia ako Travis sú relatívne drahé a väčšina vývojárov aj tak neocení výhody ich špeciálnych funkcií.

Pripravené akcie
-----

GitHub má veľkú databázu hotových akcií a balíkov, ktoré môžete hneď použiť. Zvyčajne ide o operačné systémy, programovacie jazyky alebo knižnice z komunity.

Databázu akcií nájdete priamo v [Marketplace](https://github.com/marketplace?type=actions), napríklad moja obľúbená akcia je [Poslať SMS v prípade zlyhania cez Twillio](https://github.com/marketplace/actions/twilio-sms).

Hotové príklady
------

Veľmi veľa príkladov [zverejňujem v rámci svojich vlastných úložísk](https://github.com/baraja-core), kde môžete transparentne vidieť, akú konkrétnu konfiguráciu používam.

Napríklad vo februári 2021 som použil túto konfiguráciu na kontrolu štýlu kódu v projekte:

name: Coding Style

on:
  push:
    branches:
      - master
  pull_request:
    types: [ assigned, opened, synchronize, reopened ]
  schedule:
    - cron: '1 * * * *'

jobs:
  nette_cc:
    name: Nette Code Checker
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: shivammathur/setup-php@v2
        with:
          php-version: 7.4
          coverage: none

      - run: composer create-project nette/code-checker temp/code-checker ^3 --no-progress
      - run: php temp/code-checker/code-checker --strict-types --no-progress

  nette_cs:
    name: Nette Coding Standard
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: shivammathur/setup-php@v2
        with:
          php-version: 8.0
          coverage: none

      - run: composer create-project nette/coding-standard temp/coding-standard ^3 --no-progress --ignore-platform-reqs
      - run: php temp/coding-standard/ecs check src

Tento konfiguračný súbor nainštaluje najnovšie Ubuntu (`runs-on: ubuntu-latest`), klonuje úložisko projektu (`uses: actions/checkout@v2`), nainštaluje PHP (`uses: shivammathur/setup-php@v2`) vo verzii 7.4 (`php-version: 7.4`), nainštaluje balík code-checker a potom spustí úlohu prostredníctvom PHP.

Niekoľko ďalších poznámok pre menej skúsených používateľov:

- CI pri každom spustení spustí virtuálny počítač s kompletným operačným systémom, ktorý možno použiť na vyvolanie príkazov v termináli rovnako ako napríklad váš server.
- Na kontrolu výsledkov testu sa používajú takzvané návratové kódy, ktoré definuje samotný systém Linux. Konkrétne, keď program vráti `0` (nulu), znamená to úspech a akékoľvek iné číslo znamená neúspech. Napríklad skripty shellu pracujú rovnakým spôsobom
- Keď chceme napísať vlastný test alebo zložitejšiu logiku, môžeme použiť napríklad PHP a spustiť ho ako úlohu CLI (príkaz `php file.php`), ktorá sa spustí a môže robiť všetko, na čo má práva (pracovať so súbormi, komunikovať cez internet, vypisovať na obrazovku, ...)
- Počas behu CI sa všetky výstupy zaznamenávajú na obrazovku a možno ich zobraziť priamo vo webovom prehliadači.
- CI nie je interaktívny, hoci terminál môže vykonávať interaktívne operácie s používateľom, CI to nepodporuje a musí byť navrhnutý ako úloha, ktorá sa niekedy spustí a niekedy skončí
- Je definovaný časový limit 1 hodiny pre beh kontajnera CI, ak sa za tento čas nespracuje všetko (napríklad sa program zasekne), celý systém sa násilne ukončí a ohlási sa chyba
- GitHub môže spúšťať úlohy CI automaticky pomocou cronu, avšak veľmi často ich nespúšťa v správnom čase, ale spúšťa ich s oneskorením.
- Celú logiku CI môžete tiež zabaliť do kontajnera Docker a spustiť ju lokálne na všetkých počítačoch alebo na serveri, napríklad
- Ak potrebujete získať prístup k tajným informáciám (napr. heslo do databázy), existuje možnosť definovať tajné premenné priamo v službe GitHub a CI ich má k dispozícii počas behu
- Nasadenie na webový server je vždy najlepšie vykonať cez SSH
- Celkový čas behu úloh CI je obmedzený, zvyčajne na 2 tisíc minút mesačne (veľmi často vám to bude stačiť, nikdy som ho nevyčerpal, pretože jedna úloha beží maximálne jednu minútu), alebo si môžete čas za poplatok zvýšiť.
- Môžete tiež hostiť CI runnery na svojom serveri a obísť minútový limit, ale veľmi pravdepodobne váš server nebude oveľa rýchlejší, pretože spoločnosť Microsoft investovala veľké prostriedky do svojho Azure, kde je GitHub Actions umiestnený a beží na veľmi výkonných serveroch.
- Veľmi často sa hodí inštalovať špecifické balíky len pre CI (typicky PhpStan a rôzne bezpečnostné funkcie), preto ich umiestnite do časti `composer.json` súboru `require-dev`, aby sa neinštalovali do konkrétneho projektu ako povinná závislosť

Aplikácie a boti GitHubu
-----

Na rozdiel od iných hostiteľov úložísk podporuje GitHub takzvané aplikácie GitHub a automatizačné roboty. Ide o veľkú databázu pripravených botov, ktorí môžu vykonávať automatizované operácie na vašich projektoch (aj bez CI) a keď na niečo narazia, napríklad založia problém alebo pošlú žiadosť o opravu.

Osobne ho používam a odporúčam vám ho:

- [Dependabot](https://dependabot.com) - automatická kontrola závislostí v Composeri, napríklad keď vyjde nová verzia balíkov, ktoré používate a sú kompatibilné, automaticky odošle žiadosť o stiahnutie
- [Codecov](https://github.com/marketplace/codecov) - kontroluje pokrytie kódu pomocou automatických testov a zobrazuje grafy, ktoré časti kódu ešte neboli pokryté
- [Snyk](https://github.com/marketplace/snyk) - automaticky kontroluje zraniteľnosti zabezpečenia
- [Imgbot](https://github.com/apps/imgbot) - automatická optimalizácia veľkosti obrazových dát

Mnoho ďalších aplikácií nájdete na [Marketplace](https://github.com/marketplace?type=apps).

Ďalšie praktické tipy a triky
-----

Veľmi som si obľúbil používanie automatizačných úloh v službe GitHub.

Vo všetkých svojich repozitároch mám nastavené automatické kontroly, takže keď odošlem akúkoľvek revíziu (alebo ktokoľvek iný z komunity), dostanem v reálnom čase spätnú väzbu o tom, či bola porušená kvalita kódu (codestyle a PhpStan), bezpečnosť a ďalšie.

Niektoré testy sa spúšťajú automaticky každú hodinu. Ak sa napríklad vyskytne bezpečnostná chyba alebo CVE, dozviem sa o nej najneskôr do hodiny, a to aj na úrovni konkrétnych projektov a čo presne sa stalo.

Postupom času som po testovaní rôznych konfigurácií dospel k záveru, že za ideálne minimálne nastavenie považujem nasledujúce závislosti na Composeri:

- `phpstan/phpstan` - kontrola správnosti a logiky kódu
- `tracy/tracy` - hlásenie chýb a logovanie na vysokej úrovni
- `phpstan/phpstan-nette` - Rozšírenia a špeciality Phpstan v Nette
- `spaze/phpstan-disallowed-calls` - brilantná knižnica od Michala Špačka, ktorá rieši (ne)používanie nebezpečných vecí v kóde (od zabudnutých výpisov premenných až po možnosť spustiť používateľský reťazec ako príkaz shellu)
- `roave/security-advisories` - kontroluje inštaláciu nebezpečných verzií balíkov (ak sa v určitom balíku nájde bezpečnostná chyba alebo sa vydá CVE, tento balík zabráni inštalácii poškodenej verzie)

V ideálnom prípade by ste mali mať základné nastavenie zabezpečenia nastavené na automatické spúšťanie aspoň každých 8 hodín a odosielanie chýb na váš e-mail. Pretože vždy, keď zlyhá inštalácia projektu alebo sa objaví nová zraniteľnosť, chcem o tom vedieť čo najskôr a okamžite reagovať.

Nezabudnite, že všetko funguje automaticky, takže si vždy môžete byť istí, že aj keď používate zložité procesy na kontrolu zabezpečenia a iných vecí, nestojí vás to žiadne úsilie navyše a stačí dobre vykonaná počiatočná konfigurácia.

Pretože v prevencii a automatizácii je obrovský potenciál!
