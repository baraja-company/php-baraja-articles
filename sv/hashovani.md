Hashing av strängar och lösenord
================================

> id: '7978bee8-62cc-4770-b15b-a8d08d1dcf34'
> slug:
> 	cs: hashovani
> 	sv: hashing-av-straengar-och-loesenord
> 
> perex:
> 	- 'Hash není šifra! Metody hashování dat a hesel. MD5, SHA1, Bcrypt. Ověření hesla.'
> 	- 'Hash är inte ett chiffer! Metoder för att hasha data och lösenord. MD5, SHA1, Bcrypt. Verifiering av lösenord.'
> 
> publicationDate: '2019-09-11 10:13:30'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: f6ea0b06d6ace3c41684a49938f7ce8e

Hashingprocessen (till skillnad från kryptering) producerar en utgång från indata från vilken den ursprungliga strängen inte längre kan härledas.

Den är därför väl lämpad för att skydda känsliga strängar, lösenord och kontrollsummor.

En annan trevlig egenskap hos hashfunktioner är att de alltid genererar utdata av samma längd, och en liten förändring i indata ändrar alltid hela utdata.

Hashing-funktioner
----------------

Det finns många hashfunktioner i PHP, de viktigaste är:

- **Bcrypt: password_hash()** - Den säkraste hashningen av lösenord, långsam i beräkningen, använder internt salt och hashar iterativt.
- **md5()** - Mycket snabb funktion som lämpar sig för hashning av filer. Utdata är alltid 32 tecken.
- **sha1()** - Snabb hashfunktion för filhashning, används internt av Git för hashning av överföringar. Utgången är alltid 40 tecken.

Hashing
-----------

```php
$password = 'hemligt lösenord';

echo password_hash($password); // Bcrypt
echo md5($password);
echo sha1($password);
```

> **Varning:** Varken `md5()` eller `sha1()` är lämpliga för hashning av lösenord, eftersom det är lätt att ta reda på det ursprungliga lösenordet, eller åtminstone att förberäkna lösenorden. Det är mycket bättre att använda `bcrypt`, som utvecklades för hashning av lösenord.
>
> Webbplatsen <a href="https://www.md5cracker.com/">md5cracker.com</a> har en databas med kontrollsummor (hash), prova att söka efter hash: `79c2b46ce2594ecbcbcb5b73e928345492`, som du kan se är ren `md5()` inte så säker för vanliga ord och lösenord.

Den enda korrekta lösningen: `Bcrypt + salt`.
--------------------------------------

I föredraget <a href="https://www.youtube.com/watch?v=F58_A5TM-Sc">Hur man inte gör bort sig i målplanet</a> tog David Grudl upp hur man korrekt hashar och lagrar lösenord.

Den enda korrekta lösningen är: `Bcrypt + salt`.

Särskilt:

```php
$password = 'hash';

// Genererar en säker hash
echo password_hash($password, PASSWORD_BCRYPT);

// Alternativt med högre komplexitet (standard är 10):
echo password_hash($password, PASSWORD_BCRYPT, ['kostnad' => 12]);
```

Fördelen med Bcryp är främst dess hastighet och automatiska saltning.

Det faktum att det tar **lång** tid att generera, till exempel 100 ms, gör det mycket dyrt för en angripare att testa många lösenord.

Dessutom behandlas hashen automatiskt med **tillfälligt salt**, vilket innebär att när samma lösenord hashasas upprepade gånger blir resultatet alltid en annan hash. Därför kan en angripare inte använda en förberäknad hashtabell.

Därför kan vi inte verifiera att lösenordet är korrekt genom upprepad hashning, utan måste anropa en specialiserad funktion:

```php
if (password_verify($password, $hash)) {
    // Lösenordet är korrekt
} else {
    // Lösenordet är felaktigt
}
```

Saltning av lösenord
------------

För att göra det svårare att knäcka en hash är det en bra idé att lägga in ytterligare en sträng i den ursprungliga inmatningen. Helst en slumpmässig. Denna process kallas **password salting**.

Säkerheten bygger på idén att en angripare inte kan använda en förberäknad tabell med lösenord och hash-koder, eftersom han inte känner till saltet och måste knäcka lösenorden individuellt.

Till exempel:

```php
$password = 'secret_passport';
$salt = 'fghjgtzjhg';

$hash = md5($password . $salt);

echo $password; // skriver ut det ursprungliga lösenordet
echo $hash;     // skriver ut lösenordshash inklusive salt
```

Sammansatta hash-funktioner
------------------------

Du kanske tänker att det skulle vara en bra idé att utföra hashfunktionen upprepade gånger, vilket gör det svårare att knäcka den, eftersom det ursprungliga lösenordet måste hashasas upprepade gånger.

Till exempel:

```php
$password = 'lösenord';

for ($i = 0; $i <= 1000; $i++) {
    $password = md5($password);
}

echo $password; // 1000x hashed via md5()
```

Paradoxalt nog minskar svårigheten att bryta igenom eller förblir nästan oförändrad.

Anledningen är att funktionen `md5()` är extremt snabb och kan beräkna över en miljon hash-koder i sekunden på en vanlig dator, så att det inte blir särskilt långsamt att prova lösenord ett och ett.

Det andra skälet är mer en teori, nämligen risken för att stöta på en så kallad kollision. Om vi hashar ett lösenord upprepade gånger kan det med tiden hända att vi hittar en hash som angriparen redan känner till, vilket gör att han kan hasha lösenordet med hjälp av databasen.

Därför är det bättre att använda en långsam och säker hashfunktion och utföra hashningen endast en gång, samtidigt som den slutliga utgången behandlas med saltning.

Extremt säker jämförelse av två hashes/strängar
---------------------------------------------------

Visste du att operatorn === inte är det säkraste valet för hash-jämförelse vid lösenordskontroll?

När strängar jämförs går den igenom de två strängarna tecken för tecken tills den når slutet (framgång, de är samma) eller det inte finns någon skillnad (strängarna är olika).

Detta kan utnyttjas i en attack. Om du mäter tiden tillräckligt noggrant kan du uppskatta hur många tecken som återstår att lägga till för att få en exakt matchning och nå slutet, eller så kan du uppskatta hur långt strängarna har kommit när du jämför strängar.

Lösningen är att använda funktionen hash_equals() när strängar jämförs, och det skulle spela roll om en angripare kunde ta reda på var jämförelsen misslyckades.

Och hur gör funktionen detta? Den ser till att jämförelsen av två strängar alltid tar lika lång tid, så att du inte kan se genom att mäta tiden var skillnaden uppstått. Jag anser att vissa typer av attacker är mycket osannolika och svåra att genomföra. Detta är en av dem.
