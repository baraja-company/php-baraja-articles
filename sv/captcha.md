Hur Captcha fungerar (beskrivande bild)
=======================================

> id: '9cf191e4-a49b-407b-b16e-21de7224ac43'
> slug:
> 	cs: captcha
> 	sv: hur-captcha-fungerar-beskrivande-bild
> 
> perex:
> 	- 'Captcha je druh obrázku, který ověří, že uživatel není robot a chrání webovou aplikaci. Možnosti implementace v PHP.'
> 	- En captcha är en typ av bild som verifierar att användaren inte är en robot och skyddar webbprogrammet. Alternativ för implementering av PHP.
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: '80357b2e7f0047bb488922f2ab7b4336'

Captcha är för närvarande ett av de vanligaste sätten att skydda fria format. Den skapades ursprungligen inte för att skydda datasäkerheten, utan för att skydda mot skräppost och för att inse att det är en människa.

Det är dock maskinellt genererat så det är inte alltid perfekt, men ett captcha-test lyckas med ungefär 99 % och endast 1 % av bilderna kan avkodas av en välgjord skräppostrobot.

Hur det fungerar
--------------------------

Det finns fler alternativ, jag använder till exempel den här lösningen:

- Servern tar emot informationen om att användaren har begärt en blankett och börjar skapa den.
- I det framtida captcha-testfältet placeras en bild med en hemlig adress som inte säger någonting (som ett slumpmässigt tal, en teckensträng, ... vad som helst).
- Bilden skapas och lagras på den adressen. Samtidigt skrivs den korrekta transkriptionen någonstans (kanske till en session eller databas).
- När användaren skickar in formuläret kontrolleras det om utskriften stämmer överens med vad han/hon skrev in. Om så är fallet behandlas formuläret. Om så inte är fallet begärs en ny beskrivning av en annan bild.
- Efter både lyckade och misslyckade försök raderas den tillfälliga captcha-bilden (för att aldrig användas igen). Om formuläret inte skickas in inom en viss tid raderas det också (upphör att gälla).

Korrekt transkribering
--------------------------

Om captcha-testet kan lösas är användaren förmodligen mänsklig. Det är dock bra att tänka på användare som inte kan lösa uppgiften, särskilt blinda användare. Det är en bra lösning för att kombinera flera möjliga tester (särskilt för röstförberedelse). Röstigenkänning av en maskin är dock för närvarande betydligt effektivare än att läsa från en bild. Därför är denna lösning inte alltid idealisk.

Anpassad enkel captcha-bild
--------------------------

Ofta räcker det med att skapa en tom bild med vissa mått och skriva in några tecken i den på ett läsbart sätt utan ytterligare redigering. Allvarligt talat! De flesta skräppostrobotar är dumma och kan inte angripa generiska formulär med den här typen av skydd, trots att det är helt läsbar text som bättre OCR kan transkribera perfekt.

Den resulterande bilden kan se ut så här:

```txt
<img src="captcha.php" alt="ukázková captcha">
```

Försök att uppdatera sidan några gånger och du kommer att se att koden ändras slumpmässigt varje gång. I demonstrationssyfte genereras den bara utan att sparas, så den tas bort omedelbart efter att du har laddat den.

Jag löste källkoden med hjälp av PHPGD-biblioteket, som finns tillgängligt i praktiskt taget alla PHP-installationer och webbhotell:

```php
Header("Innehållstyp: image/png");
$obr = ImageCreate(100, 35);
$pozadi = ImageColorAllocate ($obr, 219, 28, 49); //definition av bakgrundsfärg
$bila = ImageColorAllocate ($obr, 255, 255, 255); //definition av vit färg för text
$styl = array ($pozadi);
ImageSetStyle ($obr, $styl);
  $nahodne_cislo = rand(11111,99999); //dra ett slumpmässigt nummer som är 5 tecken långt
  imagestring($obr, 5, 25, 10, $nahodne_cislo, $bila); //funktion för att rita text (i det här fallet ett nummer)
ImagePNG($obr); //generering av bilden i minnet och rendering
ImageDestroy($obr); //radera bilden från minnet (den kommer inte att behövas längre, eftersom den genereras en gång)
```

Att göra bilden är sedan bara en fråga om HTML:

```html
<img src="captcha.php">
```

Observera att det här skriptet inte fungerar i sig självt utan ytterligare ändringar. Den är endast en demonstration för att generera en enkel bild.

Sammanfattning
--------------------------

Captcha-tester är ganska tillförlitliga, men irriterande och tidskrävande. Ibland är de inte läsbara, så det är bra att ge användaren möjlighet att ladda in en annan bild med hjälp av javascript så att hela sidan inte behöver laddas om och allt måste fyllas i igen.

Kom ihåg: Det som är en trivial uppgift för en människa kan aldrig vara en möjlig lösning för en maskin. Därför kan även denna primitiva captcha med perfekt läsbar text skydda mot mer än hälften av alla skräppostmeddelanden.
