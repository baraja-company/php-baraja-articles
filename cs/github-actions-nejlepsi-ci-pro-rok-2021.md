GitHub Actions - nejlepší CI pro rok 2021
=========================================

> id: c1fe3b77-a506-4f20-bf1e-90ee82817c7d
> slugCS: github-actions-nejlepsi-ci-pro-rok-2021
> perex: "Protože už nějaký čas vyvíjíte webové aplikace, určitě jste si všimli, že se pro vás celá řada věcí rutinně opakuje, i když by nemusela. Velmi často jde o technickou správu projektů, verzování souborů, automatická kontrola kódu, zpracování různých dat, nebo třeba deploy na server a bezpečnostní věci kolem toho."
> publicationDate: "2021-02-07 15:46:39"
> mainCategoryId: "3666a8a6-f2a3-405d-8263-bd53c4301fb3"

Protože už nějaký čas vyvíjíte webové aplikace, určitě jste si všimli, že se pro vás celá řada věcí rutinně opakuje, i když by nemusela. Velmi často jde o technickou správu projektů, verzování souborů, automatická kontrola kódu, zpracování různých dat, nebo třeba deploy na server a bezpečnostní věci kolem toho.

Při konzultacích ve firmách velmi často narážím na problém, kdy se velmi podceňuje prevence. Důvod bývá často ten, vývojáři vnímají některé věci jako velmi náročné a že jim to přidá práci. Pravda je ale taková, že celý setup stačí většinou nastavit jen jednou a pak jen dlouhodobě těžit z výhod, které to přináší.

Co je CI (Continuous Integration)
----------

CI/CD je nástroj, který umí automatizovat rutinní úkoly, které mají podobný základ a stále se v projektech opakují. Nejčastější použití CI je pro běh automatických testů, kontrolu kódu, automatické deploye (nasazení aplikace na webový server), správa projektu, nebo třeba bezpečnostní audity.

CI si představte jako virtuálně operační systém, který vykonává předem dané příkazy do Terminálu, nebo spouští konkrétní programy. Výstupem CI je buď úspěch (success), nebo chyba (fail), která se zobrazí přímo v projektu, ale také odešle správcům, kteří na to mohou reagovat. Dobrým zvykem je spouštět CI úlohy v návaznosti na určitou událost (například commit do repositáře, přijatý merge request / pull request, vydání tagu / verze / releasu, nebo spouštění v časové smyčce cronem).

Jak CI konkrétně funguje
------------

Základem CI je konfigurační soubor, ve kterém definujeme jeho chování a reakci na události. V případě GitHubu stačí vytvořit pomocný adresář `.github/workflows` a uvnitř vytvořit libovolný soubor s příponou `.yml`. GitHub ho bude s každým commitem analyzovat a podle potřeby spouštět.

Představme si, že chceme definovat nový konfigurační soubor s konkrétní úlohou. Protože jde o úlohu, kterou bude spouštět přímo GitHub nebo jiné prostředí na virtuálním počítači, tak musíme definovat základní věci ohledně prostředí, mezi to patří:

- Název úlohy (například název testu)
- Vyvolávací události (na základě které události se mají úlohy spouštět, například při každém pushnutí do repositáře nebo přijetí pull requestu)
- Definice samotných úloh (anglicky `runner`), (může jich být mnoho a víc běžet paralelně)

V případě úloh (job) pak dále definujeme:

- Název úlohy
- Běhové prostředí (typicky název operačního systému)
- Kroky pro sestavení kontejneru

Uvedený kontejner je už první příprava na **kompletní dokerizaci projektu**. Základní použití CI je relativně snadné a nemusíte Dockeru vůbec rozumět, stačí znalost příkazů v Terminálu.

V rámci konkrétních kroků v úloze musíme spustit jednotlivé příkazy, které sestaví instalaci právě nainstalovaného operačního systému. V případě jazyka PHP tedy bude důležité nainstalovat právě jazyk PHP (a specifikovat verzi), naklonovat repositář s projektem do runneru, instalace projektových závislostí (nejčastěji [Composerem](/composer)) a spuštění samotného testu.

Proč používat GitHub Actions (CI)?
--------

Obecně v IT komunitě koluje pověra, že je Microsoft ten zlý korporát, který dělá nekvalitní věci. V případě GitHub Actions to ale není vůbec pravda, protože mají naprosto objektivně suveréně nejlepší CI platformu na světě.

Zásadní výhoda GitHub CI je jednoduchost a elegance zápisu. Pomocí několika málo řádků zvládnete sami i bez předchozích znalostí nakonfigurovat vlastní testovací prostředí a to automatizovaně spravovat. Zároveň jako obrovskou výhodu vnímám rychlost zpracování konkrétních úloh. Například test na codestyle (formátování kódu) a PhpStan (kontrola datových typů, logiky a obecné správnosti) zvládne GitHub Actions na běžném projektu vyhodnotit pod 50 sekund, zatímco například GitLab stejný test vyhodnocuje téměř 8 minut! Ostatní řešení typu Travis jsou relativně drahé a většina vývojářů užitek jejich speciálních funkcí stejně nedokáže ocenit.

Připravené akce
-----

GitHub obsahuje velkou databázi hotových akcí a balíčků, které můžete rovnou použít. Typicky jde o operační systémy, programovací jazyky, nebo knihovny od komunity.

Databázi akcí najdete přímo v [Marketplace](https://github.com/marketplace?type=actions), například moje oblíbená akce je [Odeslání SMS v případě selhání přes Twillio](https://github.com/marketplace/actions/twilio-sms).

Hotové příklady
------

Velmi mnoho příkladů [zveřejňuji v rámci vlastních repositářů](https://github.com/baraja-core) kde můžete transparentně vidět, jakou konkrétní konfiguraci používám.

V únoru 2021 jsem například pro kontrolu codestyle v rámci projektu používal tuto konfiguraci:

```yaml
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
```

Tento konfigurační soubor nainstaluje nejnovější Ubuntu (`runs-on: ubuntu-latest`), naklonuje repositář s projektem (`uses: actions/checkout@v2`), nainstaluje PHP (`uses: shivammathur/setup-php@v2`) ve verzi 7.4 (`php-version: 7.4`), nainstaluje balíček s code-checkerem a poté přes PHP spustí úlohu.

Ještě pár poznámek pro uživatele, kteří nemají tolik zkušeností:

- CI při každém spuštění nastartuje virtuální počítač s kompletním operačním systémem, na kterým lze volat příkazy v Terminálu stejně, jako například u vás na serveru
- Pro kontrolu výsledků testu se používají tzv. návratové kódy tak, jak je definuje přímo Linux. Konkrétně to je tak, že když program vrátí hodnotu `0` (nula), tak to znamená úspěch a jakékoli jiné číslo neúspěch. Stejným způsobem fungují například shellové scripty
- Když chceme napsat vlastní test nebo složitější logiku, můžeme k tomu použít například PHP a ten spustit jako CLI úlohu (příkaz `php soubor.php`), který se spustí a může dělat cokoli, na co má práva (pracovat se soubory, komunikovat po internetu, vypisovat na obrazovku, ...)
- Při běhu CI se zaznamenává veškerý výstup na obrazovku a můžete ho vidět přímo ve webovém prohlížeči
- CI není interaktivní, i přesto, že Terminál umí spouštět interaktivní operace s uživatelem, tak CI toto nepodporuje a musí být designován jako úloha, která se někdy spustí a někdy skončí
- Pro běh CI kontejneru je definován timeout 1 hodina, pokud se za tu dobu nestine vše zpracovat (například se program zacyklí), bude celý systém násilně ukončen a zahlásena chyba
- GitHub umí CI úlohy spouštět automaticky cronem, nicméně velmi často je nespustí ve správný čas, ale běží se zpožděním
- Celou CI logiku lze zabalit také do Docker kontejneru a spouštět ji například lokálně na všem počítači nebo na serveru
- Pokud potřebujete přistupovat k tajným informacím (například heslo k databázi), existuje možnost definovat tzv. secret proměnné přímo v GitHubu a CI je má v době běhu k dispozici
- Deploy na webový server je dobré řešit vždy přes SSH
- Celkový součet běhu CI úloh je omezen, obvykle na 2 tisíce minut měsíčně (velmi často vám to bude stačit, nikdy jsem to nevyčerpal, protože jedna úloha běží maximálně minutu), případně je možné čas za poplatek navýšit
- CI runnery můžete hostovat také na vašem serveru a obejít omezení na počet minut, velmi pravděpodobně ale váš server nebude o moc rychlejší, protože Microsoft velmi investoval do svého Azure, kde GitHub Actions hostuje a běží to na velmi výkonných serverech
- Velmi často se hodí instalovat konkrétní balíky jen kvůli CI (typicky PhpStan a různé bezpečnostní funkce), proto je v souboru `composer.json` uveďte do sekce `require-dev`, aby se neinstalovali v konkrétním projektu jako povinná závislost

GitHub aplikace a roboti
-----

GitHub na rozdíl od jiných hostingů pro repositáře podporuje tzv. GitHub Apps a automatizační roboty. Jde o velkou databázi připravených robotů, kteří umí automaticky (i bez CI) provádět automatické operace s vašimi projekty a když na něco narazí, tak například založí issue nebo pošlou pull request s opravou.

Já osobně používám a vám doporučuji:

- [Dependabot](https://github.com/marketplace/dependabot-preview) - automatická kontrola závislostí například v Composeru, když vyjde nová verze balíků, které používáte a je kompatibilní, automaticky pošle pull request
- [Codecov](https://github.com/marketplace/codecov) - kontroluje pokrytí kódu automatickými testy a grafiky vykresluje, které části kódu nebyly ještě pokryty
- [Snyk](https://github.com/marketplace/snyk) - automatická kontrola bezpečnostních zranitelností
- [Imgbot](https://github.com/apps/imgbot) - automatická optimalizace datové velikosti obrázků

Mnoho dalších aplikací najdete v [Marketplace](https://github.com/marketplace?type=apps).

Další praktické tipy a triky
-----

Použití automatizačních úloh v GitHubu jsem si mimořádně oblíbil.

Ve všech repositářích mám nastavené automatické kontroly, proto když pošlu libovolný commit (nebo kdokoli jiný z komunity), tak mám v reálném čase zpětnou vazbu, jestli nebyla porušena kvalita kódu (codestyle a PhpStan), bezpečnost a další.

Některé testy nechávám spouštět každou hodinu automaticky. Pokud by se například objevila bezpečnostní zranitelnost nebo CVE, tak o tom budu nejpozději za hodinu vědět, a to dokonce na úrovni konkrétních projektů a co přesně se stalo.

Časem jsem po testování různých konfigurací došel k názoru, že za ideální minimální setup považuji tyto závislosti do Composeru:

- `phpstan/phpstan` - kontrola správnosti a logiky kódu
- `tracy/tracy` - reporting a logování chyb na vysoké úrovni
- `phpstan/phpstan-nette` - rozšíření PhpStanu a speciality v Nette
- `spaze/phpstan-disallowed-calls` - geniální knihovna od Michala Špačka, která ošetřuje (ne)použití nebezpečných věcí v kódu (od zapomenutých dumpů proměnných, až po možnost spustit uživatelský string jako shellový příkaz)
- `roave/security-advisories` - kontrola instalace nebezpečných verzí balíků (pokud se objeví bezpečností zranitelnost v konkrétním balíku nebo vyjde CVE, tento balík zabrání v instalaci poškozené verze)

Ideální je mít u základního bezpečnostního setupu nastavené automatické spouštění aspoň každých 8 hodin a nechávat si posílat chyby na mail. Kdykoli totiž instalace projektu selže, nebo se objeví nová zranitelnost, chci o tom vědět co nejdřív a hned reagovat.

Nezapomeňte, že všechno funguje automaticky a tak máte vždy jistotu, že i když používáte komplikované procesy pro kontrolu bezpečnosti i dalších věcí, tak vás to nestojí žádné úsilí navíc a stačí jen dobře provedená úvodní konfigurace.

V prevenci a automatizaci je totiž obrovský potenciál!
