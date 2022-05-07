Caesars chiffer - hur det fungerar
==================================

> id: '662dd627-c106-406e-ad6a-7ae9a492ad92'
> slug:
> 	cs: cesarova-sifra
> 	sv: caesars-chiffer---hur-det-fungerar
> 
> publicationDate: '2019-08-23 15:43:02'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '892ad0746be469b5cdbb4de72d8e85b9'

> **Varning:** Den här artikeln skrevs för många år sedan och viss information kan vara föråldrad eller felaktig. Tänk på detta när du läser.

Caesar-krypteringen är en av de enklaste hashfunktionerna. På sin tid var den praktiskt taget omöjlig att bryta, men i moderna datorers tidsålder tar det bara några tiotals sekunder, till och med några minuter, att bryta den. Den bygger på en nyckel, enligt vilken meddelandet krypteras och därefter kan det expanderas igen. Nyckeln är därför hemlig. Vid krypteringstillfället kan meddelandet ses och betyder ingenting (bara ett virrvarr av tecken). Det enda sättet att bryta chiffret är att gissa nyckeln.

Nyckeln
--------------------------

Nyckeln kan vara ett heltal med färre siffror än själva meddelandet. Vanligtvis ges 3 giltiga siffror (så det finns 999 kombinationer). Varje ytterligare siffra ökar säkerheten. För att två parter ska kunna kommunicera måste båda parter känna till den hemliga nyckeln (så de måste på något sätt överföra den till varandra på ett säkert sätt). Om nyckeln är känd av dem och inte av den andra parten kan meddelandet spridas även på ett osäkert sätt utan att innehållet äventyras, eftersom den potentiella angriparen inte känner till hur han eller hon ska få tillbaka meddelandet.

I demonstrationssyfte kommer jag att använda nyckeln **123**, normalt används något annat slumpmässigt nummer som inte antas vara så lätt att gissa.

Krypteringsförfarande
--------------------------

Principen bygger på idén att ersätta tecken i ett meddelande med andra tecken med hjälp av en nyckel. Jag kallar det "karaktärsbyte".

Låt oss till exempel ha ett meddelande som vi vill kryptera:

```php
TAJNA ZPRAVA
```

Tilldela nu ett nummer till varje tecken. Typiskt med alfabet (dess serienummer). Jag kommer att använda det engelska alfabetet, alltså denna serie tecken:

```php
A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
```

Om vi tilldelar varje tecken ett nummer får vi något som liknar:

```php
20-1-10-14-1   26-16-18-1-22-1
```

Nu kommer nyckeln. Vi tar varje enskilt nummer och lägger till nyckeln till det. Jag markerar i färg vad jag har lagt till var:

`1 2 3`

```php
21 3 13 15 3   3 17 20 4 23 3
```

Observera att när jag krypterar **Z**-tecknet skriver jag siffran 3. Detta beror på att **Z** är det sista tecknet i alfabetet, så när det når slutet räknas det igen från början av raden.

Överföring av meddelandet
--------------------------

Nu kan vi skicka meddelandet på vilket sätt som helst. Ibland till och med offentligt. Andra kommer bara att se en ologisk nummerserie, så de kommer förmodligen inte ens att veta att det är någon form av chiffer. Säkerheten förbättras också av själva nyckeln, som är hemlig. Ibland är det lämpligt att överföra meddelandet som en serie siffror, ibland är det lämpligt att omvandla dessa siffror till tecken (återigen enligt samma alfabetiska serie) och sedan överföra en sekvens av tecken. Mycket beror på omständigheterna. I allmänhet är dock en numerisk sekvens bättre, eftersom få människor kommer att misstänka att det är ett kodat meddelande.

Dekryptering hos mottagaren
--------------------------

Mottagaren dekrypterar på samma sätt. Den tar varje enskilt tecken och subtraherar siffrorna enligt nyckeln och omvandlar sedan de resulterande värdena tillbaka till tecken med hjälp av alfabetet. Det är bara ett omvänt krypteringsförfarande. Det viktiga är att känna till **nyckeln** och **teckensättningen** - det vill säga hur tecknen ska ordnas.

Knäckning och dekryptering
--------------------------

Den enda möjliga metoden för att knäcka ett lösenord är att prova alla tänkbara kombinationer av alla möjliga nycklar. Om vi inte känner till nyckelns längd blir hela processen ännu mer komplicerad. Men i allmänhet kan dagens datorer prova ungefär 100 nycklar på en sekund, så en slumpmässig nyckel med tre tecken tar ungefär en minut att knäcka.

Om nyckeln är lika lång eller längre än det ursprungliga meddelandet kan den dock inte brytas eftersom varje enskilt tecken har sin egen nyckel, så alla kombinationer måste prövas för varje tecken.

Om jag hade ett meddelande "**TAKE MESSAGE**" skulle det vara 11 tecken långt (mellanslag räknas inte). Om jag vill ha en nyckel som också är 11 tecken lång, och jag använder ett engelskt alfabet med 26 tecken, så finns det **11^26** = 1.191817654*10²⁷ kombinationer, och den genomsnittliga datorn skulle knäcka den nyckeln på 1.310999419×10²⁶ sekunder = 10^20 dagar :)
