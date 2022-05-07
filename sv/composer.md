Composer - fullständig översikt över avancerade funktioner
==========================================================

> id: a74d8d59-91ce-4602-ad52-80cf89a647bd
> slug:
> 	cs: composer
> 	sv: composer---fullstaendig-oeversikt-oever-avancerade-funktioner
> 
> perex:
> 	- Composer je pokročilý správce balíků a závislostí pro vaše PHP aplikace. Článek popisuje jeho všechny výhody a možnosti použití.
> 	- Composer är en avancerad paket- och beroendehanterare för dina PHP-program. I den här artikeln beskrivs alla dess fördelar och användningsområden.
> 
> publicationDate: '2020-03-10 20:18:19'
> mainCategoryId: '4f1d7d70-c5b0-45f1-b1d2-d03c22aa4154'
> sourceContentHash: '68340d4b4d3c8a6ed143ede176fbf04e'

Som du redan vet är [Composer] (https://getcomposer.org/) en robust paket- och beroendehanterare för PHP, med vilken du elegant kan hantera hundratals projekt samtidigt och distribuera kod som skrivits till alla program samtidigt.

Den här handledningen fungerar som en detaljerad och omfattande guide för utvecklare. Vi tar upp alla viktiga avancerade tekniker för att arbeta med Composer, samt förklarar de tekniska detaljerna och relevanta beroenden.

Installera Composer
-------------------

Oavsett plattform laddar vi ner från [den officiella webbplatsen för Composer] (https://getcomposer.org/).

Internt hämtas PHP-filen `composer-setup.php`, som när den körs i CLI-läge kan installera Composer. Det är också viktigt att veta att Composer inte fungerar utan PHP, så kontrollera först att du har PHP igång på din dator (det behöver bara vara tillgängligt för Terminal).

På Mac och Linux fungerar Composer direkt efter installationen och du behöver bara använda kommandot `composer -v` för att snabbt kontrollera att Composer är korrekt installerat.

På Linux kan följande kommando användas för att installera: `/usr/bin/php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"`

På Windows är det en bra idé att installera verktyget [Git Bash for Windows] (https://gitforwindows.org/), som gör att du kan öppna en särskild terminal som beter sig nästan som Linux och låter dig arbeta i samma miljö som i Linux.

Installationen för servrar är densamma som i en lokal miljö. Se bara till att du har rätt version av PHP som Composer använder internt.

Tillgängliga kommandon
----------------

Composer innehåller ett antal kommandon.

Användningen är: `composer <kommando>`, till exempel: `composer update`.

Översikt för `1.10.0`:

| Kommando | Beskrivning / Betydelse |
|-----------------------|----------------|
| `about` | Visar kort information om Composer. |
| `archive` | Skapar ett arkiv med innehållet i det valda Composer-paketet. |
| `browse` | Öppnar startsidan för det valda paketet, författaren eller annan relaterad sida i en webbläsare. Ofta kan det finnas dokumentation om hur man använder den. |
| `cc` | Rensar den interna Composer-cachen med versioner av paket som laddats ner tidigare. |
| `check-platform-reqs` | Kontrollera om installationskraven är uppfyllda för den aktuella plattformen. |
| `clear-cache` | Rensa den interna Composer-cachen. |
| `clearcache` | Rensa Composers interna cacheminne. |
| | `config` | Ställer in ett konfigurationsdirektiv.
| `create-project` | Skapar ett nytt projekt baserat på det valda paketet och skapar automatiskt en mapp att placera projektet i.
| `depends` | Visar vilka paket som orsakade att det valda paketet installerades. |
| `diagnose` | Diagnostiserar systemet för att identifiera vanliga fel. Det är upp till utvecklaren att bearbeta resultatet, det är bara en lista. |
| `dump-autoload` | Genererar en ny <a href="/autoloading-trid">autoloader</a>. |
| `dumpautoload` | Genererar en ny <a href="/autoloading-trid">autoloader</a>. |
| `exec` | Kör binärer och skript från leverantören. |
| `fund` | Utforskar hur du gör/ändrar dina beroenden. |
| `global` | Gör att du kan köra globala Composer-kommandon från variabeln `$COMPOSER_HOME`. |
| `help` | Skriver ut hjälp för kommandon. |
| `home` | Öppnar hemsidan för ett visst paket i webbläsaren. |
| `i` | Installerar alla projektberoenden enligt filen `composer.lock`, om den finns och är giltig. Om det finns ett problem används informationen från `composer.json` och `composer.lock` återställs till sitt ursprungliga tillstånd. |
| `info` | Visar information om de installerade paketen i projektet. Visar namnen på alla paket, deras aktuella versioner och en kort beskrivning. |
| `init` | Skapar en grundläggande funktion `composer.json` i den aktuella katalogen. |
| `install` | Installerar alla projektberoenden enligt filen `composer.lock`, om den finns och är giltig. Om ett problem uppstår används informationen från `composer.json` och `composer.lock` återgår till sitt ursprungliga tillstånd. |
| `licenses` | Visar en lista över alla paket, deras versioner och aktuella licenser. |
| `list` | Visar en lista över tillgängliga kommandon. |
| | `outdated` | Visar en lista över alla paket för vilka det finns en nyare version tillgänglig för installation och som uppfyller beroendena. För varje paket visas den senaste kompatibla versionen som Composer föreslår för installation. |
| `prohibits` | Visar vilka paket och beroenden som förhindrar installationen av det begärda paketet eller den begärda versionen. |
| `remove` | Tar bort paketet från konfigurationsavsnittet `require` eller `require-dev`. |
| `require` | Lägger till det begärda paketet i `composer.json` och installerar det. Om beroendena inte kan uppfyllas kommer den att återgå till det ursprungliga tillståndet. |
| `run` | Kör de definierade skripten i `composer.json`. |
| `run-script` | Kör de definierade skripten i `composer.json`. |
| `search` | Söker paket efter nyckelord eller sökfrågor. |
| `self-update` | Uppdaterar den interna `composer.phar` till den senaste versionen. |
| `selfupdate` | Uppdaterar interna `composer.phar` till den senaste versionen. |
| | `show` | Visar detaljerad information om de installerade paketen. |
| `status` | Visar en sammanfattning av lokala ändringar som gjorts manuellt i paket, baserat på en jämförelse med källan för paketet från vilken det ursprungligen installerades. |
| `suggests` | Visar förslag på paket. Förslagen kan omfatta olika typer av åtgärder, t.ex. att installera säkerhetsuppdateringar och så vidare. |
| `update` | Uppdaterar hela projektet i enlighet med beroendena, så att de alltid uppfylls av `composer.json`. Om det lyckas uppdateras `composer.lock`, där de installerade versionerna skrivs in. |
| `upgrade` | Alias för `update`. |
| `u` | Alias till `update`. |
| `validate` | Kontrollerar `composer.json` och `composer.lock` för syntaxfel. |
| `why` | Visar vilka paket som orsakade installationen av det valda paketet, inklusive alla beroenden. |
| `why-not` | Visar vilka paket och versioner som förhindrar installationen av det valda paketet eller den valda versionen. |

Skapa och definiera ett projekt
----------------------------------

Varje projekt som hanteras av Composer definieras av en fil `composer.json` i dess rot som definierar alla beroenden. Filen kan skapas antingen manuellt för ett befintligt projekt eller automatiskt när du skapar ett projekt.

Eftersom allt i Composer är ett paket kan själva projektet baseras på ett paket. Om du till exempel skapar dussintals eller hundratals mycket liknande projekt är det vettigt att lägga deras grundläggande konfiguration och struktur i ett separat paket som du baserar installationen på.

Ett exempel på ett sådant paket är min [Baraja Sandbox] (https://github.com/baraja-core/sandbox), som är baserad på ren Nette 3.0 och lägger till ett grundläggande beroende till min [Package Manager] (https://github.com/baraja-core/package-manager), som jag använder för alla projekt och beroendehantering i Nette-konfigurationen.

Sandlådan installeras sedan enkelt med kommandot:

```shell
$ composer create-project baraja/sandbox <your-project-name>
```

Baserat på projektnamnet skapar Composer automatiskt en mapp för att installera projektet (kopiera paketinnehållet och installera beroendena).

I mappen `vendor` hanterar Composer sedan alla paket (deras fysiska filer finns där) och genererar en autoload av klasser, som vi lämpligen lägger in direkt i `index.php` som en rad:

```php
require __DIR__ . '/vendor/autoload.php';

// Själva programkoden
```

Installera ytterligare paket och beroenden
-------------------------------------

I ett funktionellt projekt kan vi enkelt installera nya paket och lägga till beroenden.

Det finns två sätt att göra detta:

- Med kommandot `composer require ...`,
- Genom att lägga till ett beroende direkt i filen `composer.json` i avsnittet `require` och sedan använda kommandot `composer update`.

Prova att installera till exempel PackageManager: `composer require baraja-core/package-manager`, eller [Doctrine](https://github.com/baraja-core/doctrine): `composer require baraja-core/doctrine`.

Om det valda paketet inte kan installeras kan du fråga efter specifika skäl och Composer listar de beroenden som förhindrar detta. Ofta räcker det med att åtgärda ett beroende av en viss version eller ta bort inkompatibel kod. För detaljerad information, använd kommandot: `composer why baraja-core/doctrine`.

Uppdatering av projekt och paket
-----------------------------

Ett väldesignat projekt utvecklas så att du enkelt kan ladda ner uppdateringar med tiden och alltid har de senaste versionerna av alla paket. Den största fördelen är att du får alla fel rättat, ofta förbättrad prestanda och många nya funktioner. Dessutom gör den gradvisa övergången att det blir mindre komplicerat att uppdatera efter en längre tid, eftersom du löser problem i farten i mindre skala och undviker inkompatibiliteter.

För att uppdatera alla paket och beroenden använder du kommandot `composer update`.

Själva uppdateringsprocessen kan misslyckas i vissa fall. Orsaken är vanligtvis antingen ett trasigt beroende eller ett inkompatibelt paket.

För detaljerad information om varför ett paket inte kan installeras, använd kommandot: `composer why-not baraja-core/doctrine`. Om vi redan har paketet och bara den specifika versionen inte fungerar (den vill inte installeras) kan vi också be om den specifika versionen: `composer why-not baraja-core/doctrine:v1.0.20`.

I filen `composer.json` kan vi också lista beroendet av en specifik körtid. Detta är särskilt användbart när vi utvecklar ett projekt med flera personer och vill kontrollera att de har installerat alla tillägg.

Vanligtvis kontrolleras PHP-versionen (måste vara `7.1.0` eller senare):

```json
{
   "require": {
      "php": ">=7.1.0"
   }
}
```

Eventuellt andra systemtillägg:

```json
{
   "require": {
      "php": ">=7.1.0",
      "ext-json": "*",
      "ext-session": "*",
      "ext-PDO": "*"
   }
}
```

Dessa regler beaktas sedan vid installation av paket eller uppgraderingar. Detta hjälper till att förebygga problem som skulle bli uppenbara vid körning. Typiskt sett behöver till exempel ett paket för en betalningsgateway kommunicera med API:et, så det måste erkänna beroenden av tilläggen `curl` och `json`.

Felsökning av beroenden
-----------------------------

Ofta uppstår beroendeöverträdelser på grund av en dålig version av PHP. I det här fallet skickar Composer ett meddelande, till exempel "Din PHP-version (7.3.11) som åsidosätts av "config.platform.php"-versionen (7.1) uppfyller inte det kravet".

Mycket ofta beror felet på inställningar direkt i projektet `composer.json`, där följande avsnitt finns:

```json
"config": {
   "platform": {
      "php": "7.2"
   }
}
```

Ändringen **måste göras direkt i filen**. När det gäller ett globalt projekt (före installation eller med ett globalt beroende) kan du tvinga fram Composer-versionen med `composer config -g platform.php 7.2.14` (växeln `-g` betyder `global`).

I vissa fall vill vi installera de senaste paketversionerna och ignorera de lokala miljöinställningarna. I det här fallet kan vi använda det avancerade kommandot:

```shell
$ composer update --ignore-platform-reqs
```

**Använd växeln `ignore-platform-reqs` på egen risk, den kan skada projektet!** Använd den inte om du inte förstår konsekvenserna.

Manual Composer-anrop, parametrar och minneshantering
------------------------------------------------------

Composer är egentligen ett PHP-skript som är förpackat i en så kallad PHAR, som är en kompilerad version av en PHP-applikation. Den här informationen kan användas på ett bra sätt, t.ex. för att bättre parametrisera själva anropet.

I riktigt stora projekt händer det ibland att vi får slut på RAM-minne och måste allokera mycket mer, eller öka bearbetningstiden för skripten.

I Windows kan du till exempel använda det här kommandot:

```shell
$ php -d memory_limit=-1 C:/xampp/htdocs/composer.phar update
```

Med växeln `memory_limit=-1` får Composer order att inte vara känslig för RAM-gränsen och att använda så mycket minne som den behöver.

Anpassade användarskript efter Composer-åtgärd
--------------------------------------------

När du har kört Composer-åtgärder kan du aktivera automatisk exekvering av användardefinierade skript som antingen utför specifika åtgärder i projektet eller till exempel genererar en konfiguration efter distributionen. Jag använder främst detta tillvägagångssätt på en lokal server för att erbjuda ett automatiskt verktyg för databaskonfiguration, generera ett databasschema och så vidare.

Vi registrerar de nödvändiga skripten i avsnittet `scripts` i `composer.json`:

```json
"scripts": {
   "post-autoload-dump": "Baraja\\PackageManager\\PackageRegistrator::composerPostAutoloadDump"
}
```

I det här fallet anropas den statiska metoden `composerPostAutoloadDump` i klassen `Baraja\PackageManager\PackageRegistrator` automatiskt. Composer har den här klassen tillgänglig eftersom den först genererade <a href="/autoloading-trid">autoloader-klasser</a>.

Om vi bara vill köra skripten och inte utföra onödiga åtgärder med Composer är kommandot `composer dump` mycket användbart, eftersom det bara genererar en ny autoloader (vilket jag rekommenderar för att alltid hålla den uppdaterad) och sedan kör skripten omedelbart. Om du vill prova att använda skript har jag förberett ett färdigt paket [Baraja PackageManager] (https://github.com/baraja-core/package-manager) som innehåller smarta skript och ett interaktivt gränssnitt för Nette-ramverket.

Versionering av Vendor i Git?
------------------------

En fråga som vi ofta diskuterar med utvecklare är om vi ska versionera innehållet i mappen `vendor` i Git eller om den ska genereras på nytt vid varje installation.

I allmänhet verkar den renare lösningen vara att **inte versionera** innehållet alls och installera allting varje gång. Men verkligheten är att en utvecklare då och då slutar utveckla ett paket eller tar bort det helt och hållet. Den ständiga hämtningen av paket försvårar också lokala installationer och uppdateringar, och det saktar ner distributioner och kan ibland orsaka korta avbrott på webbplatsen när nya paketversioner hämtas felaktigt.

Jag ser Vendors avstängning som en form av "säkerhet". Om filerna finns fysiskt i versionssystemet har vi åtminstone en rudimentär garanti på produktionsservern att paketen kommer att fungera och att deras kod är exakt som den är i den lokalt körda installationen. Dessutom tar Vendor ofta bara några få MB i anspråk, vilket är värt att försäkra sig om en fungerande webbplats med tanke på dagens diskkapacitet.

**Praktisk anmärkning:** För den genomsnittliga webbplats som jag underhåller tar `vendor` inte upp mer än `30 MB`, vilket är en acceptabel kapacitet för Git. När vi klonar ett arkiv med junior behöver vi inte lära honom hur han ska få upp webbplatsen och den fungerar direkt.

Anpassade Composer-paket
-----------------------

Du kan skapa egna paket i Composer, antingen offentligt tillgängliga (registrerade på [Packagist](https://packagist.org/)) eller privata (du måste ha en egen paketeringsserver, till exempel [Satis](https://getcomposer.org/doc/articles/handling-private-packages.md)).

Frågan om att skapa, underhålla, utveckla och versionera paket är mycket komplex och kommer att behandlas i en separat artikel.

Under tiden kan du läsa artikeln [Semantic Versioning] (https://semver.org/lang/cs/), som jag har översatt åt dig.
