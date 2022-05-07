Inkludera säkerhet i PHP och filtillbehör
=========================================

> id: '820f8de6-ff1e-406c-8fe5-95080642656f'
> slug:
> 	cs: bezpecnost-include
> 	sv: inkludera-saekerhet-i-php-och-filtillbehoer
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '1207d637c20fcb5e8f609be2eb449135'

Ofta vill vi bifoga en fil till en sida som vi har lagrat på en disk någon annanstans. Om vi anger det exakta namnet direkt i attach-funktionen finns det inget att oroa sig för.

Bifoga en fil på ett säkert sätt
--------------------------

```php
include 'menu.html';
```

Den tidigare skrivningen är helt säker eftersom vi alltid monterar samma fil. Ett säkerhetsfel kan inte uppstå i detta fall. Det enda problemet som kan uppstå är att filen **menu.html** inte finns med, vilket ger upphov till ett varningsmeddelande (som förmodligen inte kommer att visas ändå), men det är sällsynt eftersom vi vanligtvis bifogar en fil vars existens är så gott som säker.

Bifoga en fil enligt mönstret
--------------------------

Men vad händer om vi till exempel vill bifoga artiklar till en enkel innehållssida som den här? På den här webbplatsen har jag en fysisk mapp där artiklarna lagras i HTML-format och jag bifogar dem direkt till källkoden.

Det räcker dock inte med att bara ansluta sig! En nybörjare kan kalla enskilda artiklar så här:

```php
include 'artiklar/' . $_GET['Artikel'] . '.html';
```

> Men detta är extremt farligt. En angripare kan skicka en länk till en annan katalog genom att använda `../` eller något liknande som artikelnamn, och ibland är det möjligt att bli av med ändelsen genom att skicka en null-byte i slutet. Du bör åtminstone använda funktionen `basename()`, men det är bättre att endast tillåta värden från whitelistan.

Varför inte ladda in filer som inte är relevanta?
--------------------------

Ofta har vi inget emot att ladda en felaktig (oväntad) fil - det är användarens fel att begära en sida som de egentligen inte vill ha, men det kan finnas situationer där det spelar roll. I synnerhet:

- Användaren laddar en fil som allmänheten inte kan komma åt och som endast servern kan komma åt.
- Om du laddar ett annat PHP-skript kan det utlösa en oväntad åtgärd eller ett felmeddelande som kan ge en antydan om hur webbplatsen fungerar och bidra till ytterligare attacker.
- Om du laddar en annan fil kan den inte bara läggas till i dokumentet, utan också köras.

Whitelist och validering av inmatning
--------------------------

Om vi inte har möjlighet att validera inmatningen på ett säkert sätt (t.ex. från en vitlista) bör vi inte förlita oss på användarens ärlighet och försvara skript, åtminstone på PHP-nivå i alla fall.

Det första viktiga är att placera alla laddade filer i samma mapp (katalog) och att inaktivera vissa farliga tecken, särskilt snedstreck och punkt. Detta gör det omöjligt att komma åt andra mappar som innehåller potentiellt sårbara filer. Du kan också inaktivera farliga tecken genom att helt enkelt ta bort dem från inmatningssträngen.

```php
$load = '../index'; // denna inmatning kan vara potentiellt farlig
$load = strtr($load, './', ''); // tar bort alla punkter och snedstreck från strängen
include $load .'.html';
```

Körande filer?
--------------------------

Det är viktigt att notera att **include**-konstruktionen exekverar filerna när de ansluts på samma sätt som om de vore PHP-kod, så det är en bra idé att ta hänsyn till denna möjlighet.

Ofta bifogar vi dock filer som inte behöver exekveras efteråt och vi är bara intresserade av den lagrade texten (innehållet) i form av en sträng. Därför kan vi läsa in filen i en variabel och arbeta med den som en sträng, vilket är ganska säkert.

```php
$load = '../index'; // denna inmatning kan vara potentiellt farlig
$load = strtr($load, './', ''); // tar bort alla punkter och snedstreck från strängen
$file = file_get_contents($load . '.html'); // Laddning av innehållet i variabeln
echo $file; // dumpa innehållet i filen
```

Lösningen ser vid första anblicken intressant och säker ut - och det är den också. Även om användaren lyckas anropa en PHP-fil kommer den aldrig att köras. Den kan dock visa den (dvs. källkoden) och det måste vi vara försiktiga med.

Känna igen den önskade filen från skriptet
--------------------------

Det finns ingen definitiv guide för detta, utan var och en måste göra det själv enligt manusets behov. Jag känner till exempel igen en artikel från andra filer genom att de innehåller en rubrik av storleken H1. Om någon laddar en fil utan rubrik visar jag ingenting och sidan slutar med ett felmeddelande. Det är alltid viktigt att hitta någon unik egenskap som endast de filer du vill ha har och som de andra filerna inte har, och sedan kan du gå vidare därifrån.

Slutsats
--------------------------

Även om det är relativt enkelt att validera och ladda filer gör många nybörjare fortfarande misstag - och kommer att fortsätta att göra det. Det viktigaste är att förstå innebörden av det vi laddar in och att skilja det innehåll vi vill ha från det övriga. Och viktigast av allt, arbeta med innehållet som en sträng och ladda aldrig in det direkt på sidan.
